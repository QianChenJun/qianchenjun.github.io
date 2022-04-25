---
title: IO流
author: 小小千辰
tags:
  - IO
  - Java
categories:
  - 千辰的小小笔记
abbrlink: fed4c017
date: 2021-09-17 16:02:15
updated: 2021-09-17 16:02:15
---

## File类的使用

### File类的实例化

```java
File(String filePath)
File(String parentPath,String childPath)
File(File parentFile,String childPath)
    
// 构造器1
File file1 = new File("hello.txt");
File file2 = new File("D:\\桌面\\新的开始\\Java\\code\\he.txt");

System.out.println(file1);
System.out.println(file2);

// 构造器2
File file3 = new File("D:\\桌面\\新的开始\\Java", "code");
System.out.println(file3);

// 构造器3
File file4 = new File(file3, "hi.txt");
System.out.println(file4);
```

<br>

### File类的常用方法

```java
File类的获取功能
public String getAbsolutePath()：获取绝对路径
public String getPath() ：获取路径
public String getName() ：获取名称
public String getParent()：获取上层文件目录路径。若无，返回null
public long length() ：获取文件长度（即：字节数）。不能获取目录的长度。
public long lastModified() ：获取最后一次的修改时间，毫秒值
// 这两个方法适用于文件目录
public String[] list() ：获取指定目录下的所有文件或者文件目录的名称数组
public File[] listFiles() ：获取指定目录下的所有文件或者文件目录的File数组

File类的重命名功能
public boolean renameTo(File dest):把文件重命名为指定的文件路径
例如：file1.renameTo(file2)
调用这个方法的时候要保证.file2在硬盘中不存在
     
File类的判断功能
public boolean isDirectory()：判断是否是文件目录
public boolean isFile() ：判断是否是文件
public boolean exists() ：判断是否存在
public boolean canRead() ：判断是否可读
public boolean canWrite() ：判断是否可写
public boolean isHidden() ：判断是否隐藏

File类的创建功能
创建硬盘中对应的文件或文件目录
public boolean createNewFile() ：创建文件。若文件存在，则不创建，返回false
public boolean mkdir() ：创建文件目录。如果此文件目录存在，就不创建了。如果此文件目录的上层目录不存在，也不创建。
public boolean mkdirs() ：创建文件目录。如果此文件目录存在，就不创建了。如果上层文件目录不存在，一并创建

File类的删除功能
删除磁盘中的文件或文件目录
public boolean delete()：删除文件或者文件夹
删除注意事项：Java中的删除不走回收站。
```

<br>

<br>

## IO流概述

### 流的分类

> 1. 操作数据单位：字节流、字符流
> 2. 数据的流向：输入流、输出流
> 3. 流的角色：节点流、处理流

![image-20210923163042980](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241827555.png)_流的分类_

<br>

### 流的体系结构

![image-20210923163212394](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241827179.png)_流的体系结构_

<div class="danger">
    <blockquote>
        说明：红框对应的是IO流中的4个抽象基类。<br>
		蓝框的流需要大家重点关注。
    </blockquote>
</div>

<br>

### 比较重要的几个流结构

![image-20210923163352531](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241827266.png)_重要的流结构_

<br>

### 输入、输出的标准化过程

#### 输入过程

<div class="success">
    <blockquote>
        <ol>
            <li>创建File类的对象，指明读取的数据的来源。（要求此文件一定要存在）</li>
            <li>创建相应的输入流，将File类的对象作为参数，传入流的构造器中</li>
            <li>具体的读入过程：<br>
            	创建相应的byte[] 或 char[]。
            </li>
            <li>关闭流资源</li>
        </ol>
        说明：程序中出现的异常需要使用try-catch-finally处理。
    </blockquote>
</div>

<br>

#### 输出过程

<div class="success">
    <blockquote>
        <ol>
            <li>创建File类的对象，指明写出的数据的位置。（不要求此文件一定要存在）</li>
            <li>创建相应的输出流，将File类的对象作为参数，传入流的构造器中</li>
            <li>具体的写出过程：<br>
            	write(char[]/byte[] buffer,0,len)
            </li>
            <li>关闭流资源</li>
        </ol>
        说明：程序中出现的异常需要使用try-catch-finally处理。
    </blockquote>
</div>

<br>

<br>

## 节点流（或文件流）

### FileReader/FileWriter的使用：

#### FileReader的使用

> 说明点：
> 1. `read()`的理解：返回读入的一个字符。如果达到文件末尾，返回-1
> 2. 异常的处理：为了保证流资源一定可以执行关闭操作。需要使用`try-catch-finally`处理
> 3. 读入的文件一定要存在，否则就会报`FileNotFoundException`。

