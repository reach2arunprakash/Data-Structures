#Number of Airplanes in the Sky

[]()

題意：

解題思路：

暴力法：花$$O(N^{2})$$，拿每個區段去與所有區段比，若有重疊則計算重疊次數。

排序法：將每個區段切成兩個時間點，起飛與降落，接著再依時間點來作排序，若該時間點為起飛，則 count 加一，若是降落，則 count 減一，並不斷根據 count 數來更新 max值，由於需要紀錄時間點與起飛降落，因此我們設計PlaneTime 物件來幫助我們紀錄時間點，並實作comparator來幫助我們排序，其程式碼如下：
    

```java
/**
 * Definition of Interval:
 * public classs Interval {
 *     int start, end;
 *     Interval(int start, int end) {
 *         this.start = start;
 *         this.end = end;
 *     }
 */

class PlaneTime implements Comparator<PlaneTime>{
    int time;
    int flag;
    // flag: 1代表起飛，0代表降落
    public PlaneTime(int t, int s) {
        this.time = t;
        this.flag = s;
    }
    
    public PlaneTime() {}
    
    // 設置comparator來幫助排序，若兩者時間相同的話，
    // 則依題目規定，起飛在前，降落在後
    public int compare(PlaneTime p1, PlaneTime p2){
        if(p1.time == p2.time) {
            return p1.flag - p2.flag;
        } else {
            return p1.time - p2.time;
        }
    }
}

class Solution {
    /**
     * @param intervals: An interval array
     * @return: Count of airplanes are in the sky.
     */
    public int countOfAirplanes(List<Interval> airplanes) { 
        
        if (airplanes == null || airplanes.size() == 0) {
            return 0;
        }
        
        // 建立一個特別的PlaneTime，將所有的起飛降落時間皆設為一個PlaneTime
        // 如有三架飛機，則有六個PlaneTime物件http://www.jiuzhang.com/solutions/number-of-airplanes-in-the-sky/，全部加入列表中排序。
        int len = airplanes.size();
        List<PlaneTime> timeList = new ArrayList<PlaneTime>();
        for (int i = 0; i < len; i++) {
            Interval cur = airplanes.get(i);
            PlaneTime takeOff = new PlaneTime(cur.start, 1);
            PlaneTime landing = new PlaneTime(cur.end, 0);
            timeList.add(takeOff);
            timeList.add(landing);
        }
        
        // 透過列表排序後，一一判斷每個時間點，若是起飛，則 count 加一
        // 若是降落，則 count 減一。
        Collections.sort(timeList, new PlaneTime());
        int max = 0;
        int count = 0;
        for (int i = 0; i < timeList.size(); i++) {
            PlaneTime cur = timeList.get(i);
            if (cur.flag == 1) {
                count++;
            } else if (cur.flag == 0) {
                count--;
            }
            max = (max > count) ? max : count;
        }
        
        return max;
    }
}

```
>Time Complexity：$$O(NlogN)$$

---
###Reference
1. http://www.jiuzhang.com/solutions/number-of-airplanes-in-the-sky/