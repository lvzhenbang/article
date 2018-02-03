### 试题来源

[https://segmentfault.com/q/1010000009224212](https://segmentfault.com/q/1010000009224212)

### 为什么黑魔法eval()变成了魔鬼

eval是魔鬼（evil）呢，还是JavaScript的黑魔法。很显然很多人将它当作是一个可怕的魔鬼，在开发中，现在好像形成了一种共识，不要再JavaScript代码中使用eval()。这是为什么呢？

1. eval存在安全性问题。使用它可能会造成XSS攻击，例如：我们在ajax请求时使用eval，而此时如果存在攻击的话只需要修改eval中的代码就会造成难以估量的损失。

2. 使用eva()会严重降低我们客户端的性能。使用于不使用区别很大（非现代浏览器），虽然现代浏览器提供的两种编译模式fast path和 slow path。而我们前面说的黑魔法就是eval()走的是fast path，对于eval()传递的参数计算是快了，但却消耗了浏览器的资源，虽说现代浏览器解决了这个问题，但它的安全问题还是被人诟病的。

3. 开发者不会使用（这个原因也占一定比重）

所以，就有了对字符串类型的数学表达式的合法计算算法题目。

### [逆波兰表达式](https://baike.baidu.com/item/%E9%80%86%E6%B3%A2%E5%85%B0%E8%A1%A8%E8%BE%BE%E5%BC%8F/9841727?fr=aladdin)

使用调度场算法将操作符的中缀表示法，改编为后缀表示形式。

```
// 表达式
(1+2)/4+5+(3+5)*3 
// 中缀形式
1 2 + 4 / 5 3 5 + 3 * +
// 后缀形式
1 2 + 4 / 5 + 3 5 + 3 *
```

代码实例：

```
var A = '((112 + 2) * (32 + (43 + 45 - 46) * 12))';
// 判断字符是否为运算符
function is_op(val) {
    var op_string = '+-*/()';
    return op_string.indexOf(val) > -1;
}

// 定义表达式运算符的优先级
function op_level (op) {
    if (op == '+' || op == '-') {
        return 0;
    }
    if (op == '*' || op == '/') {
        return 1;
    }
    if (op == '(') {
        return 3;
    }
    if (op == ')') {
        return 4;
    }
}
// 去除'()'运算符，初始化生成一个中缀式的表达式
function init_expression(expression) {
    var expression = expression.replace(/\s/g, ''),
        input_stack = [];
    input_stack.push(expression[0]);
    for (var i = 1; l = expression.length, i<l; i++) {
        if (is_op(expression[i]) || is_op(input_stack.slice(-1))) {
            input_stack.push(expression[i]);
        } else {
            input_stack.push(input_stack.pop()+expression[i]);
        }
    }
    return input_stack;
}
// 由于逆波兰表达式是从左往右计算的所以，需要将中缀式的表达式转换为后缀式的表达式
function RPN (input_stack) {
    var out_stack = [], op_stack = [], match = false, tmp_op;
    while (input_stack.length > 0 ) {
        var sign = input_stack.shift();
        if (!is_op(sign)) {
            out_stack.push(sign);
        } else if (op_level(sign) == 4) {
            match = false;
            while (op_stack.length > 0 ) {
                tmp_op = op_stack.pop();
                if ( tmp_op == '(') {
                    match = true;
                    break;
                } else {
                    out_stack.push(tmp_op);
                }
            } 
            if (match == false) {
                return 'lack left';
            }
        } else {
            while ( op_stack.length > 0 && op_stack.slice(-1) != '(' && op_level(sign) <= op_level(op_stack.slice(-1))) {
                out_stack.push(op_stack.pop());
            }
            op_stack.push(sign);   
        }
    }
    while (op_stack.length > 0 ){
        var tmp_op = op_stack.pop();
        if (tmp_op != '(') {
            out_stack.push(tmp_op);
        } else {
            return 'lack right';
        }
    }
    return out_stack;
}
// 计算新生成的表达式数组
function cal(expression) {
    var i, j, 
        RPN_exp = [],
        ans;
    while (expression.length > 0) {
        var sign = expression.shift();
        if (!is_op(sign)) {
            RPN_exp.push(sign);
        } else {
            j = parseFloat(RPN_exp.pop());
            i = parseFloat(RPN_exp.pop());
            RPN_exp.push(cal_two(i, j, sign));
        }
    }
    return RPN_exp[0];
}
// 计算按顺序出栈的两个非操作符的元素
function cal_two(i, j, sign) {
    switch (sign) {
        case '+':
            return i + j;
            break;
        case '-':
            return i - j;
            break;
        case '*':
            return i * j;
            break;
        case '/':
            return i / j;
            break;
        default:
            return false;
    }
}


var expression = RPN(init_expression(A))
if (expression == 'lack left' || expression == 'lack right') {
    console.log(expression);
} else {
    console.log(cal(expression));
}
```
这种实现的算法严重的浪费资源，消耗了浏览器的性能，并不可取。

### 回归eval()

v8引擎来了性能问题已经不再是问题，至于开发者不会使用eval造成的问题，只要你明确eval()的使用场景即可（包括eval的安全性）。

eval()被人称为魔鬼的原因是开发人员使用它做了一些不该做的事情。这里有篇文章[eval是魔鬼](http://www.nowamagic.net/librarys/veda/detail/1627)。

但是也有一篇文章说[eval()是魔法](https://www.nczonline.net/blog/2013/06/25/eval-isnt-evil-just-misunderstood/)

那么，我们应该如何使用eval呢？这里我总结如下几点：
