## 观察者设计模式

### 定义

观察者设计模式中有一个对象（被称为subject）根据观察者（observer）维护一个对象列表，自动通知它们对状态的任何修改。

当一个subject要通知观察者一些有趣的事情时，它会向观察者发送通知（它可以包含于通知主题相关的特定数据）

当我们不在希望某一特定的观察员被通知它们所登记的主题变化时，这个主题可以将他们从观察员名单上删除。

为了从整体上了解设计模式的用法和优势，回顾已发布的设计模式通常是有用的，这些设计模式的定义与语言无关。在GoF这本书中，观察者设计模式是这样定义的：
	
	“一个或多个观察者对某一subject的状态感兴趣，并通过附加它们自己来注册它们对该主题的兴趣。当观察者可能感兴趣的主题发生变化时，会发送一个通知信息，该通知将调用们个观察者中的更新方法。当观察者不再对主题的状态感兴趣时，他们可以简单地分离自己。”

### 组成

扩展我们所学，以一下的组件实现observer模式：

主题：维护一个观察这里表，方便添加或删除观察者

observer：为需要通知对象更改状态的对象提供一个更新接口

实际主题：向观察者发送关于状态变化的通知，存储实际观察者的状态

实际观察者：存储引用到的实际主题，为观察者实现一个更新接口，以确保状态与主题的一致。


### 实现

1.对一个subject可能拥有的观察者列表进行建模：

	function ObserverList(){
	  this.observerList = [];
	}
	 
	ObserverList.prototype.add = function( obj ){
	  return this.observerList.push( obj );
	};
	 
	ObserverList.prototype.count = function(){
	  return this.observerList.length;
	};
	 
	ObserverList.prototype.get = function( index ){
	  if( index > -1 && index < this.observerList.length ){
	    return this.observerList[ index ];
	  }
	};
	 
	ObserverList.prototype.indexOf = function( obj, startIndex ){
	  var i = startIndex;
	 
	  while( i < this.observerList.length ){
	    if( this.observerList[i] === obj ){
	      return i;
	    }
	    i++;
	  }
	 
	  return -1;
	};
	 
	ObserverList.prototype.removeAt = function( index ){
	  this.observerList.splice( index, 1 );
	};

2.对subject进行建模，并在观察者列表中补充添加、删除、通知观察者的方法

	function Subject(){
	  this.observers = new ObserverList();
	}
	 
	Subject.prototype.addObserver = function( observer ){
	  this.observers.add( observer );
	};
	 
	Subject.prototype.removeObserver = function( observer ){
	  this.observers.removeAt( this.observers.indexOf( observer, 0 ) );
	};
	 
	Subject.prototype.notify = function( context ){
	  var observerCount = this.observers.count();
	  for(var i=0; i < observerCount; i++){
	    this.observers.get(i).update( context );
	  }
	};

3.为创建一个新的观察者定义一个框架。框架中的update功能将被稍后的自定义行为覆盖

	// The Observer
	function Observer(){
	  this.update = function(){
	    // ...
	  };
	}

### 示例

使用上的观察者组件，我们做一个如下面定义的demo：

[] 在页面中添加新的可观察复选框的按钮；

[] 一个控制复选框将作为一个subject，通知其它的复选框，它们应该被检查；

[] 正在被添加的复选框容器

然后，我们定义实际的主题和实际的观察者处理句柄，以便为页面添加新的观察者并实现更新接口。

实例代码如下：

html

	<button id="addNewObserver">Add New Observer checkbox</button>
	<input id="mainCheckbox" type="checkbox"/>
	<div id="observersContainer"></div>

