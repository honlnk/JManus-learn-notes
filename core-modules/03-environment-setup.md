# 03 - 开发环境搭建指南

## 🎯 学习目标

- 理解JManus项目的运行环境要求
- 掌握Spring Boot项目的环境配置方法
- 学会AI模型API的配置和使用
- 了解企业级Java应用的部署和调试

## 📋 环境要求清单

### ✅ 已检查通过的环境
- [x] **Java版本**: OpenJDK 21.0.9 LTS (要求Java 17+)
- [x] **Maven版本**: 3.9.11 (用于项目构建)
- [x] **操作系统**: macOS 15.5 (支持良好)
- [x] **JAVA_HOME**: 已正确配置

### 🔍 项目配置分析
- **项目框架**: Spring Boot 3.5.6 + Spring AI 1.0.1
- **AI框架**: Spring AI Alibaba 1.0.0.4-SNAPSHOT
- **数据库**: 默认使用H2 (嵌入式)
- **端口**: 18080
- **构建工具**: Maven

## 🏗️ 环境搭建步骤

### 第1步：理解项目配置结构

#### 🔍 配置文件结构分析

**我是如何了解项目配置的？**

1. **文件系统探索**：
   ```bash
   find src/main/resources -name "*.yml" -o -name "*.properties"
   ```
   - 发现：存在多个profile配置文件

2. **主配置文件分析**：
   - 文件：`src/main/resources/application.yml`
   - 关键发现：
     ```yaml
     spring:
       profiles:
         active: h2  # 默认激活h2配置
     server:
       port: 18080   # 应用端口
     ```

3. **Profile机制理解**：
   - Spring Boot的Profile特性：`application-{profile}.yml`
   - 通过 `spring.profiles.active` 激活特定配置

#### 核心配置文件
```
src/main/resources/
├── application.yml              # 主配置文件
├── application-h2.yml          # H2数据库配置
├── application-mysql.yml       # MySQL数据库配置
├── application-postgres.yml    # PostgreSQL数据库配置
└── application-dev.yml         # 开发环境配置
```

#### 📋 关键配置解析（基于代码分析）

1. **数据库配置**：
   - 来源：`application.yml` 第7行 `active: h2`
   - 详情：查看 `application-h2.yml` 了解H2配置

2. **AI模型配置**：
   - 来源：`pom.xml` 中的 `spring-ai-alibaba` 依赖
   - 推论：需要DashScope API密钥

3. **日志配置**：
   - 来源：`application.yml` 第46-57行
   - 发现：日志输出到 `./logs/info.log`

4. **文件上传配置**：
   - 来源：`application.yml` 第77-87行
   - 发现：最大支持1GB文件

### 第2步：AI模型配置（动态配置机制）

#### 🔍 **重要发现：JManus使用动态API配置！**

**我的错误分析纠正**：
之前的分析存在重大错误！我原本以为需要预配置API密钥，但通过实际运行发现，JManus采用了更先进的**动态配置机制**。

#### 🎯 真实的API配置机制

**信息来源（基于代码分析）**：

1. **用户界面配置**：
   - 在Web界面中动态输入API密钥
   - 说明项目支持运行时配置

2. **数据库存储分析**：
   - 表：`dynamic_models`（来源：`DynamicModelEntity.java`）
   - 字段：
     ```java
     @Entity
     @Table(name = "dynamic_models")
     public class DynamicModelEntity {
         private String baseUrl;      // API基础URL
         private String apiKey;       // API密钥
         private String modelName;    // 模型名称
         private String type;         // 模型类型
         private boolean isDefault;   // 是否默认
         private Double temperature;  // 温度参数
         private Double topP;         // topP参数
     }
     ```

3. **REST API接口**：
   - 控制器：`ModelController.java`
   - 端点：`/api/models`
   - 支持完整的CRUD操作：
     ```java
     @PostMapping("/validate")  // 验证配置
     @PostMapping("/{id}/set-default")  // 设置默认模型
     @GetMapping("/available-models")  // 获取可用模型
     ```

#### 🏗️ Spring AI配置的特殊处理

