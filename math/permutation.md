#Permutation



```java
class Solution {
    /**
     * @param nums: A list of integers.
     * @return: A list of permutations.
     */
     
    ArrayList<ArrayList<Integer>> res;
    boolean[] visited;
    public ArrayList<ArrayList<Integer>> permute(ArrayList<Integer> nums) {
        
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
            if (visited[i] == false) {
                visited[i] = true;
                tempList.add(nums.get(i));
                helper(nums, tempList, n);
                tempList.remove(tempList.size() - 1);
                visited[i] = false;
            }
        }
    }
}


```