```java
@Test
    public void testFileReader1()  {
        FileReader fr = null;
        try {
            //1.File类的实例化
            File file = new File("hello.txt");

            //2.FileReader流的实例化
            fr = new FileReader(file);

            //3.读入的操作
            //read(char[] cbuf):返回每次读入cbuf数组中的字符的个数。如果达到文件末尾，返回-1
            char[] cbuf = new char[5];
            int len;
            // 循环结束是关键
            while((len = fr.read(cbuf)) != -1){
                //正确的写法
                String str = new String(cbuf,0,len); // 这里调用的方法是关键
                System.out.print(str);
            }
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            // 资源关闭判断是否为空
            if(fr != null){
                //4.资源的关闭
                try {
                    fr.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }

            }
        }

    }
```

<br>

#### FileWriter的使用

> 说明：
> 1. 输出操作，对应的File可以不存在的。并不会报异常
> 2. 
>     File对应的硬盘中的文件如果不存在，在输出的过程中，会自动创建此文件。
>     File对应的硬盘中的文件如果存在：
>           如果流使用的构造器是：`FileWriter(file,false) / FileWriter(file)`:对原文件的覆盖
>           如果流使用的构造器是：`FileWriter(file,true):`不会对原文件覆盖，而是在原文件基础上追加内容

```java
@Test
public void testFileWriter() {
    FileWriter fw = null;
    try {
        //1.提供File类的对象，指明写出到的文件
        File file = new File("hello1.txt");

        //2.提供FileWriter的对象，用于数据的写出
        fw = new FileWriter(file,false);

        //3.写出的操作
        fw.write("I have a dream!\n");
        fw.write("you need to have a dream!");
    } catch (IOException e) {
        e.printStackTrace();
    } finally {
        //4.流资源的关闭
        if(fw != null){

            try {
                fw.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
}
```

<br>

#### 文本文件的复制

```java
	@Test
    public void testFileReaderFileWriter() {
        FileReader fr = null;
        FileWriter fw = null;
        try {
            //1.创建File类的对象，指明读入和写出的文件
            File srcFile = new File("hello.txt");
            File destFile = new File("hello2.txt");

            //不能使用字符流来处理图片等字节数据
//            File srcFile = new File("爱情与友情.jpg");
//            File destFile = new File("爱情与友情1.jpg");


            //2.创建输入流和输出流的对象
            fr = new FileReader(srcFile);
            fw = new FileWriter(destFile);


            //3.数据的读入和写出操作
            char[] cbuf = new char[5];
            int len;//记录每次读入到cbuf数组中的字符的个数
            while((len = fr.read(cbuf)) != -1){
                //每次写出len个字符
                fw.write(cbuf,0,len);

            }
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            //4.关闭流资源
            //方式二：
            try {
                if(fw != null)
                    fw.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
            try {
                if(fr != null)
                    fr.close();
            } catch (IOException e) {
                e.printStackTrace();
            }

        }

    }
```

<br>

<br>

### FileInputStream / FileOutputStream的使用：

> 1. 对于文本文件(.txt,.java,.c,.cpp)，使用字符流处理
> 2. 对于非文本文件(.jpg,.mp3,.mp4,.avi,.doc,.ppt,...)，使用字节流处理

```java
/*
实现对图片的复制操作
 */
@Test
public void testFileInputOutputStream()  {
    FileInputStream fis = null;
    FileOutputStream fos = null;
    try {
        //1.造文件
        File srcFile = new File("爱情与友情.jpg");
        File destFile = new File("爱情与友情2.jpg");

        //2.造流
        fis = new FileInputStream(srcFile);
        fos = new FileOutputStream(destFile);

        //3.复制的过程
        byte[] buffer = new byte[5];
        int len;
        while((len = fis.read(buffer)) != -1){
            fos.write(buffer,0,len);
        }

    } catch (IOException e) {
        e.printStackTrace();
    } finally {
        if(fos != null){
            //4.关闭流
            try {
                fos.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
        if(fis != null){
            try {
                fis.close();
            } catch (IOException e) {
                e.printStackTrace();
            }

        }
    }

}
```

<br>

<br>

## 缓冲流的使用

### 相关类

* `BufferedInputStream`
* `BufferedOutputStream`
* `BufferedReader`
* `BufferedWriter`

<br>

### 作用

> 作用：提供流的读取、写入的速度
> 提高读写速度的原因：内部提供了一个缓冲区。默认情况下是8kb

![image-20210923164708948](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241827690.png)_缓冲区大小_

<br>

### 使用

#### 处理非文本文件

