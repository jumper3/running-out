[原题链接](https://leetcode-cn.com/problems/ji-qi-ren-de-yun-dong-fan-wei-lcof/)
``` javascript
var movingCount = function (m, n, k) {
    if (m <= 0 || n <= 0) {
        return 0
    }
    const visited = new Array(m).fill(0).map(() => new Array(n));
    const dfs = (i, j) => {
        if (i < 0 || j < 0 || i > m - 1 || j > n - 1 || visited[i][j]) {
            // 越界或者已经走过了
            return 0;
        }
        if ((Math.floor(i / 10) + Math.floor(j / 10) + i % 10 + j % 10) > k) {
            // 超出k值
            return 0;
        }
        visited[i][j] = true;
        return 1 + dfs(i + 1, j) + dfs(i - 1, j) + dfs(i, j + 1) + dfs(i, j - 1);
    };
    return dfs(0,0);
};
```