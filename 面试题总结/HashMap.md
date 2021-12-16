

### JDK 源码中 HashMap 的 hash 方法原理是什么？

https://www.zhihu.com/question/20733617/answer/2022919698?utm_source=wechat_session&utm_medium=social&utm_oi=760881562945351680&utm_content=group1_Answer&utm_campaign=shareopn





## **一、HashCode为什么使用31作为乘数**

### **1. 固定乘积31在这用到了**

```java
// 获取hashCode "abc".hashCode();
public int hashCode() {
    int h = hash;
    if (h == 0 && value.length > 0) {
        char val[] = value;
        for (int i = 0; i < value.length; i++) {
            h = 31 * h + val[i];
        }
        hash = h;
    }
    return h;
}
```

在获取`hashCode`的源码中可以看到，有一个固定值`31`，在for循环每次执行时进行乘积计算，循环后的公式如下； `s[0]*31^(n-1) + s[1]*31^(n-2) + ... + s[n-1]`

**那么这里为什么选择31作为乘积值呢？**

### **2. 来自stackoverflow的回答**

最多的这个回答是来自《Effective Java》的内容；

```text
The value 31 was chosen because it is an odd prime. If it were even and the multiplication overflowed, information would be lost, as multiplication by 2 is equivalent to shifting. The advantage of using a prime is less clear, but it is traditional. A nice property of 31 is that the multiplication can be replaced by a shift and a subtraction for better performance: 31 * i == (i << 5) - i. Modern VMs do this sort of optimization automatically.
```

这段内容主要阐述的观点包括；

1. 31 是一个奇质数。
2. 另外在二进制中，2个5次方是32，那么也就是 `31 * i == (i << 5) - i`。这主要是说乘积运算可以使用位移提升性能，同时目前的JVM虚拟机也会自动支持此类的优化。

**80个赞 的回答**

```text
As Goodrich and Tamassia point out, If you take over 50,000 English words (formed as the union of the word lists provided in two variants of Unix), using the constants 31, 33, 37, 39, and 41 will produce less than 7 collisions in each case. Knowing this, it should come as no surprise that many Java implementations choose one of these constants.
```

- 这个回答就很有实战意义了，告诉你用超过5千个单词计算hashCode，这个hashCode的运算使用31、33、37、39和41作为乘积，得到的碰撞结果，31被使用就很正常了。
- **他这句话就就可以作为我们实践的指向了。**



### **3. Hash值碰撞概率统计**

接下来要做的事情并不难，只是根据`stackoverflow`的回答，统计出不同的乘积数对10万个单词的hash计算结果。*10个单词表已提供，可以通过关注公众号：bugstack虫洞栈进行下载*

### **4.2.1 乘数2散列**

- 乘数是2的时候，散列的结果基本都堆积在中间，没有很好的散列。

![image-20211117233333697](D:\面试题总结\面试题总结\image-20211117233333697.png)

### **4.2.2 乘数31散列**

- 乘数是31的时候，散列的效果就非常明显了，基本在每个范围都有数据存放。

![image-20211117233401208](D:\面试题总结\面试题总结\image-20211117233401208.png)

### **4.2.3 乘数199散列**

乘数是199是不能用的散列结果，但是它的数据是更加分散的，从图上能看到有两个小山包。但因为数据区间问题会有数据丢失问题，所以不能选择。

![image-20211117233433991](D:\面试题总结\面试题总结\image-20211117233433991.png)

## 

