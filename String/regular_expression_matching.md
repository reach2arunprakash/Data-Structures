# Regular Expression Matching

[Leetcode](https://leetcode.com/problems/regular-expression-matching/)

題意：

Implement regular expression matching with support for '.' and '*'.

```
'.' Matches any single character.
'*' Matches zero or more of the preceding element.

The matching should cover the entire input string (not partial).

The function prototype should be:
bool isMatch(const char *s, const char *p)

Some examples:
isMatch("aa","a") → false
isMatch("aa","aa") → true
isMatch("aaa","aa") → false
isMatch("aa", "a*") → true
isMatch("aa", ".*") → true
isMatch("ab", ".*") → true
isMatch("aab", "c*a*b") → true
```


解題思路：

網友 [Simple and Stupid](http://simpleandstupid.com/2014/10/14/regular-expression-matching-leetcode-%E8%A7%A3%E9%A2%98%E7%AC%94%E8%AE%B0/) 提供了以下思路：

"主要思路是遞歸，每次比較s和p的第一個字符，難點主要集中在對 \* 的處理，幾種分支情況要想清楚。
主要有兩個case：
1. p的第二個字符不是 \* ，這種是簡單情況。
2. p的第二個字符是 \*，這種情況相對複雜。

對於每次對比，還要注意p的第一個字符是否為 .，如果是，則match，繼續比較後面的。
"

```java
public class Solution {
    /**
     * @param s: A string 
     * @param p: A string includes "." and "*"
     * @return: A boolean
     */
    public boolean isMatch(String s, String p) {
        
        if (p.length() == 0) {
            return s.length() == 0;
        }
        
        if (p.length() == 1) {
            
            if (s.length() < 1) {
                return false;
            } else if (p.charAt(0) != s.charAt(0) && p.charAt(0) != '.') {
                return false;
            } else {
                return isMatch(s.substring(1), p.substring(1));
            }
        }
        
        // 檢查下一個字元是否為 '*'
        // 處理 "a*"之類的，所以是檢查它下一個是否為 '*'
        // 若不是的話。則檢查當前的字元是否相同，
        // 若不同則直接返回 false，不用繼續往下比了。
        // 若相同，則從下一個字元再繼續往下遞迴
        if (p.charAt(1) != '*') {
            if (s.length() < 1) {
                return false;
            } else if (s.charAt(0) != p.charAt(0) && p.charAt(0) != '.') {
                return false;
            } else {
                return isMatch(s.substring(1), p.substring(1));
            }
        } else {
            // 如果下一個字元是 '*'
            // 則跳過當前星號，繼續往下比
            // 若後面字串皆比過對了，則返回 true
            if (isMatch(s, p.substring(2))) {
                return true;
            }
            
            // 否則不斷的枚舉s的子字串與跳過星號來比
            // 前提是 星號前面的字元與 s 字串的第i個字元符合
            int i = 0;
            while (i < s.length() && (s.charAt(i) == p.charAt(0) || p.charAt(0) == '.')) {
                if (isMatch(s.substring(i + 1), p.substring(2))) {
                    return true;
                }
                i++;
            }
            
            // 比到最後還是沒達到match的話，則返回 false
            return false;
        }
    }
}

```

處理中間兩個case 的程式碼皆一樣，因此可以進而簡化成下面的程式碼：

```java
public class Solution {
    public boolean isMatch(String s, String p) {
        if (p.length() == 0) {
            return s.length() == 0;
        }
        
        if (p.length() == 1 || (p.length() > 1 && p.charAt(1) != '*')) {
            if (s.length() < 1) {
                return false;
            } else if (p.charAt(0) != s.charAt(0) && p.charAt(0) != '.') {
                return false;
            } else {
                return isMatch(s.substring(1), p.substring(1));
            }
        }else {
            if (isMatch(s, p.substring(2))) {
                return true;
            }
            
            int i = 0;
            while (i < s.length() && (s.charAt(i) == p.charAt(0) || p.charAt(0) == '.')) {
                if (isMatch(s.substring(i + 1), p.substring(2))) {
                    return true;
                }
                i++;
            }
            
            return false;
        }
    }
}
```

較清楚的遞迴解法：

```java
public boolean isMatch(String s, String p) {
	// base case
	if (p.length() == 0) {
		return s.length() == 0;
	}
 
	// special case
	if (p.length() == 1) {
 
		// if the length of s is 0, return false
		if (s.length() < 1) {
			return false;
		}
 
		//if the first does not match, return false
		else if ((p.charAt(0) != s.charAt(0)) && (p.charAt(0) != '.')) {
			return false;
		}
 
		// otherwise, compare the rest of the string of s and p.
		else {
			return isMatch(s.substring(1), p.substring(1));
		}
	}
 
	// case 1: when the second char of p is not '*'
	if (p.charAt(1) != '*') {
		if (s.length() < 1) {
			return false;
		}
		if ((p.charAt(0) != s.charAt(0)) && (p.charAt(0) != '.')) {
			return false;
		} else {
			return isMatch(s.substring(1), p.substring(1));
		}
	}
 
	// case 2: when the second char of p is '*', complex case.
	else {
		//case 2.1: a char & '*' can stand for 0 element
		if (isMatch(s, p.substring(2))) {
			return true;
		}
 
		//case 2.2: a char & '*' can stand for 1 or more preceding element, 
		//so try every sub string
		int i = 0;
		while (i<s.length() && (s.charAt(i)==p.charAt(0) || p.charAt(0)=='.')){
			if (isMatch(s.substring(i + 1), p.substring(2))) {
				return true;
			}
			i++;
		}
		return false;
	}
}
```

網友 [peng4](https://leetcode.com/discuss/66032/java-solution-o-n-2-dp-with-some-explanations) 提供超神dp作法，程式碼如下：

```java
public boolean isMatch(String s, String p) {
    int sL=s.length(), pL=p.length();

    boolean[][] dp = new boolean[sL+1][pL+1];
    dp[0][0] = true; // If s and p are "", isMathch() returns true;

    for(int i=0; i<=sL; i++) {

        // j starts from 1, since dp[i][0] is false when i!=0;
        for(int j=1; j<=pL; j++) {
            char c = p.charAt(j-1);

            if(c != '*') {
                // The last character of s and p should match;
                // And, dp[i-1][j-1] is true;
                dp[i][j] = i>0 && dp[i-1][j-1] && (c=='.' || c==s.charAt(i-1));
            }
            else {
                // Two situations:
                // (1) dp[i][j-2] is true, and there is 0 preceding element of '*';
                // (2) The last character of s should match the preceding element of '*';
                //     And, dp[i-1][j] should be true;
                dp[i][j] = (j>1 && dp[i][j-2]) ||
                           (i>0 && dp[i-1][j] && (p.charAt(j-2)=='.' || p.charAt(j-2)==s.charAt(i-1)));
            }
        }
    }

    return dp[sL][pL];
}
```
---
###Reference
1. http://simpleandstupid.com/2014/10/14/regular-expression-matching-leetcode-%E8%A7%A3%E9%A2%98%E7%AC%94%E8%AE%B0/
2. http://hehejun.blogspot.com/2014/11/leetcoderegular-expression-matching_4.html
3. http://www.programcreek.com/2012/12/leetcode-regular-expression-matching-in-java/