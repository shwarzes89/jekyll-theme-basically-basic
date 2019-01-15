---
layout: post
title: Spring boot Mongo repository NoSuchBeanException occured
description: "Fix NoSuchBeanException with annotation"
tags: [spring boot, mongoDB]
---

When i used this code snippet to configure MongoDB, NoSuchBeanException occurred

```java
@Configuration
@EnableMongoRepositories
public class MongoConfig {
    @Bean
    public MongoClientOptions mongoOptions() {
        return MongoClientOptions.builder()
                .connectionsPerHost(30)
                .threadsAllowedToBlockForConnectionMultiplier(40)
                .build();
    }
}
```

Because the package conflicts with default configuration.
To remove NoSuchBeanException, you can fix the problem by explicitly writing the package name or removing this annotation.

```java
@Configuration
public class MongoConfig {
    @Bean
    public MongoClientOptions mongoOptions() {
        return MongoClientOptions.builder()
                .connectionsPerHost(30)
                .threadsAllowedToBlockForConnectionMultiplier(40)
                .build();
    }
}
```

or

```java
@Configuration
@EnableMongoRepositories(basePackages = "my.project.repository")
public class MongoConfig {
    @Bean
    public MongoClientOptions mongoOptions() {
        return MongoClientOptions.builder()
                .connectionsPerHost(30)
                .threadsAllowedToBlockForConnectionMultiplier(40)
                .build();
    }
}
```
