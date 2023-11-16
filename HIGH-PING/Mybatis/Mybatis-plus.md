
首先构建简单表
```sql
CREATE TABLE `user` (
  `user_id` bigint NOT NULL,
  `username` varchar(50) NOT NULL,
  `email` varchar(100) NOT NULL,
  `date_registered` date NOT NULL,
  PRIMARY KEY (`user_id`)
) ENGINE=InnoDB AUTO_INCREMENT=7 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci
```

向 pom.xml 导入依赖
```xml
<dependency>  
    <groupId>com.baomidou</groupId>  
    <artifactId>mybatis-plus-boot-starter</artifactId>  
    <version>3.5.4</version>  
</dependency>  
<dependency>  
    <groupId>com.mysql</groupId>  
    <artifactId>mysql-connector-j</artifactId>  
</dependency>
```

添加 bean 并启用 lombok 注解
```java
package games.highping.mybatisplus00.bean;  
  
import lombok.Data;  
  
import java.sql.Date;  
  
@Data  
public class User {  
    private Integer userId;  
    private String username;  
    private String email;  
    private Date dateRegistered;  
}
```

添加 mapper
```java
package games.highping.mybatisplus00.mapper;  
  
import com.baomidou.mybatisplus.core.mapper.BaseMapper;  
import games.highping.mybatisplus00.bean.User;  
import org.springframework.stereotype.Repository;  
  
@Repository  
public interface UserMapper extends BaseMapper<User> {  
}
```

在 Application 中添加注解
```java
@MapperScan("games.highping.mybatisplus00.mapper")
```

配置
```yml
spring:  
  datasource:  
    driver-class-name: com.mysql.cj.jdbc.Driver  
    url: jdbc:mysql://localhost:3306/mybatisplus?serverTimezone=GMT%2B8&characterEncoding=utf-8&userSSL=false  
    username: root  
    password: '0000'  
    type: com.zaxxer.hikari.HikariDataSource  
mybatis-plus:  
  configuration:  
    log-impl: org.apache.ibatis.logging.stdout.StdOutImpl
```

初始项目结构
![[Pasted image 20231115151934.png]]