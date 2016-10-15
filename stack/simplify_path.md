#Simplify Path

[原題網址](http://www.lintcode.com/en/problem/simplify-path/)

題意：

Given an absolute path for a file (Unix-style), simplify it.

For example,
path = "/home/", => "/home"
path = "/a/./b/../../c/", => "/c"

 

Corner Cases:

Did you consider the case where path = "/../"? In this case, you should return "/".

Another corner case is the path might contain multiple slashes '/' together, such as "/home//foo/".

In this case, you should ignore redundant slashes and return "/home/foo".

解題思路：

Updated 2015.11.20

網友 [shpolsky](https://leetcode.com/discuss/22592/java-10-lines-solution-with-stack) 提供了神之解法：

使用deque的原因是stack 的iterator有問題，請參考以下的連結：

http://stackoverflow.com/questions/16992758/is-there-a-bug-in-java-util-stacks-iterator

其程式碼如下：

```java
public class Solution {
    public String simplifyPath(String path) {
        Deque<String> stack = new LinkedList<>();
        Set<String> skip = new HashSet<>(Arrays.asList("..",".",""));
        
        for (String dir : path.split("/")) {
            if (dir.equals("..") && !stack.isEmpty()) {
                stack.pop();
            } else if (!skip.contains(dir)) {
                stack.push(dir);
            }
        }
        StringBuilder sb = new StringBuilder();
        for (String dir : stack) {
            sb.insert(0, "/" + dir);
        }
        
        if (sb.length() != 0) {
            return sb.toString();
        } else {
            return "/";
        }
    }
}
```

由網友[水中的魚](http://fisherlei.blogspot.com/2013/01/leetcode-simplify-path.html) 提供

可利用 stack 的特性，透過 "/" 把字串分割成 n 個子字串，根據 子字串 分成以下四種狀況來討論：

1. [/] ，跳過，直接開始尋找下一個元素
2. [.] ，表示當前目錄，不作任何事，直接找下一個元素
3. [..]，表示要跳到上一層，即把 stack 的最上層元素 pop 出來
4. 其他，表示是目錄名稱，直接把 stack 頂端的元素插入 res

最後，再根據 stack 的內容，重新拼path。這樣可以避免處理連續多個「/」的問題。

```java
public String simplifyPath(String path) {
        
    List<String> res = new ArrayList<String>();
    Stack<String> stack = new Stack<String>();
    StringBuilder sb = new StringBuilder();
    
    for (int i = 0; i < path.length(); i++) {
        if (path.charAt(i) == '/') {
            if (sb.length() == 0) {
                continue;
            } else if (sb.toString().equals("..")) {
                if (!stack.isEmpty()) {
                    stack.pop();
                }
                sb.setLength(0);
            } else if (sb.toString().equals(".")) {
                sb.setLength(0);
            } else {
                res.add(sb.toString());
                sb.setLength(0);
            }
        } else {
            sb.append(path.charAt(i));
        }
    }
    
    sb.setLength(0);
    for (int i = 0; i < res.size(); i++) {
        sb.append("/" + res.get(i));
    }
    if (res.isEmpty()) {
        return "/";
    } else {
        return sb.toString();
    }
}

```

網友 [愛做飯的小瑩子](http://www.cnblogs.com/springfor/p/3869666.html) 提供

直接使用 split 函數來作，但得看面試官給不給用，需要注意的是在這裡我沒沒用一個res來存放當下的path，是處理完後再一次將 path  還原，這時候我們需要還原path。但是不能彈出 stack，因為按照要求 stack 底元素應該為最先還原的目錄path。

例如：原始path是 /a/b/c/，stack 裡的順序是：a b c，如果依次 pop 還原的話是：/c/b/a（錯誤！），正確答案為：/a/b/c

所以這裡我應用了第二個 stack ，先將第一個 stack 元素彈出入 stack 到第二個 stack ，然後再利用第二個 stack 還原回初始 path 。

原始碼如下：

```java
public String simplifyPath(String path) {
        
    List<String> res = new ArrayList<String>();
    Stack<String> stack = new Stack<String>();
    
    if (path == null || path.length() == 0) {
        return path;
    }
    
    // 直接用 / 來切較方便，但得看面試官給不給用
    String[] list = path.split("/");
    
    for (int i = 0; i < list.length; i++) {
        if (list[i].equals(".") || list[i].length() == 0) {
            continue;
        } else if (list[i].equals("..")){
            if (!stack.isEmpty()) {
                stack.pop();
            }
        } else {
            stack.push(list[i]);
        }
    }
    
    StringBuilder sb = new StringBuilder();
    Stack<String> stack2 = new Stack<String>();
    while (!stack.isEmpty()) {
        stack2.push(stack.pop());
    }
    
    while (!stack2.isEmpty()) {
        sb.append("/" + stack2.pop());
    }
    
    if (sb.length() == 0) {
        return "/";
    } else {
        return sb.toString();
    }
    
}

```

>在網上還看到有人寫的最後還原path沒用到第二個 stack ，是因為可以利用Java中LinkedList 來表示第一個 stack 。LinkedList 很強大，包含了 stack 和 queue
的實現。所以最後還原時調用removeLast()函數就可以解決順序問題。

以下程式碼 由 [http://blog.csdn.net/linhuanmars/article/details/23972563](http://blog.csdn.net/linhuanmars/article/details/23972563) 提供

```java
public static String simplifyPath(String path) {  
        if(path.length() == 0){  
            return path;  
        }  
          
        String[] splits = path.split("/");  
        LinkedList<String> stack = new LinkedList<String>();  
        for (String s : splits) {  
            if(s.length()==0 || s.equals(".")){  
                continue;  
            }else if(s.equals("..")){  
                if(!stack.isEmpty()){  
                    stack.pop();  
                }  
            }else{  
                stack.push(s);  
            }  
        }  
          
        if(stack.isEmpty()){  
            stack.push("");  
        }  
        String ret = "";  
        while(!stack.isEmpty()){  
            ret += "/" + stack.removeLast();  
        }  
          
        return ret;  
    }
```
---
###Reference
1. http://www.cnblogs.com/springfor/p/3869666.html
2. http://bangbingsyb.blogspot.com/2014/11/leetcode-simplify-path.html
3. http://blog.csdn.net/linhuanmars/article/details/23972563
3. http://fisherlei.blogspot.com/2013/01/leetcode-simplify-path.html