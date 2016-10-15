#Text Justification

[Leetcode](https://leetcode.com/problems/text-justification/)

題意：

Given an array of words and a length L, format the text such that each line has exactly L characters and is fully (left and right) justified.

You should pack your words in a greedy approach; that is, pack as many words as you can in each line. Pad extra spaces ' ' when necessary so that each line has exactly L characters.

Extra spaces between words should be distributed as evenly as possible. If the number of spaces on a line do not divide evenly between words, the empty slots on the left will be assigned more spaces than the slots on the right.

For the last line of text, it should be left justified and no extra space is inserted between words.

For example,
words: ```["This", "is", "an", "example", "of", "text", "justification."]```
L: 16.

Return the formatted lines as:

```
[
   "This    is    an",
   "example  of text",
   "justification.  "
]
```

解題思路：

網友提供以下解法：

```java
public class Solution {
    public List<String> fullJustify(String[] words, int L) {
       List<String> res = new ArrayList<String>();
		int first = 0; // 一行中第一个单词的位置
		int last = 0; // 一行中最后一个单词的位置
		int count = 0; // 一行中的字符数(不包括空格)
		for (; last < words.length; last++) {// last-first表示单词间隔数,因为单词之间至少一个空格
			if ((count + words[last].length() + last - first) > L) {
				last--; // 最后一个单词不满足
				int totalSpaceCounts = L - count;// 总空格长度
				int extraSpaceCounts = 0;// 不能被均分的空格数
				int eachSpaceCounts = 0;// 每个间隔的平均空格数
				int spaceSlots = last - first;// 间隔数
				if (spaceSlots > 0) {// 判断空格是否可以被间隔均分
					extraSpaceCounts = totalSpaceCounts % spaceSlots;
					eachSpaceCounts = totalSpaceCounts / spaceSlots;
				}
				StringBuilder row = new StringBuilder();
				for (int j = first; j <= last; j++) {
					row.append(words[j]);
					if (j < last) {// 判断是不是一行的最后一个单词，如果是，后面就不用加空格了
						for (int i = 0; i < eachSpaceCounts; i++) {
							row.append(" ");
						}
						if (extraSpaceCounts > 0) {
							row.append(" ");// 不可以均分 ，多出的空格依次放在左边
							extraSpaceCounts--;// "example12of1text",
						}
					}
				}
				for (int j = row.length(); j < L; j++) {
					row.append(" ");
				}
				res.add(row.toString());
				first = last + 1;
				count = 0;

			} else {
				count += words[last].length();
			}
		}
		// 单独处理最后一行.因为最后一行的空格分布与前面不一样
		StringBuilder row = new StringBuilder();
		for (int i = first; i < words.length; i++) {
			row.append(words[i]);//"justification.  "
			if (row.length() < L) {
				row.append(" ");
			}
		}
		for (int i = row.length(); i < L; i++) {
			row.append(" ");
		}
		res.add(row.toString());
		return res;
	}
}
```

