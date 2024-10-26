获取Session对象的过程
```java
package noobMybatis;  
  
import org.apache.ibatis.io.Resources;  
import org.apache.ibatis.session.SqlSession;  
import org.apache.ibatis.session.SqlSessionFactory;  
import org.apache.ibatis.session.SqlSessionFactoryBuilder;  
  
import java.io.IOException;  
  
public class NoobMybatisInsert {  
public static void main(String[] args) {  
SqlSession sqlSession = null;  
try {  
//获取SqlSessionFactoryBuilder对象  
SqlSessionFactoryBuilder sqlSessionFactoryBuilder = new SqlSessionFactoryBuilder();  
//获取SqlSessionFactory对象  
SqlSessionFactory sqlSessionFactory = sqlSessionFactoryBuilder.build(Resources.getResourceAsStream("mybatis-config.xml"));  
sqlSession = sqlSessionFactory.openSession();  
int insertCar = sqlSession.insert("insertCar");  
System.out.println(insertCar);  
sqlSession.commit();  
} catch (IOException e) {  
sqlSession.rollback();  
throw new RuntimeException(e);  
} finally {  
if (sqlSession != null) {  
sqlSession.close();  
}  
}  
}  
}
```
创建mysql表
```MySQL
create table car_data  
(  
id bigint unsigned auto_increment  
primary key,  
carnum varchar(50) not null comment '汽车编号',  
brand varchar(100) not null comment '品牌',  
guideprice decimal(10, 2) not null comment '厂家指导价',  
producetime char(10) not null comment '生产日期',  
cartype varchar(50) not null comment '汽车类型'  
)  
comment '汽车信息表' charset = utf8mb4;
```
重置mysql表的自增
```MySQL
ALTER TABLE table_name AUTO_INCREMENT = 1;
```
查看表当前的自增值
```MySQL
SELECT AUTO_INCREMENT FROM information_schema.tables WHERE table_schema = 'database_name' AND table_name = 'table—_name';
```
修改MySQL表的字段名
```MySQL
ALTER TABLE table_name CHANGE [原字段名] [修改后字段名] varchar(50) NOT NULL;
```
创建SqlSession工具类
```java
package noobMybatis.utils;  
  
import org.apache.ibatis.io.Resources;  
import org.apache.ibatis.session.SqlSession;  
import org.apache.ibatis.session.SqlSessionFactory;  
import org.apache.ibatis.session.SqlSessionFactoryBuilder;  
  
import java.io.IOException;  
  
public class SqlSessionUtil {  
public SqlSessionUtil() {  
}  
private static final SqlSessionFactory sqlSessionFactory;  
static {  
try {  
sqlSessionFactory = new SqlSessionFactoryBuilder().build(Resources.getResourceAsStream("mybatis-config.xml"));  
} catch (IOException e) {  
throw new RuntimeException(e);  
}  
}  
public static SqlSession openSession() {  
return sqlSessionFactory.openSession();  
}  
}
```
编写Mapper.xml
```xml
<?xml version="1.0" encoding="UTF-8" ?>  
<!DOCTYPE mapper  
PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"  
"http://mybatis.org/dtd/mybatis-3-mapper.dtd">  
  
<mapper namespace="noob">  
  
<insert id="insertCar">  
insert into car_data(id,carnum,brand,guideprice,producetime,cartype)  
values (null,#{carNum},#{brand},#{guidePrice},#{produceTime},#{carType})  
</insert>  
  
<delete id="deleteCar">  
delete from car_data where id = #{noob}  
</delete>  
  
<update id="updateCar">  
update car_data set carnum = #{carNum},brand = #{brand},guideprice = #{guidePrice},cartype = #{carType} where id = #{id}  
</update>  
  
<select id="selectCar" resultType="noobMybatis.pojo.Car">  
select * from car_data where id = #{id}  
</select>  
  
<select id="selectCarAll" resultType="noobMybatis.pojo.Car">  
select * from car_data  
</select>  
  
</mapper>
```
创建test类
```java
package noobMybatis;  
  
import noobMybatis.pojo.Car;  
import noobMybatis.utils.SqlSessionUtil;  
import org.apache.ibatis.session.SqlSession;  
import org.junit.Test;  
  
import java.util.List;  
  
public class CarMapperText {  
  
@Test  
public void TestCarInsert() {  
SqlSession sqlSession = SqlSessionUtil.openSession();  
Car car = new Car(null,"赣B00000","领克",190000.00,"2023-8-2","新能源");  
int insertCar = sqlSession.insert("insertCar",car);  
System.out.println("添加" + insertCar + "条数据");  
sqlSession.commit();  
sqlSession.close();  
}  
  
@Test  
public void TestCarDelete() {  
SqlSession sqlSession = SqlSessionUtil.openSession();  
int deleteCar = sqlSession.delete("deleteCar",6L);  
System.out.println("删除了" + deleteCar + "条数据");  
sqlSession.commit();  
sqlSession.close();  
}  
  
@Test  
public void TestCarUpdate() {  
SqlSession sqlSession = SqlSessionUtil.openSession();  
Car car = new Car(5L,"赣B11111","奔驰",500000.00,"2023-8-2","越野");  
int updateCar = sqlSession.update("updateCar",car);  
System.out.println("更新了" + updateCar + "条数据");  
sqlSession.commit();  
sqlSession.close();  
}  
  
@Test  
public void TestCarSelect() {  
SqlSession sqlSession = SqlSessionUtil.openSession();  
Object selectCar = sqlSession.selectOne("selectCar", 1L);  
System.out.println(selectCar);  
sqlSession.close();  
}  
  
@Test  
public void TestCarSelectAll() {  
SqlSession sqlSession = SqlSessionUtil.openSession();  
List<Object> selectCarAll = sqlSession.selectList("noob.selectCarAll");  
selectCarAll.forEach(System.out::println);  
sqlSession.close();  
}  
}
```