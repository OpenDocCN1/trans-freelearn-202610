```
jedisConnectionFactory();
template.setKeySerializer(
    new StringRedisSerializer());
template.setHashKeySerializer(
    new StringRedisSerializer());
template.setHashKeySerializer(
    new JdkSerializationRedisSerializer());
template.setValueSerializer(
    new JdkSerializationRedisSerializer());
template.setEnableTransactionSupport(true);
template.afterPropertiesSet();

    return template;
}
}
```

5. The last thing we need to do is add the @RedisHash annotation to our entity. This serves as a mark for objects as an aggregate root to be stored on the Redis hash; in our example, we will use it on AntiHeroEntity, as follows:

```
@RedisHash("AntiHero")
public class AntiHeroEntity {
...
}
```

And this will successfully use the Redis server to cache data on our Spring Boot application as operations are executed.

# Summary

With this, we have reached the end of this chapter. Let’s have a recap of the valuable things you have learned. You have learned how to start the Spring Boot application on a Tomcat server using IntelliJ. You have also known how to create a complete Spring Boot application step by step by creating entities, using Lombok and DTOs, writing services with CRUD logic with the help of JPA repositories, and creating controllers using HTTP annotations and ModelMapper for converting entities to DTO, and vice versa.

Lastly, you have also learned how to configure a Redis server and use it on a Spring Boot application.

The skills you have learned here will make your code readable and concise because of the Lombok and JPA repositories.

In the next chapter, we will be learning how to create documentation for our APIs using springdoc-openapi and Swagger UI.