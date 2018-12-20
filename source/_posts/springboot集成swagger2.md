---
title: springboot集成swagger2
date: 2018-12-20 17:06:09
categories: java
tags: [swagger, springboot]
---





# 1.pom依赖

```java
<!-- swagger 依赖 -->
<dependency>
	<groupId>io.springfox</groupId>
	<artifactId>springfox-swagger2</artifactId>
	<version>2.8.0</version>
</dependency>
<dependency>
	<groupId>io.springfox</groupId>
	<artifactId>springfox-swagger-ui</artifactId>
	<version>2.8.0</version>
</dependency>
```



# 2.springboot配置文件

## application.yml

```java
spring:
  profiles:
    active: test
swagger:
  title: swagger
  version: 1.0.0
```

# 3.swagger配置类

```java
@Configuration
@EnableSwagger2
@Profile({"test", "dev"})
public class Swagger2Config {

    @Value("${swagger.title:swagger}")
    private String swaggerTitle;
    @Value("${swagger.version:1.0.0}")
    private String swaggerVersion;

    /**
     * 	header
     */
    private static final String HEADER = "header";

    /**
     * 	全局header参数token
     */
    private static final String TOKEN = "token";

    /**
     * 全局header参数Authorization
     */
    private static final String AUTHORIZATION = "Authorization";

    @Bean
    public Docket createRestApi() {
        return new Docket(DocumentationType.SWAGGER_2)
                .apiInfo(apiInfo())
                // 启用
                .enable(true)
                .select()
                .apis(RequestHandlerSelectors.withClassAnnotation(Api.class))
                .paths(PathSelectors.any())
                .build()
                .securitySchemes(securitySchemes())
                .securityContexts(securityContexts());
    }

    private ApiInfo apiInfo() {
        return new ApiInfoBuilder()
                .title(swaggerTitle)
                .version(swaggerVersion)
                .build();
    }

    private List<ApiKey> securitySchemes() {

        List<ApiKey> list = new ArrayList<>();
        list.add(new ApiKey(TOKEN, TOKEN, HEADER));
        list.add(new ApiKey(AUTHORIZATION, AUTHORIZATION, HEADER));
        return list;
    }

    private List<SecurityContext> securityContexts() {

        List<SecurityContext> list = new ArrayList<>();
        list.add(SecurityContext.builder()
                .securityReferences(defaultAuth())
                .forPaths(PathSelectors.any())
                .build());
        return list;
    }

    private List<SecurityReference> defaultAuth() {

        List<SecurityReference> list = new ArrayList<>();
        // 暂时不知道什么用
        // AuthorizationScope authorizationScope = new AuthorizationScope("global", "accessEverything");
        AuthorizationScope authorizationScope = new AuthorizationScope("", "");
        AuthorizationScope[] authorizationScopes = new AuthorizationScope[1];
        authorizationScopes[0] = authorizationScope;
        list.add(new SecurityReference(TOKEN, authorizationScopes));
        list.add(new SecurityReference(AUTHORIZATION, authorizationScopes));

        return list;
    }

}
```

