[链接](https://leetcode-cn.com/problems/yong-liang-ge-zhan-shi-xian-dui-lie-lcof/)

``` javascript
var CQueue = function() {
    this.inStack = []; // 入栈
    this.outStack = []; // 出栈
};

/** 
 * @param {number} value
 * @return {void}
 */
CQueue.prototype.appendTail = function(value) {
    this.inStack.push(value);
};

/**
 * @return {number}
 */
CQueue.prototype.deleteHead = function() {
    // 实际就是让出栈保持入栈的reverse
    // 这样pop reverse就是shift了
    if(!this.inStack.length && !this.outStack.length) {
        return -1;
    }
    if(!this.outStack.length) {
        while(this.inStack.length) {
            this.outStack.push(this.inStack.pop());
        }
    }
    return this.outStack.pop();
};
```