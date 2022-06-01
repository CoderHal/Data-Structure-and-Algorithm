# OODS

## 1396. Design Underground System (unsolved)

### 1. HashTable

```java
class UndergroundSystem {
  private Map<Integer, Event> arrivals;
  private Map<String, Average> averages;
  public UndergroundSystem() {
    arrivals = new HashMap<>();
    averages = new HashMap<>();
  }
    
  public void checkIn(int id, String stationName, int t) {
    arrivals.add(id, new Event(id, stationName, t));
  }
    
  public void checkOut(int id, String stationName, int t) {
    Event arrivalEvent = arrivals.get(id);
    arrivals.remove(id);
    int diff = t - arrivalsEvent.time;
    String key = arrivalsEvent.stationName + stationName;
    Average average = average.containsKey(key) ? averages.get(key) : new Average();
    average.updateAverage(diff);
    averages.put(key, average);
  }
    
  public double getAverageTime(String startStation, String endStation) {
    String key = startStation + endStation;
    return average.get(key).caculateAverage();
 	}
  class Event {
    public int id;
    public int time;
    public String stationName;
    public void Event(int id, int time, String stationName) {
      id = this.id;
      t = this.time;
      stationName = this.stationName;
    }
  }
  class Average {
    public double total = 0;
    public int count = 0;
    public void updateAverage(int diff) {
      count++;
      total += diff;
    }
    public double caculateAverage() {
      return total / count;
    }
  }
}


```



## 49. Group Anagrams

### 1. HashMap

```java
class Solution {
  public List<List<String>> groupAnagrams(String[] strs) {
    Map<String, List> map = new HashMap<>();
    for (String s : strs) {
      char[] newS = s.toCharArray();
      Arrays.sort(newS);
      String key = String.valueOf(newS);
      if (map.containsKey(key)) {
        map.get(key).add(s);
      } else {
        map.put(key, new ArrayList());
        map.get(key).add(s);
      } 
    }
    return new ArrayList(map.values()); // Map 转 List 
    // Time O(Nklogk)
    // Space O(NK)
  }
}
```