```java
	//实现文件复制的方法
    public void copyFileWithBuffered(String srcPath,String destPath){
        BufferedInputStream bis = null;
        BufferedOutputStream bos = null;

        try {
            //1.造文件
            File srcFile = new File(srcPath);
            File destFile = new File(destPath);
            //2.造流
            //2.1 造节点流
            FileInputStream fis = new FileInputStream((srcFile));
            FileOutputStream fos = new FileOutputStream(destFile);
            //2.2 造缓冲流
            bis = new BufferedInputStream(fis);
            bos = new BufferedOutputStream(fos);

            //3.复制的细节：读取、写入
            byte[] buffer = new byte[1024];
            int len;
            while((len = bis.read(buffer)) != -1){
                bos.write(buffer,0,len);
            }
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            //4.资源关闭
            //要求：先关闭外层的流，再关闭内层的流
            if(bos != null){
                try {
                    bos.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }

            }
            if(bis != null){
                try {
                    bis.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }

            }
          //说明：关闭外层流的同时，内层流也会自动的进行关闭。关于内层流的关闭，我们可以省略.
//        fos.close();
//        fis.close();
        }
    }
```

<br>

#### 处理文本文件

```java
	@Test
    public void testBufferedReaderBufferedWriter(){
        BufferedReader br = null;
        BufferedWriter bw = null;
        try {
            //创建文件和相应的流
            br = new BufferedReader(new FileReader(new File("dbcp.txt")));
            bw = new BufferedWriter(new FileWriter(new File("dbcp1.txt")));

            //读写操作
            //方式一：使用char[]数组
//            char[] cbuf = new char[1024];
//            int len;
//            while((len = br.read(cbuf)) != -1){
//                bw.write(cbuf,0,len);
//    //            bw.flush();
//            }

            //方式二：使用String
            String data;
            while((data = br.readLine()) != null){
                //方法一：
//                bw.write(data + "\n");//data中不包含换行符
                //方法二：
                bw.write(data);//data中不包含换行符
                bw.newLine();//提供换行的操作

            }


        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            //关闭资源
            if(bw != null){

                try {
                    bw.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
            if(br != null){
                try {
                    br.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }

            }
        }

    }
```

<br>

<br>

## 转换流的使用

> 1. 转换流：属于**字符流**
>
>    * `InputStreamReader`：将一个字节的输入流转换为字符的输入流
>    * `OutputStreamWriter`：将一个字符的输出流转换为字节的输出流
>
> 2. 作用：提供字节流与字符流之间的**转换**
>
>    ![image-20210923165123040](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241828255.png)_转换流的图示_
>
> 3.  
>
>    * 解码：字节、字节数组  --->字符数组、字符串
>    * 编码：字符数组、字符串 ---> 字节、字节数组
>
> 4. 说明：编码决定了解码的方式

<br>

### 使用

#### InputStreamReader的使用，实现字节的输入流到字符的输入流的转换

```java
    @Test
    public void test1() {

        InputStreamReader isr = null;
        try {
            FileInputStream fis = new FileInputStream("dbcp.txt");
            isr = new InputStreamReader(fis, "utf-8");

            char[] cbuf = new char[30];
            int len;
            while ((len = isr.read(cbuf)) != -1) {
                String str = new String(cbuf, 0, len);
                System.out.print(str);
            }
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            if (isr != null) {
                try {
                    isr.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }
```

<br>

#### 综合使用InputStreamReader和OutputStreamWriter

```java
	@Test
    public void test2() {
        InputStreamReader isr = null;
        OutputStreamWriter osw = null;

        try {
            // 1.造文件造流
            isr = new InputStreamReader(new FileInputStream("src/com/teng/Java2/IO/io/dbcp.txt"), "utf-8");
            osw = new OutputStreamWriter(new FileOutputStream("src/com/teng/Java2/IO/io/dbcp_gbk.txt"), "gbk");

            // 2.读写过程
            char[] cbuf = new char[30];
            int len;
            while ((len = isr.read(cbuf)) != -1) {
                osw.write(cbuf, 0, len);
            }
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            // 3.资源的关闭
            if (osw != null) {
                try {
                    osw.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
            if (isr != null) {
                try {
                    isr.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```

<br>

<br>

## 其他的流的使用

### 标准的输入输出流

> `System.in`:标准的输入流，默认从键盘输入
> `System.out`:标准的输出流，默认从控制台输出
>
> 修改默认的输入和输出行为：
> `System`类的`setIn(InputStream is) / setOut(PrintStream ps)`方式重新指定输入和输出的流。

