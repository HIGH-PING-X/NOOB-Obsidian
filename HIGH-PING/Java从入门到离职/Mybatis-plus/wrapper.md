在 MyBatis-Plus 中，`wrapper` 是一个查询条件构造器，用于构建动态 SQL 查询条件。

###### QueryWrapper 方法：
eq(column, value): 等于条件
ne(column, value): 不等于条件
gt(column, value): 大于条件
ge(column, value): 大于等于条件
lt(column, value): 小于条件
le(column, value): 小于等于条件
between(column, val1, val2): 在某个范围内条件
notBetween(column, val1, val2): 不在某个范围内条件
like(column, value): 模糊查询条件
notLike(column, value): 不匹配模糊查询条件
isNull(column): 为NULL条件
isNotNull(column): 不为NULL条件
in(column, values): 包含在集合中条件
notIn(column, values): 不包含在集合中条件
or(): 与上一个条件之间的 OR 连接
and(): 与上一个条件之间的 AND 连接
nested(): 嵌套查询条件

###### UpdateWrapper 方法：
eq(column, value): 等于条件
ne(column, value): 不等于条件
gt(column, value): 大于条件
ge(column, value): 大于等于条件
lt(column, value): 小于条件
le(column, value): 小于等于条件
between(column, val1, val2): 在某个范围内条件
notBetween(column, val1, val2): 不在某个范围内条件
like(column, value): 模糊查询条件
notLike(column, value): 不匹配模糊查询条件
isNull(column): 为NULL条件
isNotNull(column): 不为NULL条件
in(column, values): 包含在集合中条件
notIn(column, values): 不包含在集合中条件
or(): 与上一个条件之间的 OR 连接
and(): 与上一个条件之间的 AND 连接
nested(): 嵌套查询条件
set(column, value): 更新某个字段的值
setSql(column, sql): 使用 SQL 片段更新字段的值

```java
package games.highping.mybatisplus00;  
  
import com.baomidou.mybatisplus.core.conditions.query.QueryWrapper;  
import games.highping.mybatisplus00.bean.User;  
import games.highping.mybatisplus00.mapper.UserMapper;  
import org.junit.jupiter.api.Test;  
import org.springframework.beans.factory.annotation.Autowired;  
import org.springframework.boot.test.context.SpringBootTest;  
  
import java.util.List;  
  
@SpringBootTest  
public class MybatisPlusWrapper {  
  
    @Autowired  
    private UserMapper userMapper;  
  
    @Test  
    void testWrapperSelectList() {  
        QueryWrapper<User> queryWrapper = new QueryWrapper<>();  
        queryWrapper.like("username","NOOB").isNotNull("email");  
        List<User> list = userMapper.selectList(queryWrapper);  
        list.forEach(System.out::println);  
    }  
  
    @Test  
    //排序  
    void testWrapperRank() {  
        QueryWrapper<User> wrapper = new QueryWrapper<>();  
        wrapper.orderByDesc("username").orderByAsc("id");  
        List<User> list = userMapper.selectList(wrapper);  
        list.forEach(System.out::println);  
    }  
  
    @Test  
    void testWrapperDelete() {  
        QueryWrapper<User> wrapper = new QueryWrapper<>();  
        wrapper.isNull("username");  
        System.out.println(userMapper.delete(wrapper));  
    }  
  
    @Test  
    void testWrapperUpdate() {  
        QueryWrapper<User> wrapper = new QueryWrapper<>();  
        wrapper.eq("username","NOOB0");  
        User user = new User();  
        user.setEmail("0000@noob.com");  
        System.out.println(userMapper.update(user, wrapper));  
    }  
  
}
```