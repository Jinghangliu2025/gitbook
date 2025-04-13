---
icon: bullseye-arrow
---


# 🌱 Spring & Spring Boot 核心知识点速查（问答总结）

---

## 🌟 1. 常用注解及作用

### @Controller vs @ResponseBody
- `@Controller`：用于返回视图，传统 MVC。
- `@Controller + @ResponseBody`：返回 JSON 或 XML，适用于前后端分离。

### @Component vs @Bean
| 注解       | 用途                     | 生命周期由 Spring 管理 |
|------------|--------------------------|------------------------|
| `@Component` | 自动扫描注入容器           | ✅                     |
| `@Bean`      | 方法上声明，返回对象注入容器 | ✅（可自定义）         |

> `@Bean` 适用于需要自己手动 new 的 Bean，如第三方类。

### @Autowired vs @Resource
| 对比项           | @Autowired                | @Resource               |
|------------------|---------------------------|-------------------------|
| 来源             | Spring                    | JSR-250（Java标准）     |
| 默认注入方式     | 按类型（可加 @Qualifier） | 按名称                  |
| 支持构造器注入   | ✅ 支持                   | ❌ 不支持               |
| 支持字段/Setter注入 | ✅                        | ✅                      |

---

## ⚙️ 2. 项目启动时执行逻辑

### CommandLineRunner vs ApplicationRunner
| 接口               | 参数类型               | 说明 |
|--------------------|------------------------|------|
| `CommandLineRunner` | `String... args`        | 适合简单参数处理 |
| `ApplicationRunner` | `ApplicationArguments` | 支持 `--key=value` 结构，更强大 |

### 启动时初始化数据（JPA 示例）
```java
@Component
public class DataInitializer implements CommandLineRunner {
    @Autowired private UserRepository userRepository;

    @Override
    public void run(String... args) {
        if (userRepository.count() == 0) {
            userRepository.save(new User("admin"));
        }
    }
}
```

---

## 🧱 3. Spring Boot 数据库与 ORM

### 数据库表自动创建
Spring Boot + JPA 可通过配置 `spring.jpa.hibernate.ddl-auto` 实现自动建表：
- `create`：每次启动都创建新表
- `update`：表不存在时创建，已存在则更新字段

### 插入默认数据
结合 `CommandLineRunner`/`ApplicationRunner` + 判断 `Repository.count()` 插入。

---

## 🔄 4. 异常处理与事务

### 事务回滚行为（@Transactional）
- 默认只在 `RuntimeException` 时回滚。
- 若希望非运行时异常（checked）也回滚，需配置：
  ```java
  @Transactional(rollbackFor = Exception.class)
  ```

### 自定义异常推荐做法
```java
public class BusinessException extends RuntimeException {
    public BusinessException(String msg) {
        super(msg);
    }
}
```
优势：
- 可自动回滚事务
- 无需频繁 try-catch
- 保持代码整洁

---

## 🔍 5. 异常分类

| 类型             | 示例                      | 是否必须捕获 | 是否自动回滚（@Transactional） |
|------------------|---------------------------|----------------|------------------|
| **运行时异常**     | NullPointerException 等     | 否（unchecked）| ✅ 默认会回滚       |
| **非运行时异常**   | IOException, SQLException | 是（checked） | ❌ 不回滚，需指定   |

---

## 🧰 6. MyBatis 占位符：#{} vs ${}

| 表达式 | 类型     | 用途说明 |
|--------|----------|----------|
| `${}`  | 文本替换 | 会原样替换，适合用于表名、字段名等 |
| `#{}`  | 参数占位 | 生成 `?` 占位符，用于安全绑定参数 |

---

## 🌐 7. 框架理解

### Spring vs Spring Boot
| 项目        | 特点                                  |
|-------------|---------------------------------------|
| Spring      | 需要手动配置，结构灵活               |
| Spring Boot | 自动配置 + Starter，开发效率高        |

### Spring MVC 是什么？
- Spring Web 模块中的一部分
- 核心是 DispatcherServlet
- 支持控制器、视图解析、数据绑定等

---

## 📦 8. 构造器注入演示小结

- 推荐构造器注入方式，配合 `@Autowired` 和 `@Qualifier`
- `@Resource` 不支持构造注入，仅支持字段/Setter

```java
@Autowired
public UserService(@Qualifier("mysqlRepository") UserRepository userRepository) {
    this.userRepository = userRepository;
}
```

---

## 🌱 补充注解

### `@JsonUnwrapped`
- 用于 Jackson 序列化时展开嵌套对象字段

```java
public class Wrapper {
    @JsonUnwrapped
    private Inner inner;
}
```

会变成扁平 JSON 输出。

---

🧾 **来源备注：**
参考资料来自 [JavaGuide](https://javaguide.cn) 和你的问题内容。


