---
title: 集合(Map)
author: 小小千辰
tags:
  - 集合
  - Java
categories:
  - 千辰的小小笔记
abbrlink: 71b74931
date: 2021-09-15 15:26:14
updated: 2021-09-16 00:00:00
---

## Map接口

### 常用实现类

```
|----Map:双列数据，存储key-value键值对的数据   ---类似于高中的函数：y = f(x)
       |----HashMap:作为Map的主要实现类；线程不安全的，效率高；存储null的key和value
              |----LinkedHashMap:保证在遍历map元素时，可以照添加的顺序实现遍历。
                    原因：在原的HashMap底层结构基础上，添加了一对指针，指向前一个和后一个元素。
                    对于频繁的遍历操作，此类执行效率高于HashMap。
       |----TreeMap:保证按照添加的key-value对进行排序，实现排序遍历。此时考虑key的自然排序或定制排序
                    底层使用红黑树
       |----Hashtable:作为古老的实现类；线程安全的，效率低；不能存储null的key和value
              |----Properties:常用来处理配置文件。key和value都是String类型
   
   
      HashMap的底层：数组+链表  （jdk7及之前)
                    数组+链表+红黑树 （jdk 8)
```

<br><br>

### 存储结构的理解

```
>Map中的key:无序的、不可重复的，使用Set存储所有的key  ---> key所在的类要重写equals()和hashCode() （以HashMap为例)
>Map中的value:无序的、可重复的，使用Collection存储所的value --->value所在的类要重写equals()
> 一个键值对：key-value构成了一个Entry对象。
>Map中的entry:无序的、不可重复的，使用Set存储所的entry
```

**图示：**

![image-20210919154110587](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241753117.png)

<br><br>

### 常用方法

> 添加：`put(Object key,Object value)`
> 删除：`remove(Object key)`
> 修改：`put(Object key,Object value)`
> 查询：`get(Object key)`
> 长度：`size()`
> 遍历：`keySet() / values() / entrySet()`

<br><br>

### 源码分析

#### HashMap在**jdk7**中实现原理：

```
HashMap map = new HashMap():
  在实例化以后，底层创建了长度是16的一维数组Entry[] table。
  ...可能已经执行过多次put...
  map.put(key1,value1):
  首先，调用key1所在类的hashCode()计算key1哈希值，此哈希值经过某种算法计算以后，得到在Entry数组中的存放位置。
  如果此位置上的数据为空，此时的key1-value1添加成功。 ----情况1
  如果此位置上的数据不为空，(意味着此位置上存在一个或多个数据(以链表形式存在)),比较key1和已经存在的一个或多个数据的哈希值：
          如果key1的哈希值与已经存在的数据的哈希值都不相同，此时key1-value1添加成功。----情况2
          如果key1的哈希值和已经存在的某一个数据(key2-value2)的哈希值相同，继续比较：调用key1所在类的equals(key2)方法，比较：
                  如果equals()返回false:此时key1-value1添加成功。----情况3
                  如果equals()返回true:使用value1替换value2。

  补充：关于情况2和情况3：此时key1-value1和原来的数据以链表的方式存储。

  在不断的添加过程中，会涉及到扩容问题，当超出临界值(且要存放的位置非空)时，扩容。默认的扩容方式：扩容为原来容量的2倍，并将原的数据复制过来。
```

<br>

#### HashMap在**jdk8**中实现原理：

```
1. new HashMap():底层没有创建一个长度为16的数组
2. jdk 8底层的数组是：Node[],而非Entry[]
3. 首次调用put()方法时，底层创建长度为16的数组
4. jdk7底层结构只有：数组+链表。jdk8中底层结构：数组+链表+红黑树。
	4.1 形成链表时，七上八下（jdk7:新的元素指向旧的元素。jdk8：旧的元素指向新的元素）
	4.2 当数组的某一个索引位置上的元素以链表形式存在的数据个数 > 8 且当前数组的长度 > 64时，此时此索引位置上的所数据改为使用红黑树存储。
```

