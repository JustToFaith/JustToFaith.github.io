# Springboot 自动装配原理

**pom.xml**

- spring-boot-dependencies：核心依赖在父工程中
- 我们在写或者引入一些 Springboot 依赖的时候，不需要指定版本，因为父工程中有版本仓库。



**启动器**

- ```xml
  <dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter</artifactId>
  </dependency>
  ```

- 启动器：就是 Springboot 的启动场景

- 比如 spring-boot-starter-web，他就会自动导入 web环境所有的依赖

- springboot 会将所有的功能场景，都变成一个个启动器。

- 我们需要使用某个功能，只需要找到相关启动器`starter`。



**主程序**

```java
// @SpringBootApplication：标注这个类是一个 springboot 应用
@SpringBootApplication
public class LearnSpringbootApplication {

    public static void main(String[] args) {
        SpringApplication.run(LearnSpringbootApplication.class, args);
    }

}

```

- 注解

  - ```java
    @SpringBootConfiguration：springboot 的配置
      	@Configuration：spring 配置类
      			@Component：说明这是一个spring组件
    @EnableAutoConfiguration
      	@AutoConfigurationPackage：自动配置包
    				@Import(AutoConfigurationPackages.Registrar.class)：自动配置 `包注册`
      	@Import(AutoConfigurationImportSelector.class)：自动配置导入选择
      
    // 获取所有的配置
    List<String> configurations = getCandidateConfigurations(annotationMetadata, attributes);
    
    ```

  结论：springboot 所有自动配置都是在启动的时候扫描并加载：`spring.factories`所有的自动配置类都在这里面，但是不一定生效，要判断条件是否成立，只要导入了对应的`start`，就有对应的启动器，有了启动器，自动装配就会生效，然后就会配置成功。

  1. springboot 在启动的时候，从类路径下 `/META-INF/spring.factories`获取指定的值；
  2. 将这些自动配置的类导入容器，自动配置就会生效，帮我们自动配置；
  3. 以前我们需要自动配置的东西，现在 springboot 帮我们做了；
  4. 整个 javaEE，解决方案和自动配置的东西都在`spring-boot-autoconfigure-2.xxx.RELEASE.jar`包下；
  5. 它会将所有需要导入的组件，以类名的方式返回，这些组件就会被添加到容器。
  6. 容器中存在的`xxxAutoConfiguration`文件（@Bean），这些类给容器中导入了每个场景需要的所有组件，@Configuration，JavaConfig
  7. 有了自动配置类，免去手动配置的的过程。





[狂神说链接](https://mp.weixin.qq.com/s/hzRwZvjYSX-dy-9Drz94aQ)

