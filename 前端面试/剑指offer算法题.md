# 剑指offer



## 题目1: [用两个栈实现队列](https://leetcode.cn/problems/yong-liang-ge-zhan-shi-xian-dui-lie-lcof/)

用两个栈实现一个队列。队列的声明如下，请实现它的两个函数 appendTail 和 deleteHead ，分别完成在队列尾部插入整数和在队列头部删除整数的功能。(若队列中没有元素，deleteHead 操作返回 -1 )

来源：力扣（LeetCode）

```js
// 自己的解法发现symbol添加的私有属性执行时间比不用symbol慢了40ms,内存多了0.2m
// 通过 360 ms	53.6 MB	JavaScript	2023/03/11 18:57	不使用symbol
// 通过 404 ms	53.8 MB	JavaScript	2023/03/11 18:54	使用smybol
// 通过 392 ms	52.8 MB	JavaScript	2023/03/11 19:00	官方解答

const _item1=Symbol("STACK1")
const _item2=Symbol("STACK2")
var CQueue = function() {
    this[_item1]=[];
    this[_item2]=[];
    this.in=0;
};

/** 
 * @param {number} value
 * @return {void}
 */
CQueue.prototype.appendTail = function(value) {
    if(this.in===0)
        this[_item1].push(value)
    else{
        let length = this[_item2].length
        for(let i =0;i<length;i++)
            this[_item1].push(this[_item2].pop())
        this[_item1].push(value)
        this.in=0
    }
};

/**
 * @return {number}
 */
CQueue.prototype.deleteHead = function() {
    if(this.in===0 ){
        let length = this[_item1].length
        for(let i =0;i<length;i++)
            this[_item2].push(this[_item1].pop())
        this.in=1
    }
    if(this[_item2].length>0){
        return this[_item2].pop()
    }else
        return -1
};

/**
 * Your CQueue object will be instantiated and called as such:
 * var obj = new CQueue()
 * obj.appendTail(value)
 * var param_2 = obj.deleteHead()
 */
```

```js
##官方的写法
var CQueue = function() {
    this.inStack = [];
    this.outStack = [];
};

CQueue.prototype.appendTail = function(value) {
    this.inStack.push(value);
};

CQueue.prototype.deleteHead = function() {
    if (!this.outStack.length) {
        if (!this.inStack.length) {
            return -1;
        }
        this.in2out();
    }
    return this.outStack.pop();
};

CQueue.prototype.in2out = function() {
    while (this.inStack.length) {
        this.outStack.push(this.inStack.pop());
    }
};

```

## 题目2: [包含min函数的栈](https://leetcode.cn/problems/bao-han-minhan-shu-de-zhan-lcof/)

定义栈的数据结构，请在该类型中实现一个能够得到栈的最小元素的 min 函数在该栈中，调用 min、push 及 pop 的时间复杂度都是 O(1)。

```js
// 不使用Math.min使用min_stack能让时间复杂度到O(1)
// 通过 100 ms	48.8 MB	JavaScript	2023/03/11 19:15	官方的
// 通过 292 ms	48.6 MB	JavaScript	2023/03/11 19:14	我的
/**
 * initialize your data structure here.
 */
 const _item = Symbol('STACK')
var MinStack = function() {
    this[_item]=[]
};

/** 
 * @param {number} x
 * @return {void}
 */
MinStack.prototype.push = function(x) {
    this[_item].push(x)
};

/**
 * @return {void}
 */
MinStack.prototype.pop = function() {
    return this[_item].pop()
};

/**
 * @return {number}
 */
MinStack.prototype.top = function() {
    return this[_item][this[_item].length-1]
};

/**
 * @return {number}
 */
MinStack.prototype.min = function() {
    return Math.min(...this[_item])
};

/**
 * Your MinStack object will be instantiated and called as such:
 * var obj = new MinStack()
 * obj.push(x)
 * obj.pop()
 * var param_3 = obj.top()
 * var param_4 = obj.min()
 */
```

```js
// 官方解答
var MinStack = function() {
    this.x_stack = [];
    this.min_stack = [Infinity];
};

MinStack.prototype.push = function(x) {
    this.x_stack.push(x);
    this.min_stack.push(Math.min(this.min_stack[this.min_stack.length - 1], x));
};

MinStack.prototype.pop = function() {
    this.x_stack.pop();
    this.min_stack.pop();
};

MinStack.prototype.top = function() {
    return this.x_stack[this.x_stack.length - 1];
};

MinStack.prototype.min = function() {
    return this.min_stack[this.min_stack.length - 1];
};
```

## 题目3: [从尾到头打印链表](https://leetcode.cn/problems/cong-wei-dao-tou-da-yin-lian-biao-lcof/)

难度简单389收藏分享切换为英文接收动态反馈

输入一个链表的头节点，从尾到头反过来返回每个节点的值（用数组返回）。

**示例 1：**

```
输入：head = [1,3,2]
输出：[2,3,1]
```

```js
// 通过72 ms	42.9 MB	JavaScript	2023/03/11 19:30	

/**
 * Definition for singly-linked list.
 * function ListNode(val) {
 *     this.val = val;
 *     this.next = null;
 * }
 */
/**
 * @param {ListNode} head
 * @return {number[]}
 */
var reversePrint = function(head) {
    let reverse_head = [],p=head
    while(p){
        reverse_head.unshift(p.val)
        p=p.next
    }
    return reverse_head
};
```

## 题目4: [反转链表](https://leetcode.cn/problems/fan-zhuan-lian-biao-lcof/)

定义一个函数，输入一个链表的头节点，反转该链表并输出反转后链表的头节点。

示例:

输入: 1->2->3->4->5->NULL
输出: 5->4->3->2->1->NULL

```js
/**
 * Definition for singly-linked list.
 *     this.val = val;
 *     this.next = null;
 * }
 */
/**
 * @param {ListNode} head
 * @return {ListNode}
 */
var reverseList = function(head) {
    var new_head = null
    while(head){
        if(new_head===null)
            new_head = new ListNode(head.val)
        else{
            let p = new ListNode(head.val)
            p.next = new_head
            new_head = p
        }
        head=head.next
    }
    return new_head
};

//第二次运行
var reverseList = function (head) {
    var new_head = null
    while (head) {
        let p = new ListNode(head.val)
        p.next = new_head
        new_head = p
        head = head.next
    }
    return new_head
};
```



```js
//官方解答
var reverseList = function(head) {
    let prev = null;
    let curr = head;
    while (curr) {
        const next = curr.next;
        curr.next = prev;
        prev = curr;
        curr = next;
    }
    return prev;
};
```

