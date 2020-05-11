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

## [371. 两整数之和[位运算与尾递归]](https://leetcode-cn.com/problems/sum-of-two-integers/)

分情况讨论：

-   若b为0，显然，和为a
-   若b不为0，$a+b=每一个位相加不进位的和+每一个位相加的进位$
    -   `a ^ b` -> 每一个位相加的和（不考虑进位）
    -   `a & b` -> 只有1&1的时候对应的位才为1，故可以表示进位
        -   若进位不为0，则需要把这个进位给加上去，一直递归到进位为0

```kotlin
class S371 {
    fun getSum(a: Int, b: Int): Int {
        return when (b) {
            0 -> a
            else -> getSum(a xor b, (a and b) shl 1)
        }
    }
}
```

# 2020-05-08

## [374. 猜数字大小[二分查找]](https://leetcode-cn.com/problems/guess-number-higher-or-lower/)

Kotlin中能被继承的类需要用`open`进行修饰

```kotlin
class S374:GuessGame() {
    override fun guessNumber(n:Int):Int {
        var lo = 1
        var hi = n
        var mid:Int
        while (lo <= hi) {
            mid = lo - (lo - hi) / 2
            val rtnRes = guess(mid)
            when (rtnRes) {
                0 -> return mid
                -1 -> hi = mid - 1
                1 -> lo = mid + 1
                else -> return -1
            }
        }
        return -1
    }
}
```

## [383. 赎金信[分桶操作，字典计数]](https://leetcode-cn.com/problems/ransom-note/)

根据26个字母分桶统计出现次数，对于字符串1中的每一个字符，对桶内对应数据-1，若桶内所有数据都>=0则可以匹配

-   初始化一个定长的数组`val array = IntArray(length)`
-   遍历数组、Collections：`array.forEach{ 操作符it相关语句 }`，it做循环指针
-   对数组进行条件判断：`array.all{ it operation ... }`

```kotlin
fun canConstruct(ransomNote: String, magazine: String): Boolean {
        val bucket = IntArray(26)
        magazine.forEach { bucket[it-'a']++ }
        ransomNote.forEach { bucket[it-'a']-- }
        return bucket.all { it >= 0 }
    }
```

## [387. 字符串中的第一个唯一字符[分桶，字典]](https://leetcode-cn.com/problems/first-unique-character-in-a-string/)

-   数组带序号遍历：`s.forEachIndexed { index, c -> if(bucket[c-'a']==1) return index }`

```kotlin
class S387 {
    fun firstUniqChar(s: String): Int {
        val bucket = IntArray(26)
        s.forEach { bucket[it-'a']++ }
        s.forEachIndexed { index, c -> if(bucket[c-'a']==1) return index }
        return -1
    }
}
```

# 2020-05-09

## [389. 找不同[分桶字典]](https://leetcode-cn.com/problems/find-the-difference/)

-   数组返回第一个符合要求的元素的位置：`bucket.indexOfFirst { it == 1 }`

```kotlin
class S389 {
    fun findTheDifference(s: String, t: String): Char {
        val bucket = IntArray(26)
        s.forEach { bucket[it-'a']++ }
        t.forEach {
            if (bucket[it-'a']==0) {
                return it
            }
            bucket[it-'a']--
        }
        return ' '
    }
}
```

## [392. 判断子序列[双指针扫描]](https://leetcode-cn.com/problems/is-subsequence/)

```kotlin
class S392 {
    fun isSubsequence(s: String, t: String): Boolean {
        var i = 0
        for (c in s.toCharArray()) {
            while (i<t.length && t[i]!=c) i++
            i++
        }
        return i<=t.length
    }
}
```

## [401. 二进制手表[二进制统计]](https://leetcode-cn.com/problems/binary-watch/)

遍历0:00到11:59，检查时间的小时与分钟转化为二进制后，1的个数和，若个数和符合则将其加入到结果集合中

