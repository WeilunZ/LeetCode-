### Problem
Roman numerals are represented by seven different symbols: I, V, X, L, C, D and M.
```
Symbol       Value
I             1
V             5
X             10
L             50
C             100
D             500
M             1000
```

### Detail
For example, two is written as II in Roman numeral, just two one's added together. Twelve is written as, XII, which is simply X + II. The number twenty seven is written as XXVII, which is XX + V + II.

Roman numerals are usually written largest to smallest from left to right. However, the numeral for four is not IIII. Instead, the number four is written as IV. Because the one is before the five we subtract it making four. The same principle applies to the number nine, which is written as IX. There are six instances where subtraction is used:

I can be placed before V (5) and X (10) to make 4 and 9. 
X can be placed before L (50) and C (100) to make 40 and 90. 
C can be placed before D (500) and M (1000) to make 400 and 900.
Given an integer, convert it to a roman numeral. Input is guaranteed to be within the range from 1 to 3999.

### Ideas
* Generally, I choose the simplest way by creating a `HashMap` to store `special cases`. 
* Then I create a `Treemap` which can be used to sort by map key so as to traverse it in a descending key order.
* Finally, invoke the `putAll()` method to copy the special map into it and traverse it, then a simple circulation is used to add the corrent Roman characer to the end of a null charactor string.




### Code - Java

```Java
class Solution {
    public String intToRoman(int num) {
        Map<Integer,String> special = new HashMap<>();
        special.put(4,"IV");
        special.put(9,"IX");
        special.put(40,"XL");
        special.put(90,"XC");
        special.put(400,"CD");
        special.put(900,"CM");
        Map<Integer,String> normal = new TreeMap<>(new Comparator<Integer>(){
            public int compare(Integer a,Integer b){
                return b - a;
            }
        });
        normal.putAll(special);
        normal.put(1,"I");
        normal.put(5,"V");
        normal.put(10,"X");
        normal.put(50,"L");
        normal.put(100,"C");
        normal.put(500,"D");
        normal.put(1000,"M");
        
        if (special.containsKey(num)){
            return special.get(num);
        }
        
        String res = "";
        int temp = num;
        for (Map.Entry<Integer,String> entry: normal.entrySet()){
            while (temp >= entry.getKey()){
                res = res + entry.getValue();
                temp = temp - entry.getKey();
            }
            if (temp == 0){
                return res;
            }
        }
        return res;
    }
}
```
