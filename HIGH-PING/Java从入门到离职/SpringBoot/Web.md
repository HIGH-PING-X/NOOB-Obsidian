默认情况下，SpringBoot 从类路径中的 /static、/public、/resources、/META-INF/resources 访问 url 为当前项目的根路径/ + 静态资源名

```yml
spring:  
  mvc:  
    #为静态资源添加前缀  
    static-path-pattern: /noob/**  
  web:  
    resources:  
		#关闭静态资源  
		add-mappings: false
	    #改变SpringBoot默认的静态资源目录  
	    static-locations: [classpath:/noob/]

#也能使用webjars但是用的不多
```


