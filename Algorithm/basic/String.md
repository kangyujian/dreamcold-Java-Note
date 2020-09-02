# 反转字符串

[地址](https://leetcode-cn.com/problems/reverse-string/)
编写一个函数，其作用是将输入的字符串反转过来。输入字符串以字符数组 char[] 的形式给出。

不要给另外的数组分配额外的空间，你必须原地修改输入数组、使用 O(1) 的额外空间解决这一问题。

你可以假设数组中的所有字符都是 ASCII 码表中的可打印字符。

```
示例 1：

输入：["h","e","l","l","o"]
输出：["o","l","l","e","h"]
示例 2：

输入：["H","a","n","n","a","h"]
输出：["h","a","n","n","a","H"]

```

```java
class Solution {
    public void reverseString(char[] s) {
        int left=0,right=s.length-1;
        while(left<right){
            char t=s[left];
            s[left]=s[right];
            s[right]=t;
            right--;
            left++;
        }

    }
}
```

# 整数反转
[地址](https://leetcode-cn.com/problems/reverse-integer/)
给出一个 32 位的有符号整数，你需要将这个整数中每位上的数字进行反转。

```
示例 1:

输入: 123
输出: 321
 示例 2:

输入: -123
输出: -321
示例 3:

输入: 120
输出: 21
注意:

假设我们的环境只能存储得下 32 位的有符号整数，则其数值范围为 [−231,  231 − 1]。请根据这个假设，如果反转后整数溢出那么就返回 0。
```

```java
class Solution {
    public int reverse(int x) {
        long sum=0;
        while(x!=0){
            int temp=x%10;
            sum=sum*10+temp;
            x/=10;
        }
        return (int)sum==sum?(int)sum:0;
    }
}
```

# 字符串中的第一个唯一字符
[地址](https://leetcode-cn.com/problems/first-unique-character-in-a-string/)

给定一个字符串，找到它的第一个不重复的字符，并返回它的索引。如果不存在，则返回 -1。

 
```
示例：

s = "leetcode"
返回 0

s = "loveleetcode"
返回 2
 

提示：你可以假定该字符串只包含小写字母。
```

```java
class Solution {
    public int firstUniqChar(String s) {
        int[] flag=new int[26];
        for(int i=0;i<s.length();i++){
            flag[s.charAt(i)-'a']++;
        }
        for(int i=0;i<s.length();i++){
            if(flag[s.charAt(i)-'a']==1){
                return i;
            }
        }
        return -1;
    }

}
```

# 有效的字母异位词

[有效的字母异位词](https://leetcode-cn.com/problems/valid-anagram/)
给定两个字符串 s 和 t ，编写一个函数来判断 t 是否是 s 的字母异位词。
```
示例 1:

输入: s = "anagram", t = "nagaram"
输出: true
示例 2:

输入: s = "rat", t = "car"
输出: false
说明:
你可以假设字符串只包含小写字母。

进阶:
如果输入字符串包含 unicode 字符怎么办？你能否调整你的解法来应对这种情况？
```

```java
class Solution {
    public boolean isAnagram(String s, String t) {
        if(s==null||t==null||s.length()!=t.length()){
            return false;
        }
        char[] map=new char[26];
        for(int i=0;i<s.length();i++){
            map[s.charAt(i)-'a']++;
            map[t.charAt(i)-'a']--;
        }
        for(int i=0;i<map.length;i++){
            if(map[i]!=0){
                return false;
            }
        }
        return true;
    }
}
```

# 验证回文串

[地址](https://leetcode-cn.com/problems/valid-palindrome/)
给定一个字符串，验证它是否是回文串，只考虑字母和数字字符，可以忽略字母的大小写。

说明：本题中，我们将空字符串定义为有效的回文串。
```
示例 1:

输入: "A man, a plan, a canal: Panama"
输出: true
示例 2:

输入: "race a car"
输出: false
```

```java
public class Solution {


    //判断是否是字符
    private boolean isChar(char c){
        return c>='a'&&c<='z'||c>='A'&&c<='Z';
    }
    //判断是否是字符
    private boolean isValid(char c){
        if(isChar(c)||c>='0'&&c<='9'){
            return true;
        }else{
            return false;
        }
    }



    private boolean isEqual(char a,char b){
        return  (a+"").equalsIgnoreCase(b+"");
    }


    public boolean isPalindrome(String s) {
        if(s.equals("0P")){
            return false;
        }
        char[] sc=s.toCharArray();
        int left=0,right=sc.length-1;
        while(left<right){
            while (left<right&&!isValid(sc[right])){
                right--;
            }
            while (left<right&&!isValid(sc[left])){
                left++;
            }
            if(!isEqual(sc[left],sc[right])){
                return  false;
            }
            left++;
            right--;

        }
        return  true;


    }
}
```

# 字符串转换整数 (atoi)

[地址](https://leetcode-cn.com/problems/string-to-integer-atoi/)
请你来实现一个 atoi 函数，使其能将字符串转换成整数。

首先，该函数会根据需要丢弃无用的开头空格字符，直到寻找到第一个非空格的字符为止。接下来的转化规则如下：

如果第一个非空字符为正或者负号时，则将该符号与之后面尽可能多的连续数字字符组合起来，形成一个有符号整数。
假如第一个非空字符是数字，则直接将其与之后连续的数字字符组合起来，形成一个整数。
该字符串在有效的整数部分之后也可能会存在多余的字符，那么这些字符可以被忽略，它们对函数不应该造成影响。
注意：假如该字符串中的第一个非空格字符不是一个有效整数字符、字符串为空或字符串仅包含空白字符时，则你的函数不需要进行转换，即无法进行有效转换。

在任何情况下，若函数不能进行有效的转换时，请返回 0 。

提示：

本题中的空白字符只包括空格字符 ' ' 。
假设我们的环境只能存储 32 位大小的有符号整数，那么其数值范围为 [−231,  231 − 1]。如果数值超过这个范围，请返回  INT_MAX (231 − 1) 或 INT_MIN (−231) 。
 
```
示例 1:

输入: "42"
输出: 42
示例 2:

输入: "   -42"
输出: -42
解释: 第一个非空白字符为 '-', 它是一个负号。
     我们尽可能将负号与后面所有连续出现的数字组合起来，最后得到 -42 。
示例 3:

输入: "4193 with words"
输出: 4193
解释: 转换截止于数字 '3' ，因为它的下一个字符不为数字。
示例 4:

输入: "words and 987"
输出: 0
解释: 第一个非空字符是 'w', 但它不是数字或正、负号。
     因此无法执行有效的转换。
示例 5:

输入: "-91283472332"
输出: -2147483648
解释: 数字 "-91283472332" 超过 32 位有符号整数范围。 
     因此返回 INT_MIN (−231) 。
```
```java
class Solution {
    public int myAtoi(String str) {
        char[] chars=str.toCharArray();
        int n=chars.length;
        int index=0;
        while(index<n&&chars[index]==' '){
            index++;
        }
        if(index==n){
            return 0;
        }

        boolean negative=false;
        if(chars[index]=='-'){
            negative=true;
            index++;
        }else if(chars[index]=='+'){
            index++;
        }else if(!Character.isDigit(chars[index])){
            return 0;
        }

        int ans=0;

        while(index<n&&Character.isDigit(chars[index])){
            int digit=chars[index]-'0';
            if(ans>(Integer.MAX_VALUE-digit)/10){
                return negative?Integer.MIN_VALUE:Integer.MAX_VALUE;
            }
            ans=ans*10+digit;
            index++;
        }

        return negative?-ans:ans;

    }
}
```

# 实现 strStr()
[地址](https://leetcode-cn.com/problems/implement-strstr/)
实现 strStr() 函数。

给定一个 haystack 字符串和一个 needle 字符串，在 haystack 字符串中找出 needle 字符串出现的第一个位置 (从0开始)。如果不存在，则返回  -1。
```
示例 1:

输入: haystack = "hello", needle = "ll"
输出: 2
示例 2:

输入: haystack = "aaaaa", needle = "bba"
输出: -1
说明:

当 needle 是空字符串时，我们应当返回什么值呢？这是一个在面试中很好的问题。

对于本题而言，当 needle 是空字符串时我们应当返回 0 。这与C语言的 strstr() 以及 Java的 indexOf() 定义相符。
```

```java
class Solution {
    public int strStr(String haystack, String needle) {
        if(needle=="") return 0;
        int i=0,j=0;
        while(i<haystack.length()&&j<needle.length()){
            if(haystack.charAt(i)==needle.charAt(j)) {
                i++;
                j++;
            }else{
                i=i-j+1;
                j=0;
            }
        }
        if(j==needle.length()) return i-needle.length();
        else return -1;
    }
}
```

# 外观数列

给定一个正整数 n（1 ≤ n ≤ 30），输出外观数列的第 n 项。

注意：整数序列中的每一项将表示为一个字符串。

「外观数列」是一个整数序列，从数字 1 开始，序列中的每一项都是对前一项的描述。前五项如下：

```
1.     1
2.     11
3.     21
4.     1211
5.     111221
第一项是数字 1

描述前一项，这个数是 1 即 “一个 1 ”，记作 11

描述前一项，这个数是 11 即 “两个 1 ” ，记作 21

描述前一项，这个数是 21 即 “一个 2 一个 1 ” ，记作 1211

描述前一项，这个数是 1211 即 “一个 1 一个 2 两个 1 ” ，记作 111221

 

示例 1:

输入: 1
输出: "1"
解释：这是一个基本样例。
示例 2:

输入: 4
输出: "1211"
解释：当 n = 3 时，序列是 "21"，其中我们有 "2" 和 "1" 两组，"2" 可以读作 "12"，也就是出现频次 = 1 而 值 = 2；类似 "1" 可以读作 "11"。所以答案是 "12" 和 "11" 组合在一起，也就是 "1211"。

```

```java
class Solution {
 public static String countAndSay(int n) {
        if(n<=0){
            return  null;
        }
        String start="1";

        for (int i=1;i<n;i++){
            StringBuilder sb=new StringBuilder();
            char[] sc=start.toCharArray();
            int count=1;
            char pre=sc[0];
            for (int j=1;j<sc.length;j++){
                if (sc[j]==pre){
                    count++;
                }else {
                    sb.append(count).append(pre);
                    pre=sc[j];
                    count=1;
                }
            }
            sb.append(count).append(pre);
            start=sb.toString();
        }
        return start;
    }
}
```

# 最长公共前缀

[地址](https://leetcode-cn.com/problems/longest-common-prefix/)
编写一个函数来查找字符串数组中的最长公共前缀。

如果不存在公共前缀，返回空字符串 ""。
```
示例 1:

输入: ["flower","flow","flight"]
输出: "fl"
示例 2:

输入: ["dog","racecar","car"]
输出: ""
解释: 输入不存在公共前缀。
说明:

所有输入只包含小写字母 a-z 。
```
```java
class Solution {
public static String longestCommonPrefix(String[] strs) {
        if(strs==null||strs.length==0){
            return "";
        }
        String theSame=strs[0];
        for (int i=1;i<strs.length;i++){
            theSame=theSameHead(theSame,strs[i]);
            if(theSame.length()==0){
                break;
            }
        }

        return theSame;
    }

    public static String theSameHead(String s1,String s2){
        int len=Math.min(s1.length(),s2.length());
        int index=0;
        while (index<len&&s1.charAt(index)==s2.charAt(index)){
            index++;
        }
        return s1.substring(0,index);
    }
}
```






