[题目链接](https://leetcode-cn.com/problems/ju-zhen-zhong-de-lu-jing-lcof/)
``` javascript
/**
 * @param {character[][]} board
 * @param {string} word
 * @return {boolean}
 */
var exist = function (board, word) {
    const maxX = board.length - 1;
    if (maxX < 0) {
        return false;
    }
    const maxY = board[0].length - 1;
    if (maxY < 0) {
        return false;
    }
    const visted = new Array(board.length).fill(0).map(() => {
        return new Array(board[0].length)
    });
    const dfs = (i, j, k) => {
        if (i < 0 || j < 0 || i > maxX || j > maxY || visted[i][j] || board[i][j] !== word[k]) {
            return false;
        }
        if (k === word.length - 1) {
            return true;
        }
        visted[i][j] = true;
        const res = dfs(i - 1, j, k + 1) || dfs(i, j - 1, k + 1) || dfs(i + 1, j, k + 1) || dfs(i, j + 1, k + 1);
        visted[i][j] = false;
        return res;
    }
    for (let i = 0; i < board.length; i++) {
        for (let j = 0; j < board[i].length; j++) {
            if (dfs(i, j, 0)) {
                return true
            }
        }
    }
    return false;
};
```