```java
    @Test
    public void test() {
        BufferedReader br = null;
        try {
            // 1.创建流
            br = new BufferedReader(new InputStreamReader(System.in));

            // 2.处理
            while (true) {
                System.out.print("请输入字符串: ");
                String str = br.readLine();
                if ("e".equalsIgnoreCase(str) || "exit".equalsIgnoreCase(str)) {
                    System.out.println("程序结束");
                    break;
                }
                System.out.println(str.toUpperCase()); // 转换为大写
            }
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            if (br != null) {
                // 3.资源的关闭
                try {
                    br.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }
```

<br>

### 打印流

> `PrintStream` 和  `PrintWriter`
>
> 说明：
>
> * 提供了一系列重载的print()和println()方法，用于多种数据类型的输出
> * `System.out`返回的是`PrintStream`的实例

```java
 	@Test
    public void test2() {
        PrintStream ps = null;
        try {
            FileOutputStream fos = new FileOutputStream(new File("ASCII.txt"));
            // 创建打印输出流,设置为自动刷新模式(写入换行符或字节 '\n' 时都会刷新输出缓冲区)
            ps = new PrintStream(fos, true);
            if (ps != null) {// 把标准输出流(控制台输出)改成文件
                System.setOut(ps);
            }

            for (int i = 0; i <= 255; i++) { // 输出ASCII字符
                System.out.print((char) i);
                if (i % 50 == 0) { // 每50个数据一行
                    System.out.println(); // 换行
                }
            }

        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } finally {
            if (ps != null) {
                ps.close();
            }
        }
    }
```

<br>

### 数据流

> `DataInputStream` 和 `DataOutputStream`
>
> 作用：用于读取或写出基本数据类型的变量或字符串

> 写出到文件

```java
	@Test
    public void test3() {
        DataOutputStream dos = null;
        try {
            // 1.创建流
            dos = new DataOutputStream(new FileOutputStream("src/com/teng/Java2/IO/io/name.txt"));
            // 2.数据操作
            dos.writeUTF("刘欢");
            dos.flush();//刷新操作，将内存中的数据写入文件
            dos.writeInt(18);
            dos.flush();
            dos.writeBoolean(true);
            dos.flush();
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            // 3.资源关闭
            if (dos != null) {
                try {
                    dos.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }
```

<br>

> 从文件读取

```java
    //注意点：读取不同类型的数据的顺序要与当初写入文件时，保存的数据的顺序一致！
    @Test
    public void test4() {
        DataInputStream dis = null;
        try {
            // 1.创建流
            dis = new DataInputStream(new FileInputStream("src/com/teng/Java2/IO/io/name.txt"));
            // 2.数据操作
            String name = dis.readUTF();
            int age = dis.readInt();
            boolean isMale = dis.readBoolean();

            System.out.println("name = " + name);
            System.out.println("age = " + age);
            System.out.println("isMale = " + isMale);
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            if (dis != null) {
                // 3.资源关闭
                try {
                    dis.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```

<br>

<br>

## 对象流的使用

> 1. 对象流：`ObjectInputStream` 和 `ObjectOutputStream`
> 2. 作用
>    * `ObjectOutputStream`:内存中的对象--->存储中的文件、通过网络传输出去：**序列化过程**
>    * `ObjectInputStream`:存储中的文件、通过网络接收过来 --->内存中的对象：**反序列化过程**
> 3. 对象的序列化机制：
>
>    **对象序列化机制**允许把内存中的Java对象转换成平台无关的二进制流，从而允许把这种二进制流持久地保存在磁盘上，或通过网络将这种二进制流传输到另一个网络节点。  
>
> ​    当其它程序获取了这种二进制流，就可以恢复成原来的Java对象

### 示例代码

> **序列化过程**：将内存中的java对象保存到磁盘中或通过网络传输出去

```java
@Test
    public void testObjectOutputStream() {

        ObjectOutputStream oos = null;
        try {
            // 创造流
            oos = new ObjectOutputStream(new FileOutputStream("object.dat"));
            // 创造对象
            oos.writeObject(new String("我是中国人,我感到很自豪。"));
            // 刷新操作
            oos.flush();

            oos.writeObject(new Person("张三", 18));
            oos.flush();

            // 这里需要一个Account类
            oos.writeObject(new Person("张三", 18, new Account(180)));
            oos.flush();

        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            if (oos != null) {
                try {
                    oos.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }
```

<br>

> **反序列化**：将磁盘文件中的对象还原为内存中的一个java对象

