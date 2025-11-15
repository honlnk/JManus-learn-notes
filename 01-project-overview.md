# 01 - JManus 项目概览

## 📋 基本信息

- **项目名称**: JManus - AI Agent Management System
- **版本**: 4.2.0
- **技术栈**: Spring Boot 3.5.6 + Spring AI 1.0.1 + Java 17
- **开发者**: 阿里巴巴团队
- **许可证**: Apache License 2.0

## 🎯 项目定位

JManus 是一个基于 Spring Boot 的 AI 智能体管理系统，提供：

- **多智能体协作**：纯 Java 实现的智能体系统
- **工具集成**：浏览器自动化、数据库操作、文件处理等
- **Spring AI Alibaba**：集成阿里巴巴的 AI 扩展
- **HTTP 接口**：可集成到现有 Java 项目
- **响应式编程**：使用 WebFlux 支持异步处理

## 🏗️ 核心架构

### 🎨 系统架构白板图

**详细的系统架构白板已创建** (建议使用 Obsidian 打开 .canvas 文件)：

1. **[系统整体架构](diagrams/system-architecture.canvas)** - 完整的系统模块关系和基础设施配置
   - 包含 19 个核心模块的详细分析
   - 开发系统级别的配置说明
   - 性能优化特性和设计模式

2. **[智能体系统详解](diagrams/agent-system.canvas)** - BaseAgent、ReActAgent、DynamicAgent 的详细分析
   - 继承关系和核心方法
   - 状态管理和生命周期
   - 错误处理机制和内存管理

3. **[工具系统架构](diagrams/tool-system.canvas)** - 工具接口、内置工具、执行机制
   - 8 大工具类别详解
   - 特殊工具类型和生成式工具
   - 工具生命周期管理

4. **[数据流程分析](diagrams/data-flow.canvas)** - 从用户请求到执行完成的完整数据流
   - 7 个主要执行阶段
   - 数据结构和处理机制
   - 事件驱动和一致性保证

**建议学习顺序**：
1. 先查看 `system-architecture.canvas` 了解整体结构
2. 然后深入 `agent-system.canvas` 理解智能体核心
3. 结合 `data-flow.canvas` 理解执行流程
4. 最后研究 `tool-system.canvas` 了解工具生态

### 主要模块

#### 1. 智能体系统 (`agent`)
- **BaseAgent**: 具有状态管理的核心智能体功能
- **ReActAgent**: 推理-行动智能体实现
- **DynamicAgent**: 具有动态行为的可配置智能体
- **AgentController**: 智能体管理的 REST API

#### 2. 规划系统 (`planning`)
- **PlanningFactory**: 规划组件的中央工厂
- **PlanTemplateService**: 基于模板的规划管理
- **PlanningCoordinator**: 协调规划执行

#### 3. 工具系统 (`tool`)
- 浏览器自动化 (`BrowserUseTool`)
- 数据库操作 (`DatabaseReadTool`, `DatabaseWriteTool`)
- 文件系统操作 (`DirectoryOperator`, `TextFileOperator`)
- 文档处理 (PDF, Excel, Markdown)
- 系统操作 (`Bash` 工具)

#### 4. 运行时执行 (`runtime`)
- **PlanExecutorFactory**: 不同的执行策略
- **DynamicToolPlanExecutor**: 基于工具的计划执行
- **LevelBasedExecutorPool**: 线程池管理

#### 5. 配置管理 (`config`)
- 基于数据库的配置系统
- 支持运行时配置更新
- 按组管理配置

## 🔧 技术特点

### 1. Spring AI Alibaba 集成
```xml
<dependency>
    <groupId>com.alibaba.cloud.ai</groupId>
    <artifactId>spring-ai-alibaba-starter</artifactId>
    <version>${spring-ai-alibaba.version}</version>
</dependency>
```

### 2. 响应式编程
- 使用 Spring WebFlux 支持高并发
- 非阻塞 I/O 操作
- 响应式数据库访问

### 3. 多数据库支持
- **H2**: 默认嵌入式数据库（开发测试）
- **MySQL**: 生产环境推荐
- **PostgreSQL**: 生产环境选择

### 4. 浏览器自动化
```xml
<dependency>
    <groupId>com.microsoft.playwright</groupId>
    <artifactId>playwright</artifactId>
    <version>1.55.0</version>
</dependency>
```

### 5. MCP (Model Context Protocol) 支持
- 外部服务集成
- 标准化的工具接口
- 插件化架构

## 🚀 快速开始

### 1. 环境要求
- Java 17+
- Maven 3.6+
- IDE (推荐 IntelliJ IDEA)

### 2. 构建运行
```bash
# 构建项目
mvn clean package -DskipTests=true

# 运行应用
mvn spring-boot:run

# 或使用 JAR 运行
java -jar target/jmanus.jar
```

### 3. 访问应用
- 应用地址: http://localhost:18080
- API 文档: http://localhost:18080/swagger-ui.html（如果有）

### 4. 数据库配置
在 `application.yml` 中配置：
```yaml
spring:
  profiles:
    active: h2  # 或 mysql, postgres
```

## 📊 核心特性

### 1. 智能体特性
- **状态管理**: 完整的智能体生命周期管理
- **步骤限制**: 防止无限执行
- **卡住检测**: 自动检测和处理卡住状态
- **内存管理**: 对话历史和上下文管理

### 2. 规划特性
- **模板化**: 可重用的计划模板
- **动态执行**: 运行时规划调整
- **协调机制**: 多智能体协作

### 3. 工具特性
- **丰富工具集**: 涵盖常见操作场景
- **易于扩展**: 标准化的工具接口
- **并行执行**: 支持多工具并行调用

### 4. 企业特性
- **配置管理**: 数据库驱动的配置系统
- **多租户**: namespace 支持
- **监控**: 详细的执行记录
- **安全**: 完整的权限控制

## 🎯 应用场景

1. **自动化工作流**: 数据处理、报告生成
2. **智能客服**: 对话式 AI 应用
3. **代码助手**: 代码生成、优化建议
4. **数据分析**: 智能数据探索和可视化
5. **文档处理**: 自动化文档生成和分析

## 📈 性能特点

- **高并发**: 响应式架构支持高并发请求
- **资源优化**: 连接池、线程池优化
- **内存管理**: 智能的内存使用和清理
- **容错设计**: 完善的异常处理机制

---

*创建日期：2025-11-14*
*最后更新：2025-11-14*