js

	// Extend an object with an extension
	function extend( obj, extension ){
	  for ( var key in extension ){
	    obj[key] = extension[key];
	  }
	}
	 
	// References to our DOM elements
	 
	var controlCheckbox = document.getElementById( "mainCheckbox" ),
	  addBtn = document.getElementById( "addNewObserver" ),
	  container = document.getElementById( "observersContainer" );
	 
	 
	// Concrete Subject
	 
	// Extend the controlling checkbox with the Subject class
	extend( controlCheckbox, new Subject() );
	 
	// Clicking the checkbox will trigger notifications to its observers
	controlCheckbox.onclick = function(){
	  controlCheckbox.notify( controlCheckbox.checked );
	};
	 
	addBtn.onclick = addNewObserver;
	 
	// Concrete Observer
	 
	function addNewObserver(){
	 
	  // Create a new checkbox to be added
	  var check = document.createElement( "input" );
	  check.type = "checkbox";
	 
	  // Extend the checkbox with the Observer class
	  extend( check, new Observer() );
	 
	  // Override with custom update behaviour
	  check.update = function( value ){
	    this.checked = value;
	  };
	 
	  // Add the new observer to our list of observers
	  // for our main subject
	  controlCheckbox.addObserver( check );
	 
	  // Append the item to the container
	  container.appendChild( check );
	}

在这个示例中我们研究了如何实现和使用观察者模式，涵盖了主题(subject), 观察者(observer)，实际/具体对象(ConcreteSubject)，实际/具体观察者(ConcreteObserver)

