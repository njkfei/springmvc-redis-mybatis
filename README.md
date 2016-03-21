# springmvc-redis-mybatis
springmvc redis mybatis springboot 集成demo.
* redis提供缓存
* mybatis提供ORM框架
 在8G内存机器下，QPS性能轻松达到2K.

## redis依赖
```
		<dependency>
			<groupId>org.springframework.data</groupId>
			<artifactId>spring-data-redis</artifactId>
		</dependency>

		<dependency>
			<groupId>redis.clients</groupId>
			<artifactId>jedis</artifactId>
		</dependency>
```

## springboot版本
```
	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>1.2.2.RELEASE</version>
	</parent>
```

## 缓存配置说明
```
  @Bean
  public JedisConnectionFactory redisConnectionFactory() {
    JedisConnectionFactory redisConnectionFactory = new JedisConnectionFactory();

    // Defaults
    redisConnectionFactory.setHostName("127.0.0.1");
    redisConnectionFactory.setPort(6379);
    return redisConnectionFactory;
  }

  @Bean
  public RedisTemplate<String, String> redisTemplate(RedisConnectionFactory cf) {
    RedisTemplate<String, String> redisTemplate = new RedisTemplate<String, String>();
    redisTemplate.setConnectionFactory(cf);
    return redisTemplate;
  }

  @Bean
  public CacheManager cacheManager(RedisTemplate<String, String> redisTemplate) {
    RedisCacheManager cacheManager = new RedisCacheManager(redisTemplate);

    cacheManager.setDefaultExpiration(300); // 设置失效时间
    return cacheManager;
  }
```

## 测试
```
ab -c 100 -n 1000000 192.168.1.236:8080/user/972
```
启动测试后，cpu占有率达到70左右。
