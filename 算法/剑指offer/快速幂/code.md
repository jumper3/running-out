[链接](https://leetcode-cn.com/problems/shu-zhi-de-zheng-shu-ci-fang-lcof/)

``` javascript
/**
 * @param {number} x
 * @param {number} n
 * @return {number}
 */
// 递归快速幂
var myPow = function(x, n) {
    if(x === 1 || x === 0) return x
    if(n === 0) {
        return 1;
    }
    let b = n;
    let a = x;
    if(b < 0) {
        b = -b;
        a = 1/a;
    };
    // js递归直接爆栈了
    // 但是时间复杂度还是logn的
    return powFunction(a, b);
};

const powFunction = (x, n) => {
    if(n === 0) {
        return 1;
    }
    // n >>> 1等价Math.floor(n/2)
    const result = powFunction(x, n >>> 1);
    // n & 1 取数操作，最后一位是0是偶数是1是奇数
    return (n&1) === 1 ? result * result * x : result * result
}
```

``` javascript
/**
 * @param {number} x
 * @param {number} n
 * @return {number}
 */
// 迭代快速幂
var myPow = function(x, n) {
    if(x === 1 || x === 0) return x
    if(n === 0) {
        return 1;
    }
    let b = n;
    let a = x;
    if(b < 0) {
        b = -b;
        a = 1/a;
    };
    // 时间复杂度logn的
    return powFunction(a, b);
};

const powFunction = (x, n) => {
    let res = 1;
    let baseX = x;
    while(n > 0) {
        if(n & 1) {
            res = res * baseX;
        }
        baseX *= baseX;
        n = n>>>1;
        console.log(n);
    }
    return res;
}
```