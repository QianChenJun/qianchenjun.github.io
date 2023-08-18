---
title: 分布式锁+AOP实现缓存
tags:
  - AOP
  - 分布式锁
categories:
  - Java
abbrlink: 32677e9a
date: 2023-08-18 09:45:31
updated: 2023-08-18 09:45:31
---

> 由于将数据加入缓存的代码存在通用性，所以我们通过定义一个AOP（注解方式）来简化这部分代码的开发。
>
> 其使用原理类似于`@TransactionManager`开启事务
>
> ![image-20230405155806192](https://qianchen-image.oss-cn-beijing.aliyuncs.com/blog/202304051558287.png)

*<!--more-->*

### 定义注解@GmallCache

```java
@Target({ElementType.METHOD})
@Retention(RetentionPolicy.RUNTIME)
@Inherited
@Documented
public @interface GmallCache {
    String prefix() default "cache:";
    String suffix() default ":info";
}
```

### 定义切面

```java
import com.alibaba.fastjson.JSON;
import com.atguigu.gmall.common.constant.RedisConst;
import org.apache.commons.lang.StringUtils;
import org.aspectj.lang.ProceedingJoinPoint;
import org.aspectj.lang.annotation.Around;
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.reflect.MethodSignature;
import org.redisson.api.RLock;
import org.redisson.api.RedissonClient;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.stereotype.Component;

import java.util.Arrays;
import java.util.concurrent.TimeUnit;

@Component
@Aspect
public class GmallCacheAspect {

    @Autowired
    private RedisTemplate redisTemplate;

    @Autowired
    private RedissonClient redissonClient;

    // 定义一个环绕通知
    @Around("@annotation(com.atguigu.gmall.common.cache.GmallCache)")
    public Object gmallCacheAspectMethod(ProceedingJoinPoint joinPoint) throws Throwable {
        Object obj = null;
        /*
         业务逻辑！
         1. 必须先知道这个注解在哪些方法 || 必须要获取到方法上的注解
         2. 获取到注解上的前缀
         3. 必须要组成一个缓存的key！
         4. 可以通过这个key 获取缓存的数据
            true:
                直接返回！
            false:
                分布式锁业务逻辑！
         */
        // 拼接存入Redis的key
        MethodSignature signature = (MethodSignature) joinPoint.getSignature();
        // 获取注解对象
        GmallCache gmallCache = signature.getMethod().getAnnotation(GmallCache.class);
        // 获取前缀
        String prefix = gmallCache.prefix();
        // 获取后缀
        String suffix = gmallCache.suffix();
        // 获取方法传递的参数
        Object[] args = joinPoint.getArgs();
        // 拼接key
        String key = prefix + Arrays.asList(args) + suffix;

        // 查询缓存中的数据
        obj = getRedisData(key, signature);

        try {
            if (obj == null) {
                // 缓存中没有对应数据，调用数据库查询数据，并且将数据放到缓存中
                // 分布式锁操作数据库（防止缓存穿透）
                RLock lock = redissonClient.getLock(prefix + Arrays.asList(args) + ":lock");

                // 调用tryLock
                boolean res = lock.tryLock(RedisConst.SKULOCK_EXPIRE_PX1, RedisConst.SKULOCK_EXPIRE_PX2, TimeUnit.SECONDS);

                if (res) {
                    // 如果获取锁成功，执行业务逻辑（查询数据库）
                    obj = joinPoint.proceed(args);

                    try {
                        if (obj == null) {
                            // 如果数据库不存在对应数据
                            // 这个地方需要注意返回对应类型的数据，否则会出现ClassCastException
                            // 获取返回值类的字节码对象
                            Class aClass = signature.getReturnType();
                            obj = aClass.newInstance();

                            // 将对象放入缓存
                            redisTemplate.opsForValue().set(key, JSON.toJSONString(obj), RedisConst.SKUKEY_TEMPORARY_TIMEOUT, TimeUnit.SECONDS);

                        } else {
                            // 如果数据库存在对应数据
                            // 将对象放入缓存
                            redisTemplate.opsForValue().set(key, JSON.toJSONString(obj), RedisConst.SKUKEY_TIMEOUT, TimeUnit.SECONDS);
                        }
                        // 返回对应数据
                        return obj;
                    } finally {
                        // 释放锁
                        lock.unlock();
                    }
                } else {
                    // 如果获取锁失败
                    Thread.sleep(100);
                    return gmallCacheAspectMethod(joinPoint);
                }

            } else {
                // 缓存中存在对应数据，直接返回缓存中的数据
                return obj;
            }
        } catch (Throwable e) {
            e.printStackTrace();
        }
        // 如果执行到了此行，说明程序出现了异常，调用对应的数据库操作方法兜底即可
        return joinPoint.proceed(args);
    }

    /**
     * 查询缓存中的数据
     * @param key
     * @param signature
     * @return
     */
    private Object getRedisData(String key, MethodSignature signature) {
        // 数据存入Redis的时候是Json字符串
        String strJson = (String) redisTemplate.opsForValue().get(key);

        // 判断数据
        if (!StringUtils.isEmpty(strJson)) {
            // 将Json字符串转成对应的数据类型
            return JSON.parseObject(strJson, signature.getReturnType());
        }

        // 数据为空返回null
        return null;
    }
}
```

> 上述代码主要实现了缓存的逻辑，以及调用原方法查询数据库时可能出现**缓存穿透**等问题做了优化。
>
> **将该注解放到某一个查询数据库的方法之上即可完成添加缓存的操作。**
>
> ![image-20230405160825253](https://qianchen-image.oss-cn-beijing.aliyuncs.com/blog/202304051608304.png)
