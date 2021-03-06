# Number of Digit

计算数字k在0到n中的出现的次数，k可能是0~9的一个值

样例

    例如n=12，k=1，在 [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12]，我们发现1出现了5次 (1, 10, 11, 12)

## Solution

+ 当某一位的数字小于i时，那么该位出现i的次数为：更高位数字x当前位数
+ 当某一位的数字等于i时，那么该位出现i的次数为：更高位数字x当前位数+低位数字+1
+ 当某一位的数字大于i时，那么该位出现i的次数为：(更高位数字+1)x当前位数

假设一个5位数N=abcde，我们现在来考虑百位上出现2的次数，即，从0到abcde的数中， 有多少个数的百位上是2。分析完它，就可以用同样的方法去计算个位，十位，千位， 万位等各个位上出现2的次数。

当百位c为0时，比如说12013，0到12013中哪些数的百位会出现2？我们从小的数起， 200~299, 1200~1299, 2200~2299, … , 11200~11299, 也就是固定低3位为200~299，然后高位依次从0到11，共12个。再往下12200~12299 已经大于12013，因此不再往下。所以，当百位为0时，百位出现2的次数只由更高位决定， 等于更高位数字(12)x当前位数(100)=1200个。

当百位c为1时，比如说12113。分析同上，并且和上面的情况一模一样。 最大也只能到11200~11299，所以百位出现2的次数也是1200个。

上面两步综合起来，可以得到以下结论：

当某一位的数字小于2时，那么该位出现2的次数为：更高位数字x当前位数
当百位c为2时，比如说12213。那么，我们还是有200~299, 1200~1299, 2200~2299, … , 11200~11299这1200个数，他们的百位为2。但同时，还有一部分12200~12213， 共14个(低位数字+1)。所以，当百位数字为2时， 百位出现2的次数既受高位影响也受低位影响，结论如下：

当某一位的数字等于2时，那么该位出现2的次数为：更高位数字x当前位数+低位数字+1
当百位c大于2时，比如说12313，那么固定低3位为200~299，高位依次可以从0到12， 这一次就把12200~12299也包含了，同时也没低位什么事情。因此出现2的次数是： (更高位数字+1)x当前位数。结论如下：

当某一位的数字大于2时，那么该位出现2的次数为：(更高位数字+1)x当前位数

## Code

```java
class Solution {
    /*
     * param k : As description.
     * param n : As description.
     * return: An integer denote the count of digit k in 1..n
     */
    public int digitCounts(int k, int n) {
        int count = 0;
        int base = 1;
        while (n / base >= 1) {
            int curBit = n % (base*10) / base;
            int higher = n / (base*10);
            int lower = n % base;
            if (curBit < k) {
                count += higher * base;
            }
            else if (curBit == k) {
                count += higher * base + lower + 1;
            }
            else {
                count += (higher + 1) * base;
            }
            base *= 10;
        }
        return count;
    }
};


```