实例演示：[demo](https://jsfiddle.net/sanlv/mgagzo0r/)

### 观察者和发布者订阅模式之间的差异

最然，观察者模式很有用，但是在JavaScript中我们经常会用一种被称为发布/订阅模式的变体来实现。虽然它们很相似，但是这些模式之间还是有区别的。

观察者模式要求希望接受主题通知的观察者（或对象）必须订阅该对象触发事件的对象（主题）

然而，发布/订阅模式使用一个主题/事件通道，该通道位于希望接受通知（订阅者）和触发事件（发布者）的对象之间。此事件系统允许代码定义特定用于应用程序的事件，这些事件可以通过自定义参数来传递订阅者所需的值。这样的思路是为了避免订阅者和发布者的依赖关系。

与观察者模式不同，它允许任何订阅者实现一个适当的事件处理程序来注册并接收发布者发布的主题通知。

下面一个例子，如果提供了功能实现，如何使用发布/订阅模式，可以在幕后支持publish()，subscribe()，unsubscribe()

	// A very simple new mail handler
 
	// A count of the number of messages received
	var mailCounter = 0;
	 
	// Initialize subscribers that will listen out for a topic
	// with the name "inbox/newMessage".
	 
	// Render a preview of new messages
	var subscriber1 = subscribe( "inbox/newMessage", function( topic, data ) {
	 
	  // Log the topic for debugging purposes
	  console.log( "A new message was received: ", topic );
	 
	  // Use the data that was passed from our subject
	  // to display a message preview to the user
	  $( ".messageSender" ).html( data.sender );
	  $( ".messagePreview" ).html( data.body );
	 
	});
	 
	// Here's another subscriber using the same data to perform
	// a different task.
	 
	// Update the counter displaying the number of new
	// messages received via the publisher
	 
	var subscriber2 = subscribe( "inbox/newMessage", function( topic, data ) {
	 
	  $('.newMessageCounter').html( ++mailCounter );
	 
	});
	 
	publish( "inbox/newMessage", [{
	  sender: "hello@google.com",
	  body: "Hey there! How are you doing today?"
	}]);
	 
	// We could then at a later point unsubscribe our subscribers
	// from receiving any new topic notifications as follows:
	// unsubscribe( subscriber1 );
	// unsubscribe( subscriber2 );

它的用来促进松散耦合。它们不是直接调用其他对象的方法，而是订阅另一个对象的特定任务或活动，并在发生改变时得到通知。

### 优势

观察者和发布/订阅模式鼓励我们认真考虑应用程序的不同部分之间的关系。他们还帮助我们确定那些层次包含了直接关系，而那些层次则可以替换为一系列的主题和观察者。这可以有效地将应用程序分解为更小的、松散耦合的块，以改进代码管理和重用潜力。使用观察者模式的进一步动机是，我们需要在不适用类紧密耦合的情况下保持相关对象间的一致性。例如，当对象需要能够通知其他对象是，不需要对这些对象进行假设。

在使用任何模式时，观察者和主题之间都可以存在动态关系。这题懂了很大的灵活性，当我们的应用程序的不同部分紧密耦合时，实现的灵活性可能不那么容易实现。

虽然它不一定是解决所有问题的最佳方案，但这些模式仍然是设计解耦系统的最佳工具之一，并且应该被认为是任何javascript开发人员的工具链中最重要的工具。

### 劣势

这些模式的一些问题主要源于他们的好处。在发布/订阅模式中，通过将发布者与订阅者分离，有时很保证我们的应用程序的某些特定部分可以像我们预期的那样运行。

例如，发布者可能会假设一个或多个订阅者正在监听他们。假设我们使用这样的假设来记录或输出一些应用程序的错误。如果执行日志记录崩溃的订阅者(或者由于某种原因不能正常运行)，那么由于系统的解耦特性，发布者将无法看到这一点。

这种情况的另一种说法是，用户不知道彼此的存在，对交换发布者的成本视而不见。由于订阅者和发布者之间的动态关系，更新依赖关系可能很难跟踪。

### 发布/订阅模式的实现

发布/订阅在JavaScript生态系统中很适用，这在很大程度上是因为在核心的ECMAScript实现是事件驱动的。在浏览器环境中尤其如此，因为DOM将事件作为脚本的主要交互API。

也就是说，ECMAScript和DOM都不提供在实现代码中创建自定义事件系统的核心对象或方法(可能只有DOM3 CustomEvent，它是绑定到DOM的，不是通用)。

幸运的是，流行的JavaScript库，如dojo、jQuery(自定义事件)和YUI已经有了一些实用工具，它们可以帮助轻松实现发布/订阅系统。下面我们可以看到一些例子：

	var pubsub = {};
 
	(function(myObject) {
	 
	    // Storage for topics that can be broadcast
	    // or listened to
	    var topics = {};
	 
	    // A topic identifier
	    var subUid = -1;
	 
	    // Publish or broadcast events of interest
	    // with a specific topic name and arguments
	    // such as the data to pass along
	    myObject.publish = function( topic, args ) {
	 
	        if ( !topics[topic] ) {
	            return false;
	        }
	 
	        var subscribers = topics[topic],
	            len = subscribers ? subscribers.length : 0;
	 
	        while (len--) {
	            subscribers[len].func( topic, args );
	        }
	 
	        return this;
	    };
	 
	    // Subscribe to events of interest
	    // with a specific topic name and a
	    // callback function, to be executed
	    // when the topic/event is observed
	    myObject.subscribe = function( topic, func ) {
	 
	        if (!topics[topic]) {
	            topics[topic] = [];
	        }
	 
	        var token = ( ++subUid ).toString();
	        topics[topic].push({
	            token: token,
	            func: func
	        });
	        return token;
	    };
	 
	    // Unsubscribe from a specific
	    // topic, based on a tokenized reference
	    // to the subscription
	    myObject.unsubscribe = function( token ) {
	        for ( var m in topics ) {
	            if ( topics[m] ) {
	                for ( var i = 0, j = topics[m].length; i < j; i++ ) {
	                    if ( topics[m][i].token === token ) {
	                        topics[m].splice( i, 1 );
	                        return token;
	                    }
	                }
	            }
	        }
	        return this;
	    };
	}( pubsub ));

简单实现如下：

	// Return the current local time to be used in our UI later
	getCurrentTime = function (){
	 
	   var date = new Date(),
	         m = date.getMonth() + 1,
	         d = date.getDate(),
	         y = date.getFullYear(),
	         t = date.toLocaleTimeString().toLowerCase();
	 
	        return (m + "/" + d + "/" + y + " " + t);
	};
	 
	// Add a new row of data to our fictional grid component
	function addGridRow( data ) {
	 
	   // ui.grid.addRow( data );
	   console.log( "updated grid component with:" + data );
	 
	}
	 
	// Update our fictional grid to show the time it was last
	// updated
	function updateCounter( data ) {
	 
	   // ui.grid.updateLastChanged( getCurrentTime() );
	   console.log( "data last updated at: " + getCurrentTime() + " with " + data);
	 
	}
	 
	// Update the grid using the data passed to our subscribers
	gridUpdate = function( topic, data ){
	 
	  if ( data !== undefined ) {
	     addGridRow( data );
	     updateCounter( data );
	   }
	 
	};
	 
	// Create a subscription to the newDataAvailable topic
	var subscriber = pubsub.subscribe( "newDataAvailable", gridUpdate );
	 
	// The following represents updates to our data layer. This could be
	// powered by ajax requests which broadcast that new data is available
	// to the rest of the application.
	 
	// Publish changes to the gridUpdated topic representing new entries
	pubsub.publish( "newDataAvailable", {
	  summary: "Apple made $5 billion",
	  identifier: "APPL",
	  stockPrice: 570.91
	});
	 
	pubsub.publish( "newDataAvailable", {
	  summary: "Microsoft made $20 million",
	  identifier: "MSFT",
	  stockPrice: 30.85
	});

用户接口通知

接下来我们假设有一个web应用程序负责显示实时股票信息。

应用程序可能有一个网格用于显示股票统计数据和显示最新更新点的计数器。当数据模型发生变化时，应用程序将需要更新网格和计数器。在这个场景中，我们的主题(将发布主题/通知)是数据模型，我们的订阅者是网格和计数器。

当我们的订阅者收到通知时，模型本身已经更改，他们可以相应地更新自己。

在我们的实现中，我们的订阅用户将收主题“newDataAvailable”，以了解是否有新的股票信息可用。如果一个新的通知发布到这个主题，它将触发gridUpdate向包含该信息的网格添加一个新的行。它还将更新上一次更新的计数器，以记录上一次添加的数据

	// Return the current local time to be used in our UI later
	getCurrentTime = function (){
	 
	   var date = new Date(),
	         m = date.getMonth() + 1,
	         d = date.getDate(),
	         y = date.getFullYear(),
	         t = date.toLocaleTimeString().toLowerCase();
	 
	        return (m + "/" + d + "/" + y + " " + t);
	};
	 
	// Add a new row of data to our fictional grid component
	function addGridRow( data ) {
	 
	   // ui.grid.addRow( data );
	   console.log( "updated grid component with:" + data );
	 
	}
	 
	// Update our fictional grid to show the time it was last
	// updated
	function updateCounter( data ) {
	 
	   // ui.grid.updateLastChanged( getCurrentTime() );
	   console.log( "data last updated at: " + getCurrentTime() + " with " + data);
	 
	}
	 
	// Update the grid using the data passed to our subscribers
	gridUpdate = function( topic, data ){
	 
	  if ( data !== undefined ) {
	     addGridRow( data );
	     updateCounter( data );
	   }
	 
	};
 
	// Create a subscription to the newDataAvailable topic
	var subscriber = pubsub.subscribe( "newDataAvailable", gridUpdate );
	 
	// The following represents updates to our data layer. This could be
	// powered by ajax requests which broadcast that new data is available
	// to the rest of the application.
	 
	// Publish changes to the gridUpdated topic representing new entries
	pubsub.publish( "newDataAvailable", {
	  summary: "Apple made $5 billion",
	  identifier: "APPL",
	  stockPrice: 570.91
	});
 
	pubsub.publish( "newDataAvailable", {
	  summary: "Microsoft made $20 million",
	  identifier: "MSFT",
	  stockPrice: 30.85
	});