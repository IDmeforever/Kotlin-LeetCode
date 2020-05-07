刷题顺序：*简单->中等->困难*

# 2020-05-06

## 290.单词规律[HashMap对应]

对应关系，首先想到使用HashMap去做对应。
分情况进行讨论：map含有这个key，取出值检查是否为当前值；map不含有这个key，检查是否有对应的value，有为false，无则放入

```java
public boolean wordPattern(String pattern, String str) {
        if(pattern.length()==0 && str.length()==0){
            return true;
        } else if(pattern.length()==0 || str.length()==0) {
            return false;
        }

        String[] letters = str.split(" ");
        if(letters.length != pattern.length()) {
            return false;
        }

        Map<Character, String> map = new HashMap<>();

        for(int i=0; i<pattern.length(); i++) {
            // map含有这个key，取出值检查是否为当前值
            if(map.containsKey(pattern.charAt(i))) {
                if(!map.get(pattern.charAt(i)).equals(letters[i])) {
                    return false;
                }
            }
            // map不含有这个key，检查是否有对应的value，有为false，无则放入
            else {
                if(map.containsValue(letters[i])) {
                    return false;
                } else {
                    map.put(pattern.charAt(i), letters[i]);
                }
            }
        }
        return true;
    }
```

## 292. Nim游戏[找模式规律]

https://leetcode-cn.com/problems/nim-game/

自己可拿1-3个，先手拿，那么有总数1块，2，3的情况都可以一次性拿光胜利；4个的话，自己先手必败；5-7个可以拿n个使得剩余个数为4，那么4个先手必败的情况下对手必败，故为4的倍数则自己必败。

```java
public boolean canWinNim(int n) {
	return (n % 4 != 0);
}
```

# 2020-05-07

## [303. 区域和检索 - 数组不可变[预先计算]](https://leetcode-cn.com/problems/range-sum-query-immutable/)

设$sum[k]$为从[0, k-1]的累加和, 即$sum[k]=a[0]+...+a[k-1]$

则$sumRange(i,j)=a[i]+..+a[j]=a[0]+...+a[i]+...+a[j]-(a[0]+...+a[i-1])=sum[j+1]-sum[i]$

```kotlin
package LeetCode

class NumArray(nums: IntArray) {

    private val sum = IntArray(nums.size + 1)

    init {
        sum[0] = 0
        // 索引形式进行遍历
        for (index in nums.indices) {
            sum[index+1] = sum[index] + nums[index]
        }
    }

    fun sumRange(i: Int, j: Int): Int {
        return sum[j+1] - sum[i]
    }

}

/**
 * Your NumArray object will be instantiated and called as such:
 * var obj = NumArray(nums)
 * var param_1 = obj.sumRange(i,j)
 */
```

## [344. 反转字符串[双指针]](https://leetcode-cn.com/problems/reverse-string/)

```kotlin
class Solution {
    fun reverseString(s: CharArray): Unit {
        var len = s.size-1
        var p = 0
        var tmp: Char
        while (p <= len) {
            tmp = s[p]
            s[p++] = s[len]
            s[len--] = tmp
        }
    }
}
```

## [345. 反转字符串中的元音字母[双指针]](https://leetcode-cn.com/problems/reverse-vowels-of-a-string/)

Kotlin判断元素是否在数组中`element in listof('a', 'e', 'i', 'o', 'u')`

```Kotlin
class S345 {
    fun reverseVowels(s: String): String {
        var end = s.length - 1
        var start = 0
        val chars = s.toCharArray()
        while (start < end) {
            while ((!isVowels(s[start])) && start<end) start++
            while ((!isVowels(s[end])) && start<end) end--
            if(start >= end) break
            val tmp = s[start]
            chars[start] = chars[end]
            chars[end] = tmp
            start++
            end--
        }
        return String(chars)
    }

    private fun isVowels1(char: Char): Boolean {
        return char == 'a' || char == 'e' || char == 'i' || char == 'o' || char == 'u' ||
                char == 'A' || char == 'E' || char == 'I' || char == 'O' || char == 'U'
    }
    
    private fun isVowels(char: Char) = when(char) {
        'a','e','i','o','u','A','E','I','O','U' -> true
        else -> false
    }
}
```

## [349. 两个数组的交集[集合交集函数]](https://leetcode-cn.com/problems/intersection-of-two-arrays/)

两个`HashSet`的交集：`set1.retainsAll(set2)`

`IntArray` -> `Set`：  `val set1 = HashSet(nums1.asList())`

`Set` -> `IntArray`： `set1.toIntArray()`

```kotlin
class S349_1 {
    fun intersection(nums1: IntArray, nums2: IntArray): IntArray {
        val set1 = HashSet(nums1.asList())
        val set2 = HashSet(nums2.asList())
        set1.retainAll(set2)
        return set1.toIntArray()
    }
}
```

## [350. 两个数组的交集 II[排序后双指针]](https://leetcode-cn.com/problems/intersection-of-two-arrays-ii/)

Kotlin**可变长数组**==`ArrayList`==：`val res = ArrayList<Int>()`

`ArrayList` -> `IntArray`：`res.toIntArray()`

```kotlin
class S350 {
    fun intersect(nums1: IntArray, nums2: IntArray): IntArray {
        Arrays.sort(nums1)
        Arrays.sort(nums2)

        val res = ArrayList<Int>()

        var p1 = 0
        var p2 = 0

        while (p1<nums1.size && p2<nums2.size) {
            if (nums1[p1] < nums2[p2]) {
                p1++
            } else if (nums1[p1] > nums2[p2]) {
                p2++
            } else {
                res.add(nums1[p1])
                p1++
                p2++
            }
        }

        return res.toIntArray()
    }
}
```

## [367. 有效的完全平方数[二分查找]](https://leetcode-cn.com/problems/valid-perfect-square/)

`when`

1.  `when(expr) { case1 -> xxx  case2 -> xxx }`
2.  `when{ expr1 -> xxx   expr2 -> xxx }`

```kotlin
class S367 {
    fun isPerfectSquare(num: Int): Boolean {
        if (num < 1) return false
        if (num == 1) return true
        var lo: Long = 0
        var hi: Long = num/2L
        var mid: Long = 0
        while (lo <= hi) {
            mid = lo - (lo - hi) / 2L
            val mul = mid * mid
            if (mul == num.toLong()) {
                return true
            } else if (mul < num.toLong()) {
                lo = mid + 1
            } else {
                hi = mid - 1
            }
        }
        return false
    }
}
```

