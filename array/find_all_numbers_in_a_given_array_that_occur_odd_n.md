# Find all numbers in a given array that occur odd number of times

```java
        public List<Integer> find(List<Integer> nums) {
                List<Integer> result = new ArrayList<>();
                if (nums == null) {
                        return result;
                }
                
                Collections.sort(nums);
                int length = nums.size();
                
                for (int i = 0; i < length; i++) {
                        int count = 1;
                        int val = nums.get(i);
                        while (i + 1 < length && nums.get(i) == nums.get(i + 1)) {
                                i++;
                                count++;
                        }
                        if (count % 2 == 1) {
                                result.add(val);
                        }
                }
                
                return result;
        }
```