-   数字对应的二进制中1的数量的统计

    ```kotlin
    fun count_1_Num(num: Int): Int {
        var res = 0
        var n = num
        while (n != 0) {
            n = n and (n - 1)
            res++
        }
        return res
    }
    ```

    -   原理：

        ```bash
        假设n=5，对应二进制为：0101，计算n&(n-1)
        1> n = 0101 & 0100 = 0100 -> 最低位1变为0 -> cnt = 1
        2> n = 0100 & 0011 = 0000 -> 最低位1变为0 -> cnt = 2
        3> n = 0000 -> cnt = 2
        每一次n&(n-1)都会将其最低位1变为0，故可用来统计1的数量
        ```

```kotlin
class S401 {
    /**
     * n -> 当前亮灯的数量
     */
    fun readBinaryWatch(num: Int): List<String> {
        val res = mutableListOf<String>()
        for (i in 0 until 12) {
            if (count_1_Num(i) == num) {
                res.add("%d:00".format(i))
                continue
            }
            for (j in 0 until 60) {
                if (count_1_Num(i) + count_1_Num(j) == num) {
                    res.add("%d:%02d".format(i, j))
                }
            }
        }
        return res
    }

    /**
     * 统计数中转化为二进制中1的数量
     */
    fun count_1_Num(num: Int): Int {
        var res = 0
        var n = num
        while (n != 0) {
            n = n and (n - 1)
            res++
        }
        return res
    }
}
```

# 2020-05-10

## [404. 左叶子之和[递归/队列]](https://leetcode-cn.com/problems/sum-of-left-leaves/)

使用递归+标记的方法判断是否是左叶子节点

```kotlin
class S404 {
    fun sumOfLeftLeaves(root: TreeNode?): Int {
        return sumOfLL(root, 0)
    }

    /**
     * type: 0->root 1->left 2->right
     */
    fun sumOfLL(root: TreeNode?, type: Int): Int {
        if (root == null) {
            return 0
        } else {
            // 为叶子节点
            if (root.left == null && root.right == null) {
                if (type == 1) {
                    return root.`val`
                } else {
                    return 0
                }
            }
            else if (root.left == null) {
                return sumOfLL(root.right, 2)
            }
            else if (root.right == null) {
                return sumOfLL(root.left, 1)
            } else {
                return sumOfLL(root.left, 1) + sumOfLL(root.right, 2)
            }
        }
    }
}
```

使用队列进行非递归遍历

```kotlin
class Solution {
    fun sumOfLeftLeaves(root: TreeNode?): Int {
        if (root == null) return 0

        val queue: Queue<TreeNode> = LinkedList<TreeNode>()
        queue.offer(root)
        var sum = 0
        while (queue.isNotEmpty()) {
            val node = queue.poll()
            val left = node.left
            if (left != null) {
                if (left.left == null && left.right == null) sum += left.`val`
                else queue.offer(left)
            }
            if (node.right != null) queue.offer(node.right)
        }
        return sum
    }
}
```

## [405. 数字转换为十六进制数[位运算,&0xf取最低四位]](https://leetcode-cn.com/problems/convert-a-number-to-hexadecimal/)

-   循环取n的最低4位转化为二进制，加入字符串中后进行翻转即为结果==`n & 0xf`==
-   每次右移位4位，正数最左侧补0，负数最左侧补1 ==`n >>= 4`==
-   循环终止条件
    -   1.  n为0，（n>0），此时无需继续循环，高位肯定为0
        2.  16进制字符串的长度<8 -> n的位数小于等于32位所以最后结果肯定小于等于8位

```kotlin
class S405 {
    private val tenToHex = listOf('0', '1', '2', '3', '4', '5', '6', '7', '8', '9', 'a', 'b', 'c', 'd', 'e', 'f')

    fun toHex(num: Int): String {
        if (num == 0) return "0"
        val sb = StringBuilder()
        var n = num
        while (n!=0 && sb.length<8) {
            sb.append(tenToHex[n and 0xf])
            n = n shr 4
        }
        return sb.reverse().toString()
    }
}
```

## [409. 最长回文串[分桶]](https://leetcode-cn.com/problems/longest-palindrome/)

分桶，统计每个字符出现的次数

-   偶数次数的可以均匀分布在左右，所以可以加入结果中
-   奇数次数的可以取其偶数部分分布在左右，取奇数次数减一加入结果中
-   若存在奇数次数的字符，则至多可挑选一个最长奇数次序的放入结果中，即只要出现奇数次数，最终最长结果为奇数。由于只能选择一个奇数次数的字符放入结果，所以最多+1，故出现奇数则结果加一
-   可以统计奇数出现的次数，桶的综合减去oddNum再加上1即为最后结果

