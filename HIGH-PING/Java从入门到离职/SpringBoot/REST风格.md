GET 查询
POST 新增/保存
PUT 修改/更新
DELETE 删除

@RequestParam 用于接收 url 地址传参或表单传参
@RequestBody 用于接收 json 数据
@PathVariable 用于接收/获取路径参数，使用{参数名称}描述路径参数参数路径
```java
@GetMapping("/{name}")  
private String Noob(@PathVariable String name) {
	System.out.println(name);  
	return "NOOB";  
}
```
与@PathVariable 类似的还有:
	@RequestHeader  获取请求头
	@RequestParam  获取请求参数
	@CookieValue  获取cookie值
	@RequestBody  获取请求体
	@MatrixVariable  获取矩阵变量
	@RequestAttribute  获取 request 域属性

使用@MatrixVariable 注解
```java
package games.highping.springboot00.controller;  
  
import org.springframework.web.bind.annotation.*;  
  
import java.util.HashMap;  
import java.util.List;  
import java.util.Map;  
  
@RestController  
public class MatrixVariableController {  
  
    //http://localhost:1999/MatrixVariable/noob;name=noob,xy;number=1  
    @GetMapping("/MatrixVariable/{path}")  
    public Map MatrixVariable(@MatrixVariable("name") List<String> name,  
                              @MatrixVariable("number") Integer number,  
                              @PathVariable("path") String path) {  
        Map<String,Object> map = new HashMap<>();  
        map.put("name",name);  
        map.put("number",number);  
        map.put("path",path);  
        return map;  
    }  
  
}
```
```java
//使用@MatrixVariable可能会需要以下配置，主要为把setRemoveSemicolonContent改为false
package games.highping.springboot00.config;  
  
import org.springframework.context.annotation.Bean;  
import org.springframework.context.annotation.Configuration;  
import org.springframework.web.servlet.config.annotation.PathMatchConfigurer;  
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;  
import org.springframework.web.util.UrlPathHelper;  
  
@Configuration(proxyBeanMethods = false)  
public class MatrixVariableConfig implements WebMvcConfigurer {  
  
    @Override  
    public void configurePathMatch(PathMatchConfigurer configurer) {  
        UrlPathHelper urlPathHelper = new UrlPathHelper();  
        urlPathHelper.setRemoveSemicolonContent(false);  
        configurer.setUrlPathHelper(urlPathHelper);  
    }  
      
    @Bean  
    public WebMvcConfigurer webMvcConfigurer() {  
        return new WebMvcConfigurer() {  
            @Override  
            public void configurePathMatch(PathMatchConfigurer configurer) {  
                UrlPathHelper urlPathHelper = new UrlPathHelper();  
                urlPathHelper.setRemoveSemicolonContent(false);  
                configurer.setUrlPathHelper(urlPathHelper);  
            }  
        };  
    }  
      
}
```

@RestController 同时代替@Controller 和@ResponseBody

开启页面表单的 REST 风格
```yml
spring:   
  mvc:  
    hiddenmethod:  
      filter:  
        enabled: true
```
```html
<form action="/noob" method="post">  
    <input name="_method" type="hidden" value="Put">  
    <input value="Put" type="submit">  
</form>  
  
<form action="/noob" method="post">  
    <input name="_method" type="hidden" value="DELETE">  
    <input value="Delete" type="submit">  
</form>
```
如果要改变 HTML 中的_method 可以重写 HiddenHttpMethodFilter 方法
```java
package games.highping.springboot00.config;  
  
import org.springframework.context.annotation.Bean;  
import org.springframework.context.annotation.Configuration;  
import org.springframework.web.filter.HiddenHttpMethodFilter;  
  
@Configuration(proxyBeanMethods = false)  
public class hiddenHttpMethodFilterConfig {  
  
    @Bean  
    public HiddenHttpMethodFilter hiddenHttpMethodFilter() {  
        HiddenHttpMethodFilter hiddenHttpMethodFilter = new HiddenHttpMethodFilter();  
        hiddenHttpMethodFilter.setMethodParam("自定义名称");  
        return hiddenHttpMethodFilter;  
    }  
  
}
```