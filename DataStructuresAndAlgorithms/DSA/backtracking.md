# 回溯算法

使用回溯法进行求解，回溯是一种通过穷举所有可能情况来找到所有解的算法。如果一个候选解最后被发现并不是可行解，回溯算法会舍弃它，并在前面的一些步骤做出一些修改，并重新尝试找到可行解。

究其本质，其实就是枚举。用递归

回溯算法是一种搜索法，试探法，它会在每一步做出选择，一旦发现这个选择无法得到期望结果，就回溯回去，重新做出选择。深度优先搜索利用的就是回溯算法思想。

<https://juejin.cn/post/6844903748456677389>

<https://juejin.cn/post/6844904037934972936>

[例子见算法题集](../jstopic.md)

## 适用场景

回溯算法很简单，它就是不断的尝试，直到拿到解。它的这种算法思想，使它通常用于解决广度的搜索问题，即从一组可能的解中，选择一个满足要求的解

## 通用大概逻辑

```js
let result = []
backTrace(depth,temp){
  if(depth == len){
    result.push(temp);
  }
  for (let i = index; i < nums.length; i ++){
    temp.push(nums[i]);
    backTrace(depth+1,temp)
    //回溯
    temp.pop();
  }
}
return result;
```

## 例子：电话号码字母组合

给定一个仅包含数字 2-9 的字符串，返回所有它能表示的字母组合。

给出数字到字母的映射（与电话按键相同）。注意 1 不对应任何字母。

```js
const letterCombinations = function (digits) {
    if (!digits) {
        return [];
    }
    const len = digits.length;
    const map = new Map();
    map.set('2', 'abc');
    map.set('3', 'def');
    map.set('4', 'ghi');
    map.set('5', 'jkl');
    map.set('6', 'mno');
    map.set('7', 'pqrs');
    map.set('8', 'tuv');
    map.set('9', 'wxyz');
    const result = [];

    function generate(i, str) {
        if (i == len) {
            result.push(str);
            return;
        }
        const tmp = map.get(digits[i]);
        for (let r = 0; r < tmp.length; r++) {
            generate(i + 1, str + tmp[r]);
        }
    }
    generate(0, '');
    return result;
};

letterCombinations([2,3]);
// 输入 [2,3]
// 输出：["ad", "ae", "af", "bd", "be", "bf", "cd", "ce", "cf"].
```
