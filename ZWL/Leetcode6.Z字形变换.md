# LeetCode 6.Z字形变换
## 题目描述
将一个给定字符串根据给定的行数，以从上往下、从左到右进行 Z 字形排列。

比如输入字符串为 "LEETCODEISHIRING" 行数为 3 时，排列如下：
```
L   C   I   R
E T O E S I I G
E   D   H   N
```
之后，你的输出需要从左往右逐行读取，产生出一个新的字符串，比如："LCIRETOESIIGEDHN"。

请你实现这个将字符串进行指定行数变换的函数：
> string convert(string s, int numRows);

**示例1**
```
输入: s = "LEETCODEISHIRING", numRows = 3
输出: "LCIRETOESIIGEDHN"
```
**示例2**
```
输入: s = "LEETCODEISHIRING", numRows = 4
输出: "LDREOEIIECIHNTSG"
解释:
L     D     R
E   O E   I I
E C   I H   N
T     S     G
```

## 思路
维护一个布尔型变量godown，以及一个int型的变量curRow代表当前字符应该放在第几行，当curRow到达0或者行数-1时，代表着需要变向，也就是说godown取反，而curRow每次加1或者减1取决于godown是true还是false。使用StringBuilder来进行字符的append。

## 代码
```java
class Solution {
    public String convert(String s, int numRows) {
        if (numRows==1) return s;
        List<StringBuilder> list=new ArrayList<>();
        for (int i=0;i<numRows;i++){
            list.add(new StringBuilder());
        }
        int curRow=0;
        boolean godown=false;
        for (char c:s.toCharArray()){
            list.get(curRow).append(c);
            if (curRow==0||curRow==numRows-1){
                godown=!godown;
            }
            curRow+=godown?1:-1;
        }
        StringBuilder ret=new StringBuilder();
        for (StringBuilder sb:list){
            ret.append(sb);
        }
        return ret.toString();
    }
}
```