```java
    @Test
    public void testObjectInputStream() {
        ObjectInputStream ois = null;
        try {
            ois = new ObjectInputStream(new FileInputStream("object.dat"));

            Object obj = ois.readObject();
            String str = (String) obj;
            System.out.println(str);

            Person person1 = (Person) ois.readObject();
            System.out.println(person1);

            Person person2 = (Person) ois.readObject();
            System.out.println(person2);



        } catch (IOException e) {
            e.printStackTrace();
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        } finally {
            if (ois != null) {

                try {
                    ois.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```

<br>

### 实现序列化的对象所属的类需要满足的条件（重要）

<details>
	<summary>点击查看</summary>
    <ul>
            <ol>
            	<li>需要实现接口：Serializable</li>
                <li>当前类提供一个全局常量：serialVersionUID</li>
                <li>除了当前Person类需要实现Serializable接口之外，还必须保证其内部所属性也必须是可序列化的。（默认情况下，基本数据类型可序列化）</li>
            </ol>
        <b>补充：ObjectOutputStream和ObjectInputStream不能序列化static和transient修饰的成员变量</b>
    </ul>
</details>

<br>

<br>

## RandomAccessFile的使用

> 1. 随机存取文件流：`RandomAccessFile`
>
> 2. 使用说明
>
>    1. RandomAccessFile直接继承于java.lang.Object类，实现了DataInput和DataOutput接
>    2. RandomAccessFile既可以作为一个输入流，又可以作为一个输出流
>    3. 
>       1. 如果RandomAccessFile作为输出流时，写出到的文件如果不存在，则在执行过程中自动创建。
>       2. 如果写出到的文件存在，则会对**原有文件内容进行覆盖**。（默认情况下，从头覆盖）
>
>    4. 可以通过相关的操作，实现`RandomAccessFile`“插入”数据的效果

### 示例代码

```java
@Test
public void test1() {
    RandomAccessFile raf1 = null; // 只读
    RandomAccessFile raf2 = null; // 可读可写
    try {
        // 1.实例化流对象
        raf1 = new RandomAccessFile(new File("蜡笔小新.jpg"), "r");
        raf2 = new RandomAccessFile(new File("蜡笔小新1.jpg"), "rw");

        // 2.文件操作
        byte[] buffer = new byte[1024];
        int len;
        while ((len = raf1.read(buffer)) != -1) {
            raf2.write(buffer);
        }
    } catch (IOException e) {
        e.printStackTrace();
    } finally {
        // 3.资源的关闭
        if (raf1 != null) {
            try {
                raf1.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
        if (raf2 != null) {
            try {
                raf2.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
}
```

<br>

> 使用RandomAccressFile实现数据的插入效果

```java
@Test
public void test3() {
    RandomAccessFile raf = null;
    try {
        raf = new RandomAccessFile(new File("shello.txt"), "rw");

        raf.seek(3); // 将指针调到角标3的位置
        // 保存角标3后面的所有数据到StringBuilder中
        StringBuilder builder = new StringBuilder((int) new File("src/com/teng/Java2/IO/io1/hello.txt").length());
        byte[] buffer = new byte[20];
        int len;
        while ((len = raf.read(buffer)) != -1) {
            builder.append(new String(buffer, 0, len));
        }
        // 调回指针
        raf.seek(3);
        raf.write("xyz".getBytes());

        // 将StringBuilder中的数据写入文件
        raf.write(builder.toString().getBytes());
    } catch (IOException e) {
        e.printStackTrace();
    } finally {
        if (raf != null) {
            // 资源关闭
            try {
                raf.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
}
```

<br>

> 思考：将`StringBuilder`替换为`ByteArrayOutputStream`

```java
	@Test
	public void test1() throws Exception {
		FileInputStream fis = new FileInputStream("abc.txt");
		String info = readStringFromInputStream(fis);
		System.out.println(info);
	}

	private String readStringFromInputStream(FileInputStream fis) throws IOException {
		// 方式一：可能出现乱码
		// String content = "";
		// byte[] buffer = new byte[1024];
		// int len;
		// while((len = fis.read(buffer)) != -1){
		// content += new String(buffer);
		// }
		// return content;

		// 方式二：BufferedReader
		BufferedReader reader = new BufferedReader(new InputStreamReader(fis));
		char[] buf = new char[10];
		int len;
		String str = "";
		while ((len = reader.read(buf)) != -1) {
			str += new String(buf, 0, len);
		}
		return str;

		// 方式三：避免出现乱码
		// ByteArrayOutputStream baos = new ByteArrayOutputStream();
		// byte[] buffer = new byte[10];
		// int len;
		// while ((len = fis.read(buffer)) != -1) {
		// baos.write(buffer, 0, len);
		// }
		//
		// return baos.toString();
	}
```

<br><br>
