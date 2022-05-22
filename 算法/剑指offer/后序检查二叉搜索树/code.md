[题目链接](https://leetcode.cn/problems/er-cha-sou-suo-shu-de-hou-xu-bian-li-xu-lie-lcof/)

```javascript
/**
 * @param {number[]} postorder
 * @return {boolean}
 */
var verifyPostorder = function (postorder) {
    if (postorder.length < 2) {
        // 单值必然满足二叉搜索树
        return true;
    }
    // 后序遍历中，最后一个值必然为root，倒数第二值必然为root->右树root || 左树root(当右树为空时)
    // 后序遍历中，永远是先后序遍历完左树，再遍历右树的
    // 由此可知进入右树遍历时，他的前一个节点就是左树根节点
    // 二叉搜索树中，左树任意节点必然小于根节点，右树任意节点必然大于根节点
    // 则找到右树root左侧第一个大于root的值，标志着进入右树的遍历，检查之后所有节点是否大于root
    // 如果不大于则二叉搜索树不满足，如果全部大于则递归检查root的左树和右树

    const verifyBFT = (root, left) => {
        const right = root - 1;
        const rootVal = postorder[root];
        console.log(root, left, right)
        if(right < 0 || left < 0 || right <= left) {
            // 区间不合法，则代表没有树或者单节点
            return true;
        }
        let k = left;
        while (k < root, postorder[k] < rootVal) {
            // 找到区间里大于root的第一个值，标志右树进入
            k++
        }
        for(let i = k; i < root; i++) {
            // 右树节点都不能比root小
            if(postorder[i] < rootVal) return false;
        }
        // 检查左树
        if(!verifyBFT(k-1,left)) {
            return false;
        };
        // 检查右树
        if(!verifyBFT(right,k)) {
            return false;
        }
        return true;
    }
    return verifyBFT(postorder.length - 1, 0);
};
```

``` javascript
/**
 * @param {number[]} postorder
 * @return {boolean}
 */
var verifyPostorder = function (postorder) {
    if (postorder.length < 2) {
        // 单值必然满足二叉搜索树
        return true;
    }
    // 单调栈
    // 特性是二叉树反向后序中，单调递增的永远出现在右树
    // 所以从后往前遍历，单调递增的压入栈内
    // 遇到第一个递减（比前一个小）的节点k，则他是左树的节点，从单调栈内找到比他大的最小值做他的父节点j（右树特性是所有的节点都大于根节点，则这个k只能做最小的右树节点的左树）
    // 然后开始下一轮压栈（寻找递增）
    // 如果寻找过程中出现有任意值大于父节点j，则认为不满足二叉搜索树
    let parent = Number.MAX_SAFE_INTEGER;
    let helperStack = [];
    for(let i = postorder.length - 1; i>=0 ; i--) {
        if(postorder[i] > parent) {
            return false;
        }
        while(helperStack.length && helperStack[helperStack.length - 1] > postorder[i]) {
            parent = helperStack.pop();
        }
        helperStack.push(postorder[i]);
    }
    return true;
};
```