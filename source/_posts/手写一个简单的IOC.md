---
title: 手写一个简单的IOC
tags:
  - IOC
  - Spring
categories:
  - Java
abbrlink: 1b8a0000
date: 2023-04-12 15:51:21
updated: 2023-04-12 15:51:21
---

*<!--more-->*

## 自定义两个注解

### @Bean

> 类似于Spring中的@Component

```java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
public @interface Bean {
}
```



### @Di

> 类似于Spring中的@Autowired

```java
@Target({ElementType.FIELD})
@Retention(RetentionPolicy.RUNTIME)
public @interface Di {
}
```



## Dao

```java
public interface UserDao {
    void print();
}

@Bean
public class UserDaoImpl implements UserDao {
    @Override
    public void print() {
        System.out.println("Dao层代码执行结束...");
    }
}
```



## Service

```java
public interface UserService {
    void out();
}

@Bean
public class UserServiceImpl implements UserService {

    @Di
    private UserDao userDao;

    @Override
    public void out() {
        userDao.print();
        System.out.println("Service层代码执行完毕...");
    }
}

```



## 核心包

### ApplicationContext

```java
public interface ApplicationContext {
    Object getBean(Class clazz);
}
```



### AnnotationApplicationContext

```Java
import tk.qianchen.ioc.inno.Bean;
import tk.qianchen.ioc.inno.Di;

import java.io.File;
import java.io.IOException;
import java.lang.reflect.Field;
import java.net.URL;
import java.net.URLDecoder;
import java.util.Enumeration;
import java.util.HashMap;
import java.util.Map;

/**
 * @Date: 2023/4/11
 * @Author: Thousand_Star
 * @Description:
 */
public class AnnotationApplicationContext implements ApplicationContext {

    // 存储Bean的容器
    private Map<Class, Object> beanFactory = new HashMap<>();

    // 文件根路径
    private static String rootPath;

    /**
     * 根据class获取容器中的Bean
     * @param clazz
     * @return
     */
    @Override
    public Object getBean(Class clazz) {
        return beanFactory.get(clazz);
    }

    /**
     * 根据包路径扫描加载Bean
     * 扫描当前包及其子包，将所有标有@Bean的类都通过反射进行实例化
     * @param basePackage tk.qianchen
     */
    public AnnotationApplicationContext(String basePackage) {
        try {
            // 将传递的路径中的.变成\  --> tk.qianchen -> tk\qianchen
            String packagePath = basePackage.replaceAll("\\.", "\\\\");

            // 根据当前线程获取全路径集合
            Enumeration<URL> urls = Thread.currentThread().getContextClassLoader().getResources(packagePath);

            while (urls.hasMoreElements()) {
                // 获取全路径（全路径 = 根路径 + 包路径）
                URL url = urls.nextElement();

                // 将字符转义
                String filePath = URLDecoder.decode(url.getFile(), "utf-8");

                // 保存根路径（除去包路径剩下的部分）
                rootPath = filePath.substring(0, filePath.length() - packagePath.length());

                // 加载bean
                loadBean(new File(filePath));
            }
        } catch (IOException e) {
            throw new RuntimeException(e);
        }

        // 依赖注入
        loadDi();

    }

    private void loadDi() {
        // 1. 遍历容器集合
        for (Map.Entry<Class, Object> entry : beanFactory.entrySet()) {
            // 2. 取出每一个value
            Object obj = entry.getValue();

            Class<?> clazz = obj.getClass();

            // 3. 获取成员变量列表
            Field[] declaredFields = clazz.getDeclaredFields();

            // 4. 遍历成员变量列表
            for (Field field : declaredFields) {
                Di annotation = field.getAnnotation(Di.class);
                if (annotation != null) {
                    field.setAccessible(true);

                    // 5. 给该变量赋值
                    try {
                        field.set(obj, beanFactory.get(field.getType()));
                    } catch (IllegalAccessException e) {
                        throw new RuntimeException(e);
                    }
                }
            }
        }
    }

    /**
     * 将Bean保存到容器中
     * @param file
     */
    private void loadBean(File file) {
        // 1.判断file对象是否是文件夹
        if (file.isDirectory()) {
            // 2.如果是文件夹，获取子文件列表
            File[] childrenFiles = file.listFiles();

            // 3.判断子文件列表是否为空
            if (childrenFiles == null || childrenFiles.length == 0) {
                // 4.如果为空，则返回
                return;
            }
            // 5.如果不为空，遍历文件夹
            for (File childrenFile : childrenFiles) {
                // 5.1 遍历得到每个File文件，如果是文件夹，递归
                if (childrenFile.isDirectory()) {
                    loadBean(childrenFile);
                }

                // 5.2 如果是文件，得到包路径+类名称部分 -- 截取根路径
                String pathWithClass = childrenFile.getAbsolutePath().substring(rootPath.length() - 1);

                // 5.3 判断当前文件是否是.class类型
                if (pathWithClass.contains(".class")) {
                    // 5.4 如果是.class类型，则将路径中的\替换成. 并且将.class去掉
                    String fullName = pathWithClass
                            .replaceAll("\\\\", ".")
                            .replace(".class", "");

                    // 5.5 判断类上面是否有@Bean注解，如果有则实例化
                    try {
                        Class<?> clazz = Class.forName(fullName);

                        // 5.6 如果当前类不是接口
                        if (!clazz.isInterface()) {
                            Bean bean = clazz.getAnnotation(Bean.class);
                            if (bean != null) {
                                Object instance = clazz.newInstance();
                                // 5.6 将对象实例化后放到map集合中
                                // 判断一下当前实例有没有接口，有则作为key
                                if (clazz.getInterfaces().length > 0) {
                                    beanFactory.put(clazz.getInterfaces()[0], instance);
                                } else {
                                    beanFactory.put(clazz, instance);
                                }

                            }
                        }
                    } catch (Exception e) {
                        throw new RuntimeException(e);
                    }
                }
            }
        }
    }
}
```



## 测试文件

```java
public class TestUser {
    public static void main(String[] args) {
        ApplicationContext context = new AnnotationApplicationContext("tk.qianchen");
        UserService bean = (UserService) context.getBean(UserService.class);
        bean.out();
    }
}
```

![image-20230412201010885](https://qianchen-image.oss-cn-beijing.aliyuncs.com/blog/202304122010105.png)
