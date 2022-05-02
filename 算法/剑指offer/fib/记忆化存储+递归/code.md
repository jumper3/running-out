[链接](https://leetcode-cn.com/problems/fei-bo-na-qi-shu-lie-lcof/)
``` javascript
const fibArray = [];
var fib = function(n) {
    fibArray[0] = 0;
    fibArray[1] = 1;
    fibArray[2] = 1;
    return dpFib(n, fibArray);
};
let dpFib = (n, fibArray) => {
    if(fibArray[n] != undefined && typeof(fibArray[n]) === 'number') {
        return fibArray[n];
    }
    fibArray[n] = dpFib(n-1, fibArray) + dpFib(n-2, fibArray);
    fibArray[n] = fibArray[n] % 1000000007; // 题目要求取模1e9+7
    return fibArray[n];
}
```