**为什么需要排除ChatClientAutoConfiguration？**

1. **自定义架构需求**（来源：`../resources/spring-ai-config.md`）：
   - JManus实现多智能体协作，需要自定义LLM服务层
   - 避免与Spring AI默认配置冲突

2. **配置文件中的排除**（来源：`application.yml`第39-43行）：
   ```yaml
   spring:
     ai:
       autoconfigure:
         exclude:
           - org.springframework.ai.model.chat.client.autoconfigure.ChatClientAutoConfiguration
   ```

3. **自定义实现**：
   - `LlmService.java`：自定义LLM服务
   - `MemoryConfig.java`：自定义内存管理
   - 支持多种数据库：MySQL、PostgreSQL、H2

#### 🚀 实际的配置流程

**第1步：启动应用（无需预配置API）**
```bash
# 直接启动，JManus会使用H2数据库
mvn spring-boot:run
```

**第2步：Web界面配置**
1. 访问：http://localhost:18080
2. 在配置界面添加模型：
   - Base URL：`https://dashscope.aliyuncs.com/compatible-mode/v1`
   - API Key：您的DashScope密钥
   - Model Name：`qwen-plus`（或其他DashScope模型）
   - Type：`DASHSCOPE`

**第3步：验证配置**
- 使用 `/api/models/validate` 接口测试
- 设置为默认模型
- 开始使用智能体

#### 💡 这种设计的优势

1. **用户友好**：无需修改配置文件
2. **多模型支持**：可同时配置多个AI提供商
3. **运行时切换**：支持动态切换默认模型
4. **安全性**：API密钥存储在数据库中，支持加密

### 第3步：项目构建和启动

#### 🔍 启动方式分析

**我是如何知道这些启动方式的？**

1. **项目根目录检查**：
   - 发现 `Makefile` 文件，说明支持make命令
   - 发现 `pom.xml`，确认是Maven项目

2. **Makefile分析**：
   ```bash
   cat Makefile
   ```
   - 发现：`build`、`test`、`run` 等目标
   - 推论：支持 `make build`、`make run` 等命令

3. **主类查找**：
   ```bash
   find src -name "*Application.java"
   ```
   - 发现：`JManusApplication.java`
   - 确认：标准的Spring Boot应用主类

4. **版本信息获取**：
   - 来源：`pom.xml` 第10行 `<version>4.6.3</version>`
   - 用途：确定JAR包名称

#### 方式一：使用Maven命令（推荐）
```bash
# 1. 清理并编译项目
mvn clean compile -DskipTests

# 2. 打包项目
mvn package -DskipTests

# 3. 运行项目
mvn spring-boot:run

# 或者直接运行JAR包
java -jar target/jmanus-4.6.3.jar
```

#### 方式二：使用IDE
1. 在IntelliJ IDEA中打开项目
2. 找到 `JManusApplication.java` 主类（位置：通过搜索发现）
3. 右键 → Run 'JManusApplication'

#### 方式三：使用Makefile
```bash
# 构建项目（跳过测试）
make build

# 运行测试
make test

# 运行应用程序
make run
```

### 第4步：验证安装成功

#### 🔍 验证信息分析

**我是如何知道这些验证方法的？**

1. **端口信息获取**：
   - 来源：`application.yml` 第2行 `server.port: 18080`
   - 推论：应用运行在18080端口

2. **H2控制台配置**：
   - 来源：`application-h2.yml` 第7-12行
   - 发现：
   ```yaml
   h2:
     console:
       enabled: true
       path: /h2-console
   ```
   - 推论：可以通过 `/h2-console` 访问数据库控制台

3. **数据库连接信息**：
   - 来源：`application-h2.yml` 第3-6行
   - 发现：
   ```yaml
   url: jdbc:h2:file:./h2-data/openmanus_db
   username: sa
   password: $FSD#@!@#!#$!12341234
   ```

4. **API端点发现**：
   - 方法：搜索Controller类
   ```bash
   find src -name "*Controller.java"
   ```
   - 发现：`AgentController`、`ManusController` 等
   - 推论：存在REST API接口

