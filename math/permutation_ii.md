#Permutation II

題意：也是給定一陣列，來列出所有可能的排列，但因有重複的元素，要避免產生重複的排列。

解題思路：一樣是使用遞迴的方式去作，唯一需要注意的就是重複的元素只能連續取，不能跳著取。

```java
class Solution {
    /**
     * @param nums: A list of integers.
     * @return: A list of unique permutations.
     */
    ArrayList<ArrayList<Integer>> res;
    boolean[] visited;
    public ArrayList<ArrayList<Integer>> permuteUnique(ArrayList<Integer> nums) {

        res = new ArrayList<ArrayList<Integer>>();
        if (nums == null || nums.size() == 0) {
            return res;
        }

        visited = new boolean[nums.size()];
        Collections.sort(nums);
        helper(nums, new ArrayList<Integer>(), nums.size());
        return res;

    }

    private void helper(ArrayList<Integer> nums, List<Integer> tempList, int n) {
        if (tempList.size() == n) {
            res.add(new ArrayList<Integer>(tempList));
            return;
        }

        for (int i = 0; i < n; i++) {
            if (visited[i] || (i != 0 && visited[i - 1] && nums.get(i) == nums.get(i - 1))) {
                continue;
            }
                visited[i] = true;
                tempList.add(nums.get(i));
                helper(nums, tempList, n);
                tempList.remove(tempList.size() - 1);
                visited[i] = false;
            
        }
    }
}
```
九章解法：

```java
public class Solution {
    public ArrayList<ArrayList<Integer>> permuteUnique(int[] num) {
        ArrayList<ArrayList<Integer>> result = new ArrayList<ArrayList<Integer>>();
        if(num == null || num.length == 0)
            return result;
        ArrayList<Integer> list = new ArrayList<Integer>();
        int[] visited = new int[num.length];
        
        Arrays.sort(num);
        helper(result, list, visited, num);
        return result;
    }
    
    public void helper(ArrayList<ArrayList<Integer>> result, ArrayList<Integer> list, int[] visited, int[] num) {
        if(list.size() == num.length) {
            result.add(new ArrayList<Integer>(list));
            return;
        }
		
        for(int i = 0; i < num.length; i++) {
            if (visited[i] == 1 || (i != 0 && num[i] == num[i - 1] && visited[i - 1] == 0)){
		// 上面的判斷其實並不影響最終結果，目的是為了讓dfs能更快
		/*
			上面這一連串判斷條件，重點在於要能理解!visited.contains(i-1)
			要理解這個，首先要明白i作為數組內的序號，i是唯一的
			給出一個排好序的數組，[1,2,2]
			第一層遞歸			第二層遞歸			第三層遞歸
			[1]					[1,2]				[1,2,2]
		        序號:[0]				 [0,1]			[0,1,2]
			這種都是OK的，但當第二層遞歸i掃到的是第二個"2"，情況就不一樣了
			[1]					[1,2]				[1,2,2]			
                         序號:[0]				[0,2]				[0,2,1]
			所以這邊判斷的時候!visited.contains(0)就變成了true，不會再繼續遞歸下去，跳出循環
			步主要就是為了去除連續重複存在的，很神奇反正 = =||
	*/
				continue;
			}
            visited[i] = 1;
            list.add(num[i]);
            helper(result, list, visited, num);
            list.remove(list.size() - 1);
            visited[i] = 0;
        }
    }    
}
```
---
###Reference
1. http://www.jiuzhang.com/solutions/permutations-ii/