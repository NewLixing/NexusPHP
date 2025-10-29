# NexusPHP 产品需求文档使用指南
# NexusPHP Product Requirements Document Guide

[English](#english) | [中文](#中文)

---

## 中文

### 📋 文档概述

本仓库包含通过逆向工程 NexusPHP 项目生成的详细产品需求文档（PRD），旨在为未来的重构和现代化工作提供全面的参考。

### 📚 文档列表

1. **`PRD.md`** - 完整英文版产品需求文档
   - 1,842 行详细文档
   - 17 个主要章节
   - 57 个子章节
   - 涵盖所有系统功能和技术细节

2. **`PRD_中文摘要.md`** - 中文摘要版本
   - 精简的中文版本
   - 涵盖核心功能和架构
   - 适合快速了解系统

3. **`README_PRD.md`** - 本文件
   - 文档使用指南
   - 中英双语说明

### 🎯 文档用途

这些文档可用于：

1. **系统理解**
   - 快速了解 NexusPHP 的整体架构
   - 理解各个模块的功能和交互
   - 学习私有 Tracker 系统的设计思路

2. **重构规划**
   - 识别技术债务和改进机会
   - 制定现代化升级路线图
   - 评估迁移到新技术栈的可行性

3. **新功能开发**
   - 了解现有功能边界
   - 评估新功能的集成点
   - 保持系统一致性

4. **培训与文档**
   - 新开发者快速上手
   - 系统维护人员参考
   - 用户手册编写基础

5. **系统重写**
   - 作为需求规格说明
   - 功能完整性检查清单
   - 测试用例设计参考

### 📖 如何使用

#### 快速开始

1. **想快速了解系统？**
   - 阅读 `PRD_中文摘要.md` 第 1-8 节
   - 重点关注：项目概述、技术架构、用户系统

2. **想深入了解某个功能？**
   - 查看 `PRD.md` 第 3 节（功能需求）
   - 根据功能类别查找相关子章节

3. **想了解数据结构？**
   - 查看 `PRD.md` 第 4 节（数据模型）
   - 包含 75 个数据表的详细说明

4. **想进行系统重构？**
   - 阅读 `PRD.md` 第 12 节（已知限制与技术债）
   - 参考第 13 节（未来增强方向）

#### 详细导航

**PRD.md 完整目录：**

1. **Executive Summary** - 执行摘要
   - 产品概述
   - 目标受众
   - 核心价值

2. **System Architecture** - 系统架构
   - 技术栈
   - 核心模块

3. **Functional Requirements** - 功能需求
   - 用户管理（3.1）
   - 种子管理（3.2）
   - Tracker 引擎（3.3）
   - 分享率与魔力值系统（3.4）
   - 社区功能（3.5）
   - 管理功能（3.6）
   - 附加功能（3.7）

4. **Data Model** - 数据模型
   - 核心实体
   - 数据库表列表（75 个表）
   - 关键关系
   - 重要字段

5. **Non-Functional Requirements** - 非功能需求
   - 性能
   - 安全性
   - 可靠性
   - 可用性
   - 可维护性
   - 兼容性

6. **User Workflows** - 用户工作流
   - 主要用户流程
   - 管理流程

7. **Business Rules** - 业务规则
   - 分享率与经济规则
   - 内容管理规则
   - 用户管理规则
   - 社区规则
   - 安全规则

8. **Interface Requirements** - 界面需求
   - 用户界面页面
   - UI 组件
   - 视觉设计要求

9. **External Interfaces** - 外部接口
   - BitTorrent 协议接口
   - 邮件接口
   - 外部服务集成
   - 文件系统接口
   - 缓存接口

10. **Deployment & Infrastructure** - 部署与基础设施
    - 服务器要求
    - 安装流程
    - 生产环境考虑

11. **Migration & Upgrade Paths** - 迁移与升级路径
    - 数据迁移
    - 版本升级流程

12. **Known Limitations & Technical Debt** - 已知限制与技术债
    - 当前限制
    - 重构机会（按优先级）

13. **Future Enhancements** - 未来增强
    - 功能增强
    - 技术增强

14. **Compliance & Legal** - 合规与法律
    - 许可证
    - 隐私考虑
    - 服务条款

15. **Success Metrics & KPIs** - 成功指标与 KPI
    - 用户参与度指标
    - 内容指标
    - 社区健康指标
    - 技术指标
    - 经济指标

16. **Glossary** - 术语表

17. **Appendices** - 附录
    - 配置参数
    - 数据库表参考
    - 文件结构
    - 用户等级晋升

### 🔧 重构建议优先级

根据文档分析，建议按以下优先级进行重构：

#### 高优先级 🔴
1. **安全性改进**
   - 升级密码哈希（MD5 → bcrypt/argon2）
   - 实现全站 CSRF 保护
   - 使用预处理语句防止 SQL 注入
   - 添加输入验证框架

2. **架构现代化**
   - 实现 MVC 模式
   - 引入依赖注入
   - 创建服务层
   - 分离业务逻辑和展示层

3. **数据库优化**
   - MyISAM → InnoDB（支持事务）
   - 引入 ORM（Doctrine/Eloquent）
   - 实现数据库迁移机制

#### 中优先级 🟡
4. **代码组织**
   - 创建合适的类结构
   - 实现 PSR-4 自动加载
   - 使用命名空间
   - 减少全局变量

5. **前端现代化**
   - 响应式设计
   - 现代 JS 框架（Vue/React）
   - 替换 Flash 为 HTML5
   - 改善无障碍性

6. **API 开发**
   - RESTful API
   - JSON 响应
   - OAuth2 认证
   - API 文档

#### 低优先级 🟢
7. **测试基础设施**
   - 单元测试
   - 集成测试
   - CI/CD

8. **开发者体验**
   - Composer 包管理
   - Docker 开发环境
   - PSR-12 代码标准

### 💡 使用场景示例

#### 场景 1: 评估迁移到 Laravel 框架

1. 阅读第 2 节了解当前技术栈
2. 查看第 4 节理解数据模型
3. 参考第 12 节了解当前限制
4. 使用第 3 节作为功能清单
5. 规划渐进式迁移路径

#### 场景 2: 添加新的用户权限功能

1. 查看 3.1.2 节了解当前权限系统
2. 检查 4.2 节理解用户表结构
3. 参考 7.3 节了解用户管理业务规则
4. 评估对现有 18 级用户系统的影响

#### 场景 3: 优化网站性能

1. 查看第 5.1 节了解性能要求
2. 检查第 15.4 节查看技术指标
3. 参考第 10.3 节了解性能优化建议
4. 评估缓存策略改进

### 📊 系统关键数据

- **数据库表**: 75 个
- **用户等级**: 18 级
- **PHP 文件**: 447 个
- **种子促销状态**: 7 种
- **支持语言**: 3 种（英语、简体中文、繁体中文）
- **文档行数**: 1,842 行（英文完整版）

### 🤝 贡献

如发现文档中的错误或遗漏，欢迎提交 Issue 或 Pull Request。

### 📄 许可证

本文档遵循与 NexusPHP 项目相同的开源许可证。

---

## English

### 📋 Document Overview

This repository contains a comprehensive Product Requirements Document (PRD) generated through reverse engineering of the NexusPHP project, designed to provide a complete reference for future refactoring and modernization efforts.

### 📚 Document List

1. **`PRD.md`** - Complete English Product Requirements Document
   - 1,842 lines of detailed documentation
   - 17 major sections
   - 57 subsections
   - Covers all system features and technical details

2. **`PRD_中文摘要.md`** - Chinese Summary Version
   - Condensed Chinese version
   - Covers core features and architecture
   - Suitable for quick system overview

3. **`README_PRD.md`** - This File
   - Documentation guide
   - Bilingual instructions

### 🎯 Document Purpose

These documents can be used for:

1. **System Understanding**
   - Quickly understand NexusPHP's overall architecture
   - Understand module functions and interactions
   - Learn private tracker system design principles

2. **Refactoring Planning**
   - Identify technical debt and improvement opportunities
   - Create modernization roadmap
   - Evaluate feasibility of migrating to new tech stack

3. **New Feature Development**
   - Understand existing feature boundaries
   - Evaluate integration points for new features
   - Maintain system consistency

4. **Training & Documentation**
   - Onboard new developers quickly
   - Reference for system maintainers
   - Foundation for user manual writing

5. **System Rewrite**
   - Use as requirements specification
   - Feature completeness checklist
   - Test case design reference

### 📖 How to Use

#### Quick Start

1. **Want a quick system overview?**
   - Read `PRD_中文摘要.md` sections 1-8 (if you read Chinese)
   - Or read `PRD.md` sections 1-2
   - Focus on: Overview, Architecture, User System

2. **Want to understand a specific feature?**
   - Check `PRD.md` Section 3 (Functional Requirements)
   - Find relevant subsections by feature category

3. **Want to understand data structures?**
   - See `PRD.md` Section 4 (Data Model)
   - Contains detailed descriptions of 75 database tables

4. **Want to refactor the system?**
   - Read `PRD.md` Section 12 (Known Limitations & Technical Debt)
   - Reference Section 13 (Future Enhancements)

#### Detailed Navigation

**PRD.md Complete Table of Contents:**

1. **Executive Summary**
   - Product overview
   - Target audience
   - Key value propositions

2. **System Architecture**
   - Technology stack
   - Core modules

3. **Functional Requirements**
   - User Management (3.1)
   - Torrent Management (3.2)
   - Tracker Engine (3.3)
   - Ratio & Bonus System (3.4)
   - Community Features (3.5)
   - Administrative Features (3.6)
   - Additional Features (3.7)

4. **Data Model**
   - Core entities
   - Database table list (75 tables)
   - Key relationships
   - Critical fields

5. **Non-Functional Requirements**
   - Performance
   - Security
   - Reliability
   - Usability
   - Maintainability
   - Compatibility

6. **User Workflows**
   - Primary user workflows
   - Administrative workflows

7. **Business Rules**
   - Ratio & economy rules
   - Content management rules
   - User management rules
   - Community rules
   - Security rules

8. **Interface Requirements**
   - User interface pages
   - UI components
   - Visual design requirements

9. **External Interfaces**
   - BitTorrent protocol interface
   - Email interface
   - External service integrations
   - File system interface
   - Cache interface

10. **Deployment & Infrastructure**
    - Server requirements
    - Installation process
    - Production considerations

11. **Migration & Upgrade Paths**
    - Data migration
    - Version upgrade process

12. **Known Limitations & Technical Debt**
    - Current limitations
    - Refactoring opportunities (prioritized)

13. **Future Enhancements**
    - Feature enhancements
    - Technical enhancements

14. **Compliance & Legal**
    - Licensing
    - Privacy considerations
    - Terms of service requirements

15. **Success Metrics & KPIs**
    - User engagement metrics
    - Content metrics
    - Community health metrics
    - Technical metrics
    - Economic metrics

16. **Glossary**

17. **Appendices**
    - Configuration parameters
    - Database table reference
    - File structure
    - User class progression

### 🔧 Refactoring Priority Recommendations

Based on document analysis, refactoring is recommended in the following priority order:

#### High Priority 🔴
1. **Security Improvements**
   - Upgrade password hashing (MD5 → bcrypt/argon2)
   - Implement site-wide CSRF protection
   - Use prepared statements to prevent SQL injection
   - Add input validation framework

2. **Architecture Modernization**
   - Implement MVC pattern
   - Introduce dependency injection
   - Create service layer
   - Separate business logic from presentation

3. **Database Optimization**
   - MyISAM → InnoDB (transaction support)
   - Introduce ORM (Doctrine/Eloquent)
   - Implement database migrations

#### Medium Priority 🟡
4. **Code Organization**
   - Create proper class structure
   - Implement PSR-4 autoloading
   - Use namespaces
   - Reduce global variables

5. **Frontend Modernization**
   - Responsive design
   - Modern JS framework (Vue/React)
   - Replace Flash with HTML5
   - Improve accessibility

6. **API Development**
   - RESTful API
   - JSON responses
   - OAuth2 authentication
   - API documentation

#### Low Priority 🟢
7. **Testing Infrastructure**
   - Unit tests
   - Integration tests
   - CI/CD

8. **Developer Experience**
   - Composer package management
   - Docker development environment
   - PSR-12 code standards

### 💡 Usage Examples

#### Scenario 1: Evaluating Migration to Laravel Framework

1. Read Section 2 to understand current tech stack
2. Review Section 4 to understand data model
3. Reference Section 12 to understand current limitations
4. Use Section 3 as feature checklist
5. Plan progressive migration path

#### Scenario 2: Adding New User Permission Feature

1. Check Section 3.1.2 to understand current permission system
2. Examine Section 4.2 to understand user table structure
3. Reference Section 7.3 for user management business rules
4. Assess impact on existing 18-tier user system

#### Scenario 3: Optimizing Website Performance

1. Review Section 5.1 for performance requirements
2. Check Section 15.4 for technical metrics
3. Reference Section 10.3 for performance optimization suggestions
4. Evaluate cache strategy improvements

### 📊 Key System Statistics

- **Database Tables**: 75
- **User Classes**: 18
- **PHP Files**: 447
- **Torrent Promotion States**: 7
- **Supported Languages**: 3 (English, Chinese Simplified, Chinese Traditional)
- **Documentation Lines**: 1,842 (English complete version)

### 🤝 Contributing

If you find errors or omissions in the documentation, please submit an Issue or Pull Request.

### 📄 License

This documentation follows the same open-source license as the NexusPHP project.

---

**Last Updated**: 2025-10-29
**Document Version**: 1.0
