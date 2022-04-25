---
title: 集合(Collection)
author: 小小千辰
tags:
  - 集合
  - Java
categories:
  - 千辰的小小笔记
abbrlink: 4aa8edf0
date: 2021-09-13 14:35:00
updated: 2021-09-14 00:00:00
---

## Collection接口

### 单列集合框架结构

```
|----Collection接口：单列集合，用来存储一个一个的对象
          |----List接口：存储有序的、可重复的数据。  -->“动态”数组
              |----ArrayList、LinkedList、Vector

          |----Set接口：存储无序的、不可重复的数据   -->高中讲的“集合”
             |----HashSet、LinkedHashSet、TreeSet
```

**Collection接口继承树：**

 ![image-20210919144517492](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241758256.png)

<br>

<br>

### 接口常用方法

1. 添加
   1. add(Objec tobj)
   2. addAll(Collection coll)
2. 获取有效元素的个数
   1. int size()
3. 清空集合
   1. void clear()
4. 是否是空集合
   1. boolean isEmpty()
5. 是否包含某个元素
   1. boolean contains(Object obj)：是通过元素的equals方法来判断是否是同一个对象
   2. boolean containsAll(Collection c)：也是调用元素的equals方法来比较的。拿两个集合的元素挨个比较
6. 删除
   1. boolean remove(Object obj) ：通过元素的equals方法判断是否是要删除的那个元素。只会删除找到的第一个元素
   2. boolean removeAll(Collection coll)：取当前集合的差集
7. 取两个集合的交集
   1. boolean retainAll(Collection c)：把交集的结果存在当前集合中，不影响c
8. 集合是否相等
   1. boolean equals(Object obj)
9. 转成对象数组
   1. Object[] toArray()
10. 获取集合对象的哈希值
    1. hashCode()
11. 遍历
    1. iterator()：返回迭代器对象，用于集合遍历

<br>

<br>



### Collection集合与数组间的转换

```java
//集合 --->数组：toArray()
Object[] arr = coll.toArray();
for(int i = 0;i < arr.length;i++){
    System.out.println(arr[i]);
}

//拓展：数组 --->集合:调用Arrays类的静态方法asList(T ... t)
List<String> list = Arrays.asList(new String[]{"AA", "BB", "CC"});
System.out.println(list);

List arr1 = Arrays.asList(new int[]{123, 456});
System.out.println(arr1.size());//1

List arr2 = Arrays.asList(new Integer[]{123, 456});
System.out.println(arr2.size());//2
```

<br>

<br>

### 存储元素的要求

> 向Collection接口的实现类的对象中添加数据obj时，要求obj所在类要重写equals().

<br>

<br>



## List接口

### 存储数据特点

> 储存有序的、可重复的数据。

<br>

<br>

### 常用方法

```
增：add(Object obj)
删：remove(int index) / remove(Object obj)
改：set(int index, Object ele)
查：get(int index)
插：add(int index, Object ele)
长度：size()
遍历：① Iterator迭代器方式
     ② 增强for循环
     ③ 普通的循环
```

<br>

<br>

### 常用实现类

```
|----Collection接口：单列集合，用来存储一个一个的对象
  |----List接口：存储有序的、可重复的数据。  -->“动态”数组,替换原有的数组
      |----ArrayList：作为List接口的主要实现类；线程不安全的，效率高；底层使用Object[] elementData存储
      |----LinkedList：对于频繁的插入、删除操作，使用此类效率比ArrayList高；底层使用双向链表存储
      |----Vector：作为List接口的古老实现类；线程安全的，效率低；底层使用Object[] elementData存储
```

<br>

<br>

### 源码分析

#### ArrayList的源码分析：

```java
1.1 jdk 7情况下
    ArrayList list = new ArrayList();//底层创建了长度是10的Object[]数组elementData
    list.add(123);//elementData[0] = new Integer(123);
    ...
    list.add(11);//如果此次的添加导致底层elementData数组容量不够，则扩容。
    默认情况下，扩容为原来的容量的1.5倍，同时需要将原有数组中的数据复制到新的数组中。
 
    结论：建议开发中使用带参的构造器（给定长度）：ArrayList list = new ArrayList(int capacity)
  
1.2 jdk 8中ArrayList的变化：
    ArrayList list = new ArrayList();//底层Object[] elementData初始化为{}.并没有创建长度为10的数组
 
    list.add(123);//第一次调用add()时，底层才创建了长度10的数组，并将数据123添加到elementData[0]
    ...
    后续的添加和扩容操作与jdk 7 无异。
1.3 小结：jdk7中的ArrayList的对象的创建类似于单例的饿汉式，而jdk8中的ArrayList的对象
          的创建类似于单例的懒汉式，延迟了数组的创建，节省内存。
```

<br>

#### LinkedList的源码分析：

```java
LinkedList list = new LinkedList(); 内部声明了Node类型的first和last属性，默认值为null
list.add(123);//将123封装到Node中，创建了Node对象。

其中，Node定义为：体现了LinkedList的双向链表的说法
private static class Node<E> {
     E item;
     Node<E> next;
     Node<E> prev;
 
     Node(Node<E> prev, E element, Node<E> next) {
         this.item = element;
         this.next = next;
         this.prev = prev;
     }
 }
```

<br>

#### Vector的源码分析：

```
jdk7和jdk8中通过Vector()构造器创建对象时，底层都创建了长度为10的数组。
在扩容方面，默认扩容为原来的数组长度的2倍。
```

