# JManus 学习笔记

## 📚 学习路径总览

本文档包记录学习 JManus（基于 Spring AI Alibaba 的智能体管理系统）的全过程。

### 学习目标
- 深入理解 AI 智能体系统的设计和实现
- 掌握 Spring AI Alibaba 框架的使用
- 具备自定义智能体和工具开发能力
- 理解企业级 AI 应用的架构设计

### 预计学习时间：2-3周

---

## 📋 学习进度

- [x] **第一阶段：项目概览** (1天)
  - [x] 了解项目整体架构和核心概念
  - [x] 搭建开发环境并运行项目

- [x] **第二阶段：智能体系统** (2-3天)
  - [x] 理解 BaseAgent 抽象基类设计
  - [x] 学习 ReActAgent 推理-行动模式
  - [x] 分析智能体状态管理机制

- [ ] **第三阶段：规划系统** (2-3天)
  - [ ] PlanningFactory 工厂模式
  - [ ] PlanTemplateService 模板管理
  - [ ] 计划执行协调机制

- [ ] **第四阶段：工具系统** (2-3天)
  - [ ] 工具接口设计
  - [ ] 内置工具分析
  - [ ] 自定义工具开发

- [ ] **第五阶段：系统整合** (3-4天)
  - [ ] API 接口设计
  - [ ] 数据库设计
  - [ ] 事件系统
  - [ ] MCP 集成

- [ ] **第六阶段：实践项目** (3-5天)
  - [ ] 创建自定义智能体
  - [ ] 开发专用工具
  - [ ] 集成外部服务

---

## 📁 文档结构

```
docs/learning-notes/
├── README.md                   # 本文件，学习总览
├── 01-project-overview.md      # 项目概览笔记
├── 02-agent-system.md          # 智能体系统详解
├── 03-planning-system.md       # 规划系统详解
├── 04-tool-system.md           # 工具系统详解
├── 05-api-design.md            # API 接口设计
├── 06-database-design.md       # 数据库设计
├── 07-advanced-topics.md       # 高级主题
├── 08-practice-examples.md     # 实践案例
├── diagrams/                   # 架构图和流程图
│   ├── system-architecture.puml
│   ├── agent-lifecycle.puml
│   └── tool-execution.puml
├── examples/                   # 代码示例
│   ├── custom-agent/
│   ├── custom-tool/
│   └── integration-examples/
└── exercises/                  # 练习题
    ├── basic-exercises.md
    ├── advanced-exercises.md
    └── project-ideas.md
```

---

## 🎯 学习方法建议

### 1. 理论与实践结合
- 每学习一个模块，先阅读源码理解原理
- 然后运行项目，观察实际执行流程
- 最后编写简单的示例代码验证理解

### 2. 循序渐进
- 从基础概念开始，逐步深入复杂功能
- 不要急于求成，确保每个模块都理解透彻
- 遇到问题及时记录，寻找解决方案

### 3. 代码阅读技巧
- 从接口和抽象类开始，理解设计模式
- 关注核心流程，先忽略细节实现
- 使用 IDE 调试功能，跟踪代码执行

### 4. 实践建议
- 搭建本地开发环境
- 多尝试不同的配置和参数
- 记录遇到的问题和解决方案

---

## 📖 参考资料

### 官方文档
- [Spring AI 官方文档](https://spring.io/projects/spring-ai)
- [Spring Boot 文档](https://spring.io/projects/spring-boot)

### 相关技术
- [Spring AI Alibaba](https://github.com/alibaba/spring-ai-alibaba)
- [ReAct 论文](https://arxiv.org/abs/2210.03629)
- [MCP 协议](https://modelcontextprotocol.io/)

### 开发工具
- IntelliJ IDEA（推荐）
- Maven 构建工具
- Postman（API 测试）
- PlantUML（图表绘制）

---

## 📝 学习记录

### 重要发现
- *记录学习过程中的重要发现和见解*

### 问题与解决
- *记录遇到的问题和解决方案*

### 代码片段
- *记录有用的代码片段和模式*

---

*最后更新：2025-11-14*