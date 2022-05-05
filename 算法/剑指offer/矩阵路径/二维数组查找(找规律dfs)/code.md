[题目链接](https://leetcode-cn.com/problems/er-wei-shu-zu-zhong-de-cha-zhao-lcof/)

``` javascript
var findNumberIn2DArray = function (matrix, target) {   
        // 超时解法     
        // 暴力搜索
        // 从0，0开始，每一次都搜索下、右两个方向，如果比自己小就移动下标

        const maxX = matrix.length - 1;
        if (maxX === -1) {
            return false;
        }
        const maxY = matrix[0].length - 1;
        if (maxY === -1) {
            return false;
        }
        let isFind = false;
        const findNumber = (x, y) => {
            if (isFind) {
                return;
            }
            if (matrix[x][y] === target) {
                isFind = true;
                return;
            }
            if(x + 1 > maxX && y + 1 > maxY) {
                return;
            }
            if (x + 1 <= maxX) {
                if (target >= matrix[x + 1][y]) {
                    findNumber(x + 1, y);
                }
            }
            if (y + 1 <= maxY) {
                if (target >= matrix[x][y + 1]) {
                    findNumber(x, y + 1);
                }
            }
        }
        findNumber(0,0);
        return isFind;
    };
```

``` javascript
var findNumberIn2DArray = function (matrix, target) {
        // 从右上角开始
        // 如果当前值比目标值大，则往左移动（y--）
        // 如果当前值比目标值小，则往下移动（x++）
        const maxX = matrix.length - 1;
        if (maxX === -1) {
            return false;
        }
        const maxY = matrix[0].length - 1;
        if (maxY === -1) {
            return false;
        }
        let isFind = false;
        let x = 0;
        let y = maxY;
        while (x <= maxX && y >= 0) {
            if (matrix[x][y] === target) {
                isFind = true;
                break;
            }
            if (matrix[x][y] > target) {
                y--;
            } else {
                x++;
            }
        }
        return isFind;
    };
```

``` javascript
```