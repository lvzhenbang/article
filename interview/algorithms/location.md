### 问题来源

[`https://segmentfault.com/q/1010000013091395?_ea=3284779`](https://segmentfault.com/q/1010000013091395?_ea=3284779)

### 问题描述：

存在一个0，1值的二维数组，给定一个坐标[x,y]，如果该坐标所代表的元素值为1，则返回该坐标所代表的元素相邻的所有值为1的元素坐标。

### 解题思路

对于这种查找元素这类题目，脑袋里的第一个想法就是应该使用遍历。然后选择使用何种遍历，由于这个查找元素是跟位置有关的，所以使用宽度遍历（宽度遍历的定义）最合适。

[宽度遍历](http://book.51cto.com/art/201012/236668.htm)：宽度优先遍历，是以离初态距离为序进行遍历。

### 解题方案

初始化定义：

* queue: 一个临时的缓存队列，存储临时匹配的结果
* result： 一个结果数组，存储所有的匹配结果
* memo：一个原数组的元素的记忆数组，如果存在记忆为true，初始值全为false

```
// 定义一个遍历数组
var arr =[
    [0,0,0,0,0,0,0,0,0,0,0], 
    [0,0,0,0,0,0,0,0,0,0,0], 
    [0,0,0,0,0,0,0,0,0,0,0], 
    [0,0,0,0,1,0,0,0,1,0,0], 
    [0,0,0,0,1,0,0,0,1,0,0],
    [0,0,0,0,1,0,0,0,1,0,0],
    [0,0,0,0,1,0,0,0,0,0,0],
    [0,0,0,0,1,0,0,0,0,0,0],
    [0,0,0,0,1,1,1,0,0,0,0],
    [0,0,0,0,0,0,0,0,0,0,0],
    [0,0,0,0,0,0,0,0,0,0,0],
]

// 声明一个遍历方法
function fn ([x, y]) {
    // 定义一个缓存队列queue，存储临时匹配的结果
    const queue = []
    // 定义一个result，存储所有的匹配结果
    const result = []
    // 定义一个原数组的元素的记忆数组，初始值为false，如果原数组的元素元素存在则记忆值变为true，
    const memo = arr.map(row => new Array(row.length).fill(false))
    // 定义一个方向数组，它的元素值分别表示左、右、上、下
    const direction = [[-1, 0], [1, 0], [0, -1], [0, 1]]

	// 如果指定位置元素值不为1，直接返回false，跳出查询函数;
	// 如果存在，则将位置结果推入临时数组queue，和结果数组result
    if (arr[x][y] !== 1) {
    	return false
    } else {
    	queue.push([x, y])
    	result.push([x, y])
    }

    // 临时存储结果数组中是否有元素，如果有，则进行循环；如果没有，则跳出while循环，执行其它语句
    while(queue.length > 0) {
    	// 从缓存队列中取出存在元素的坐标
        const [x, y] = queue.pop()
        // 查找该坐标位置左右上下位置值为1的元素，如果存在且记忆数组没有记忆过该元素，那么就将用memo记忆该元素，
        // 然后推入临时数组queue和结果数组，然后结束本次循环，接着返回循环条件判断，看是否接着执行循环，如果执行条件满足，重复循环体内的执行语句
        direction.forEach(([h, v]) => {
            const newX = x + h
            const newY = y + v
            if (arr[newX][newY] === 1 && !memo[newX][newY]) {
                memo[newX][newY] = true
                queue.push([newX, newY])
                result.push([newX, newY])
            }
        })
    }
    
    return result
}

fn([3, 4])
```

根据JavaScript的特性，可以对算法进行优化，对于上例的记忆数组，我们可以使用对象来处理，这样可以减小初始化的开销。代码如下：

```
function fn([x, y]) {
    var memo = {}, // 将记忆数组改为记忆对象
    	queue = [],
    	result = [],
    	direction = [[-1, 0],[1, 0],[0, -1],[0, 1]]

	if (arr[x][y] !== 1) {
    	return false
    } else {
    	queue.push([x, y])
    	result.push(memo[x + "," + y] = [x, y])
    }

    while(queue.length > 0) {
    	const [x, y] = queue.pop()
        direction.forEach(([h, v]) => {
            const newX = x + h
            const newY = y + v
            if (arr[newX][newY] === 1 && !memo[newX + "," + newY]) {
                queue.push([newX, newY]);
                result.push(memo[x + "," + y] = [newX, newY]);
            }
        })
    }
    return result;
}
```

当然，对于遍历如果我们使用递归方法的代码的书写量将会减少不少。可以将代码修改如下：

```
function fn(point) {
    var memo = {},
    	result = [],
    	direction = [[-1, 0],[1, 0],[0, -1],[0, 1]]
    function dg([x, y]) {
        result.push(memo[x + "," + y] = [x, y]);
        direction.forEach(([h, v]) => {
            const newX = x + h
            const newY = y + v
            if (arr[newX][newY] === 1 && !memo[newX + "," + newY]) {
                dg([newX, newY]);
            }
        })
    }
    dg(point);
    return result;
}
```

好了，今天宽度遍历算法的就先说到这里，后续可能还会继续修改，也欢迎各位批评指正。有问题或者有其他想法的可以在我的[GitHub](https://github.com/lvzhenbang/article)上pr。