5. **Actuator端点**：
   - 来源：`pom.xml` 搜索 `spring-boot-starter-actuator`
   - 推论：支持 `/actuator/health` 健康检查

#### 检查启动日志
项目启动成功后，应该看到：
```
Started JManusApplication in X.XXX seconds
JManus AI Agent Management System is running on port 18080
```

#### 访问Web界面
- **主应用**: http://localhost:18080
- **H2控制台**: http://localhost:18080/h2-console
  - JDBC URL: `jdbc:h2:file:./h2-data/openmanus_db`
  - 用户名: `sa`
  - 密码: `$FSD#@!@#!#$!12341234`

#### API接口测试
```bash
# 健康检查
curl http://localhost:18080/actuator/health

# 获取配置
curl http://localhost:18080/api/config/group/default
```

## 🔧 常见问题和解决方案

### 问题1：编译错误
**症状**: Maven编译失败
**解决方案**:
```bash
# 清理Maven缓存
mvn clean

# 强制更新依赖
mvn dependency:resolve -U

# 重新编译
mvn compile -DskipTests
```

### 问题2：端口被占用
**症状**: `Port 18080 is already in use`
**解决方案**:
```bash
# 查找占用端口的进程
lsof -i :18080

# 杀死进程
kill -9 <PID>

# 或者修改端口
# 在 application.yml 中修改 server.port
```

### 问题3：AI模型连接失败
**症状**: DashScope API调用失败
**解决方案**:
1. 检查API密钥是否正确
2. 检查网络连接
3. 确认DashScope服务可用性

### 问题4：内存不足
**症状**: `OutOfMemoryError`
**解决方案**:
```bash
# 增加JVM内存
export MAVEN_OPTS="-Xmx2g -Xms1g"

# 或在pom.xml中配置
```

## 📚 学习要点

### 1. Spring Boot配置原理
- **配置文件优先级**: application-{profile}.yml > application.yml
- **环境变量覆盖**: 系统环境变量可以覆盖配置文件中的值
- **Profile机制**: 支持多环境配置切换

### 2. Spring AI Alibaba集成
- **自动配置**: 通过Spring Boot AutoConfiguration机制
- **模型抽象**: 支持多种AI模型的统一接口
- **工具集成**: 原生支持Function Calling

### 3. 企业级应用特性
- **连接池配置**: HikariCP数据库连接池
- **日志管理**: Logback日志框架
- **文件处理**: 支持大文件上传和处理

## 🎯 下一步学习计划

### 实践任务
1. **[ ] 成功启动项目**
   - 验证所有依赖正常
   - 检查数据库连接
   - 测试API接口

2. **[ ] 配置AI模型**
   - 获取DashScope API密钥
   - 测试AI模型调用
   - 验证智能体功能

3. **[ ] 环境切换**
   - 尝试切换到MySQL数据库
   - 配置开发/生产环境
   - 测试不同Profile

### 深入学习
1. **Spring Boot高级配置**
2. **Spring AI框架原理**
3. **企业级应用部署**
4. **监控和日志管理**

## 🔗 相关资源

- [Spring Boot官方文档](https://spring.io/projects/spring-boot)
- [Spring AI Alibaba文档](https://github.com/alibaba/spring-ai-alibaba)
- [DashScope API文档](https://help.aliyun.com/zh/dashscope/)
- [H2数据库文档](https://www.h2database.com/html/main.html)

## 💡 JManus技术架构特点

**现代企业级设计模式**：
1. **动态配置机制**：运行时通过Web界面配置AI模型
2. **数据库持久化**：API密钥和模型配置存储在数据库中
3. **Spring AI自定义**：排除默认配置，实现特定需求
4. **用户友好设计**：无需修改配置文件即可使用

**架构学习价值**：
- 现代企业级应用越来越倾向于运行时配置
- Spring Boot的自动配置可以通过`exclude`灵活控制
- 数据库驱动的配置管理比文件配置更灵活

---

**学习时间估计**: 2-3小时
**难度级别**: ⭐⭐☆☆☆ (初级)
**前置知识**: Java基础、Maven使用

*最后更新：2025-11-16*
