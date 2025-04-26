# Maven 使用与基础原理简明

## 1. Maven中 groupId 有 org 和 com 区别
- `org.xxx`: 通常指非商业组织或公益组织发布的包
- `com.xxx`: 通常指商业公司发布的包

## 2. Maven使用本地包
- 正确方式：使用 `mvn install` 把 jar 包安装到本地 Maven仓库 (.m2/repository)
- 直接把文件拷贝进 Maven仓库目录，Maven无法识别，因为没有 Maven 应有的结构和 metadata（举例：pom.xml）

## 3. Maven 在无网环境下使用
- 必须确保所有依赖已经下载到本地 Maven仓库，否则网络失效后无法找到包

## 4. Maven compile 和 IDEA Run的区别
- **Maven compile**: 第一步生成 `.class`，仅编译，不执行
- **IDEA Run**: 首先 compile，然后启动 JVM，实际运行程序

## 5. Maven compile 编译的是什么
- 把 `src/main/java` 中的 `.java` 文件编译生成 `.class`
- 编译时需要找到依赖 jar 中的类，进行类型检查和方法校验

## 6. Maven 生命周期和 pom.xml 依赖关系
- pom.xml 定义依赖，Maven在 compile/package/test 等生命周期步骤中根据 pom 规则找包
- 生命周期和依赖处理很密切，没有依赖，compile 都不能通过

## 7. 依赖和程序运行关系
- jar 包是类和方法的集合，自身不会运行
- JVM 起动后，通过 **ClassLoader** 把 jar 里的 `.class` 文件读进来，加载成类对象，才能实际执行

## 8. 没有 Maven 时怎么处理依赖
- 自己下载 jar，放到 lib 目录
- 编译时手动指定 classpath
- 运行时也手动指定 classpath
- 管理版本、冲突等相当麻烦

## 9. 总结
- Maven 是 Java 项目依赖管理、编译、构建、托管生命周期的标准工具
- 依赖最终是通过 JVM 的 ClassLoader 加载进来使用，而不是编译后直接跟程序绑定
- 没有 Maven 时，依赖管理麻烦、高错、运维成本高