```kotlin
class S409 {
    fun longestPalindrome1(s: String): Int {
        val map = HashMap<Char, Int>()
        s.forEach {
            if (map.containsKey(it)) {
                map[it] = map[it]!!+1
            } else {
                map[it] = 1
            }
        }
        var res = 0
        var hasOdd = false
        map.forEach { (t, u) ->
            if (u%2==0) {
                res += u
            } else {
                hasOdd = true
                res += u-1
            }
        }
        return res + if(hasOdd) 1 else 0
    }

    fun longestPalindrome(s: String): Int {
        val bucket = IntArray(128)
        s.forEach { bucket[it.toInt()]++ }
        var oddCount = bucket.count { (it and 1) == 1 }
        return if(oddCount > 1) bucket.sum()-(oddCount-1) else bucket.sum()
    }
}
```

# 2020-05-11

## [412. Fizz Buzz[穷举]](https://leetcode-cn.com/problems/fizz-buzz/)

```kotlin
class Solution {
    fun fizzBuzz(n: Int): List<String> {
        val re = ArrayList<String>(n)
        for (i in 1..n) {
            re.add(when {
                i % 3 != 0 && i % 5 != 0 -> i.toString()
                i % 3 == 0 && i % 5 == 0 -> "FizzBuzz"
                i % 3 == 0 -> "Fizz"
                else -> "Buzz"
            })
        }
        return re
    }
}
```

## [414. 第三大的数[遍历,边界条件]](https://leetcode-cn.com/problems/third-maximum-number/)

一次遍历，多次比较，注意判断`Int.MIN_VALUE`这个边界值可能存在于测试用例中

```kotlin
import kotlin.math.max

class S414 {
    fun thirdMax(nums: IntArray): Int {
        if (nums.size == 1) return nums[0]
        if (nums.size == 2) return max(nums[0], nums[1])

        var max1 = Int.MIN_VALUE
        var max2 = Int.MIN_VALUE
        var max3 = Int.MIN_VALUE

        var maxCount = 0        // 记录有多少个大的，防止都为同一数字，若maxCount>=3则返回第三大，否则第一大
        var hasIntMin = false   // 数字中是否有Int.MIN_VALUE

        nums.forEach {
            // 检查是不是Int.MIN_VALUE
            if (it == Int.MIN_VALUE && !hasIntMin) {
                hasIntMin = true
                maxCount++
            }
            when {  // and判断是为了防止it值等于max1或max2
                it > max1 -> {
                    maxCount++
                    max3 = max2
                    max2 = max1
                    max1 = it
                }
                it > max2 && it < max1 -> {
                    maxCount++
                    max3 = max2
                    max2 = it
                }
                it > max3 && it < max2 -> {
                    maxCount++
                    max3 = it
                }
            }
        }

        return if (maxCount >= 3) max3 else max1
    }
}
```

## [415. 字符串相加[双指针,进位加法]](https://leetcode-cn.com/problems/add-strings/)

字符串末尾相加，使用双指针指向两个字符串的末尾，使用`carry`表示进位并计算当前的末位的值，每一次循环都加上上一次`carry`进位的值+串1指针值+串2指针值，模10为末位，除10为进位

**可用于：字符串加法、链表加法以及二进制加法**

```kotlin
import java.lang.StringBuilder

class S415 {
    fun addStrings(num1: String, num2: String): String {
        val sb = StringBuilder()
        var carry = 0
        var i = num1.length - 1
        var j = num2.length - 1
        // 循环结束条件 1.串1指针为0 + 2.串2指针为0 + 3.carry进位为0
        while (i >= 0 || j >= 0 || carry != 0) {
            // 进位加上两字符串指针值，模10为末位，除10为进位，保留到下一次循环
            if (i >= 0) carry += (num1[i--] - '0')
            if (j >= 0) carry += (num2[j--] - '0')
            sb.append(carry % 10)
            carry /= 10
        }
        return sb.reverse().toString()
    }
}
```