<br>

#### HashMap底层典型属性的属性的说明：

> `DEFAULT_INITIAL_CAPACITY` : HashMap的默认容量，16
> `DEFAULT_LOAD_FACTOR`：HashMap的默认加载因子：0.75
> `threshold`：扩容的临界值，=容量*填充因子：16 * 0.75 => 12
> `TREEIFY_THRESHOLD`：Bucket中链表长度大于该默认值，转化为红黑树:8
> `MIN_TREEIFY_CAPACITY`：桶中的Node被树化时最小的hash表容量:64

<br>

####  LinkedHashMap的底层实现原理

> LinkedHashMap底层使用的结构与HashMap相同，因为LinkedHashMap继承于HashMap.
> 区别就在于：LinkedHashMap内部提供了Entry，替换HashMap中的Node.
>
> ![image-20210919154530497](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241753693.png)

<br><br>

### TreeMap的使用

> 向TreeMap中添加key-value，要求key必须是由同一个类创建的对象
> 因为要按照key进行排序：自然排序 、定制排序

<br><br>

### Hashtable

* Hashtable是个古老的Map 实现类，JDK1.0就提供了。不同于HashMap，Hashtable是线程安全的
* Hashtable实现原理和HashMap相同，功能相同。底层都使用哈希表结构，查询速度快，很多情况下可以互用。
* 与HashMap不同，Hashtable不允许使用null 作为key和value
* 与HashMap一样，Hashtable也不能保证其中Key-Value 对的顺序
* Hashtable判断两个key相等、两个value相等的标准，与HashMap一致。

<br><br>

### 使用Properties读取配置文件

```java
//Properties:常用来处理配置文件。key和value都是String类型
public static void main(String[] args)  {
    FileInputStream fis = null;
    try {
        Properties pros = new Properties();

        fis = new FileInputStream("jdbc.properties");
        pros.load(fis);//加载流对应的文件

        String name = pros.getProperty("name");
        String password = pros.getProperty("password");

        System.out.println("name = " + name + ", password = " + password);
    } catch (IOException e) {
        e.printStackTrace();
    } finally {
        if(fis != null){
            try {
                fis.close();
            } catch (IOException e) {
                e.printStackTrace();
            }

        }
    }
```

<br><br>

### 面试题

<details>
	<summary>HashMap的底层实现原理？</summary>
    <a href="#源码分析">见上文HashMap底层原理分析</a>
</details>

<br><br>

## Collections工具类的使用

> 作用：操作Collection和Map的工具类

### 常用方法

1. reverse(List)：反转 List 中元素的顺序
2. shuffle(List)：对 List 集合元素进行随机排序
3. sort(List)：根据元素的自然顺序对指定 List 集合元素升序排序
4. sort(List，Comparator)：根据指定的 Comparator 产生的顺序对 List 集合元素进行排序
5. swap(List，int， int)：将指定 list 集合中的 i 处元素和 j 处元素进行交换
6. Object max(Collection)：根据元素的自然顺序，返回给定集合中的最大元素
7. Object max(Collection，Comparator)：根据 Comparator 指定的顺序，返回给定集合中的最大元素
8. Object min(Collection)
9. Object min(Collection，Comparator)
10. int frequency(Collection，Object)：返回指定集合中指定元素的出现次数
11. void copy(List dest,List src)：将src中的内容复制到dest中
12. boolean replaceAll(List list，Object oldVal，Object newVal)：使用新值替换 List 对象的所旧值

<br>

> 说明：ArrayList和HashMap都是线程不安全的，如果程序要求线程安全，我们可以将ArrayList、HashMap转换为线程的。
> 使用synchronizedList(List list） 和 synchronizedMap(Map map）

![ ](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241753486.png)

<br>

<br>

### 面试题

<details>
	<summary>Collection 和 Collections的区别？</summary>
    <a href="https://www.cnblogs.com/jxxblogs/p/11547898.html">不写了找个博客看看吧</a>
</details>