<br>

<br>

### 存储元素的要求

> 添加的对象，所在的类要重写equals()方法

<br>

<br>

### 面试题

<details>
	<summary>ArrayList、LinkedList、Vector者的异同？</summary>
    <ul>
        <li>相同：三个类都是实现了List接口，存储数据的特点相同：存储有序的、可重复的数据</li>
        <li>不同：看上面的3、4部分</li>
    </ul>
</details>

<br>

<br>

## Set接口

### 存储数据特点

> 无序的、不可重复的元素

<div class="success">
    <blockquote>
        以HashSet为例说明：
            <ol>
            	<li>无序性：不等于随机性。存储的数据在底层数组中并非照数组索引的顺序添加，而是根据数据的哈希值决定的。</li>
                <li>不可重复性：保证添加的元素按照equals()判断时，不能返回true.即：相同的元素只能添加一个。</li>
            </ol>
    </blockquote>
</div>

<br>

<br>

### 元素的添加过程

> 以HashSet为例

```
我们向HashSet中添加元素a,首先调用元素a所在类的hashCode()方法，计算元素a的哈希值，
此哈希值接着通过某种算法计算出在HashSet底层数组中的存放位置（即为：索引位置，判断
数组此位置上是否已经元素：
    如果此位置上没其他元素，则元素a添加成功。 --->情况1
    如果此位置上有其他元素b(或以链表形式存在的多个元素，则比较元素a与元素b的hash值：
        如果hash值不相同，则元素a添加成功。--->情况2
        如果hash值相同，进而需要调用元素a所在类的equals()方法：
               equals()返回true,元素a添加失败
               equals()返回false,则元素a添加成功。--->情况3

对于添加成功的情况2和情况3而言：元素a 与已经存在指定索引位置上数据以链表的方式存储。
jdk 7 :元素a放到数组中，指向原来的元素。
jdk 8 :原来的元素在数组中，指向元素a
总结：七上八下

HashSet底层：数组+链表的结构。（前提：jdk7)
```

<br>

<br>

### 常用方法

> Set接口中没有额外定义新的方法，使用的都是Collection中声明过的方法。

<br>

<br>

### 常用实现类

```
|----Collection接口：单列集合，用来存储一个一个的对象
      |----Set接口：存储无序的、不可重复的数据   -->高中讲的“集合”
          |----HashSet：作为Set接口的主要实现类；线程不安全的；可以存储null值
              |----LinkedHashSet：作为HashSet的子类；遍历其内部数据时，可以按照添加的顺序遍历
             在添加数据的同时，每个数据还维护了两个引用，记录此数据前一个数据和后一个数据。对于频繁的遍历操作，LinkedHashSet效率高于HashSet.
          |----TreeSet：可以照添加对象的指定属性，进行排序。
```

<br>

<br>

### 存储元素的要求

```
HashSet/LinkedHashSet:

要求：向Set(主要指：HashSet、LinkedHashSet)中添加的数据，其所在的类一定要重写hashCode()和equals()
要求：重写的hashCode()和equals()尽可能保持一致性：相等的对象必须具有相等的散列码
     重写两个方法的小技巧：对象中用作 equals() 方法比较的 Field，都应该用来计算 hashCode 值。

TreeSet:
1.自然排序中，比较两个对象是否相同的标准为：compareTo()返回0.不再是equals().
2.定制排序中，比较两个对象是否相同的标准为：compare()返回0.不再是equals().

```

<br>

<br>

### TreeSet的使用

#### 使用说明

> 1.向TreeSet中添加的数据，要求是相同类的对象。
> 2.两种排序方式：自然排序（实现Comparable接口 和 定制排序（Comparator）

<br>

#### 常用的排序方式:

> 方式一：自然排序（User类实现了Comparable接口）

```java
@Test
    public void test1(){
        TreeSet set = new TreeSet();

        //失败：不能添加不同类的对象
//        set.add(123);
//        set.add(456);
//        set.add("AA");
//        set.add(new User("Tom",12));

            //举例一：
//        set.add(34);
//        set.add(-34);
//        set.add(43);
//        set.add(11);
//        set.add(8);

        //举例二：
        set.add(new User("Tom",12));
        set.add(new User("Jerry",32));
        set.add(new User("Jim",2));
        set.add(new User("Mike",65));
        set.add(new User("Jack",33));
        set.add(new User("Jack",56));


        Iterator iterator = set.iterator();
        while(iterator.hasNext()){
            System.out.println(iterator.next());
        }

    }
```

<br>

> 方式二：定制排序

```java
	@Test
    public void test2(){
        Comparator com = new Comparator() {
            //照年龄从小到大排列
            @Override
            public int compare(Object o1, Object o2) {
                if(o1 instanceof User && o2 instanceof User){
                    User u1 = (User)o1;
                    User u2 = (User)o2;
                    return Integer.compare(u1.getAge(),u2.getAge());
                }else{
                    throw new RuntimeException("输入的数据类型不匹配");
                }
            }
        };

        TreeSet set = new TreeSet(com);
        set.add(new User("Tom",12));
        set.add(new User("Jerry",32));
        set.add(new User("Jim",2));
        set.add(new User("Mike",65));
        set.add(new User("Mary",33));
        set.add(new User("Jack",33));
        set.add(new User("Jack",56));


        Iterator iterator = set.iterator();
        while(iterator.hasNext()){
            System.out.println(iterator.next());
        }
    }
```



