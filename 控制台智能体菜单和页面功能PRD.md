# 控制台智能体菜单和页面功能PRD

## 1. 文档概述

### 1.1 文档目的
本文档旨在定义控制台智能体菜单和页面的功能需求、界面设计和交互逻辑，为开发团队提供清晰的开发指南。

### 1.2 适用范围
适用于控制台智能体模块的所有相关页面和功能开发。

### 1.3 术语定义
- **智能体**：具备特定能力和个性的AI实体，可与用户进行对话并执行任务
- **智能体广场**：展示和分享智能体的公共平台
- **DSL**：领域特定语言，用于智能体的配置和导出
- **MCP**：模型调用协议，用于智能体与外部服务的交互

## 2. 功能需求

### 2.1 核心功能
1. 智能体创建与编辑
2. 智能体列表管理
3. 智能体配置管理
4. 智能体发布与版本控制
5. 智能体对话记录与分析
6. 智能体集成与扩展

### 2.2 辅助功能
1. 智能体模板市场
2. DSL导入/导出
3. 权限管理
4. 日志与审计
5. 多语言支持

## 3. 菜单结构

### 3.1 菜单层级
```
控制台
├── 智能体管理
│   ├── 智能体列表
│   ├── 智能体广场
│   ├── 对话记录
│   └── 智能体配置
└── 系统设置
    └── MCP服务设置
```

### 3.2 菜单详情

| 菜单名称 | 路径 | 权限 | 功能描述 |
|---------|------|------|----------|
| 智能体列表 | `/console/ai/agent` | AI_AGENT_LIST | 展示所有智能体，支持搜索、过滤和批量操作 |
| 智能体广场 | `/console/ai/agent/square` | AI_AGENT_SQUARE | 展示公共智能体，支持浏览和导入 |
| 对话记录 | `/console/ai/agent/chat-logs` | AI_AGENT_CHAT_LOGS | 查看所有智能体的对话历史 |
| 智能体配置 | `/console/ai/agent/config` | AI_AGENT_CONFIG | 管理智能体的全局配置和模板 |
| MCP服务设置 | `/console/settings/mcp` | MCP_SERVER_SETTINGS | 配置和管理MCP服务器 |

## 4. 页面功能

### 4.1 智能体列表页面

#### 4.1.1 页面结构
```
智能体列表页面
├── 顶部工具栏
│   ├── 搜索框
│   ├── 创建智能体按钮
│   ├── 导入DSL按钮
│   └── 批量操作按钮
├── 智能体卡片列表
│   ├── 智能体名称
│   ├── 智能体类型
│   ├── 创建时间
│   ├── 状态
│   └── 操作按钮（编辑、删除、导出DSL）
└── 分页组件
```

#### 4.1.2 核心功能
- 搜索：支持按名称、类型、标签搜索智能体
- 创建：支持从零创建智能体或从模板创建
- 导入：支持导入YAML格式的DSL配置文件
- 导出：支持导出智能体配置为YAML格式
- 编辑：支持修改智能体的基本信息和配置
- 删除：支持删除智能体（需二次确认）
- 批量操作：支持批量删除智能体

### 4.2 智能体创建/编辑页面

#### 4.2.1 页面结构
```
智能体创建/编辑页面
├── 基本信息表单
│   ├── 智能体名称
│   ├── 智能体描述
│   ├── 智能体类型
│   ├── 智能体头像
│   └── 状态
├── 配置表单
│   ├── 角色设定
│   ├── 模型配置
│   ├── 知识库关联
│   ├── 快捷指令
│   └── 高级配置
└── 操作按钮
    ├── 保存草稿
    ├── 发布智能体
    └── 取消
```

#### 4.2.2 核心功能
- 基本信息：设置智能体的基本属性
- 角色设定：定义智能体的身份、性格和行为准则
- 模型配置：选择AI模型和设置参数
- 知识库关联：关联智能体需要用到的知识库
- 快捷指令：配置常用的快捷操作
- 高级配置：设置上下文长度、温度、TOP_P等参数

### 4.3 智能体配置页面

#### 4.3.1 页面结构
```
智能体配置页面
├── 配置列表
│   ├── 配置名称
│   ├── 配置类型
│   ├── 创建时间
│   └── 操作按钮（编辑、删除）
└── 配置表单
    ├── 配置名称
    ├── 配置类型
    ├── 配置内容
    └── 保存按钮
```

#### 4.3.2 核心功能
- 配置管理：管理智能体的全局配置和模板
- 配置类型：支持系统配置和自定义配置
- 配置内容：支持JSON格式的配置内容

### 4.4 智能体发布页面

#### 4.4.1 页面结构
```
智能体发布页面
├── 发布信息表单
│   ├── 版本号
│   ├── 发布说明
│   └── 发布范围
└── 操作按钮
    ├── 发布智能体
    └── 取消
```

#### 4.4.2 核心功能
- 版本控制：支持版本号管理和发布历史
- 发布范围：支持公开、私有、指定用户等发布范围
- 发布说明：支持添加版本更新说明

## 5. 数据结构

### 5.1 智能体实体
```typescript
interface Agent {
    id: string;
    name: string;
    description?: string;
    createMode: string;
    avatar?: string;
    chatAvatar?: string;
    rolePrompt?: string;
    showContext: boolean;
    showReference: boolean;
    enableFeedback: boolean;
    enableWebSearch: boolean;
    userCount: number;
    modelConfig?: ModelConfig;
    billingConfig?: ModelBillingConfig;
    datasetIds?: string[];
    openingStatement?: string;
    openingQuestions?: string[];
    quickCommands?: QuickCommandConfig[];
    autoQuestions?: AutoQuestionsConfig;
    formFields?: FormFieldConfig[];
    formFieldsInputs?: Record<string, any>;
    mcpServerIds?: string[];
    isPublished: boolean;
    isPublic: boolean;
    publishToken?: string;
    apiKey?: string;
    createBy: string;
    publishConfig?: {
        allowOrigins?: string[];
        rateLimitPerMinute?: number;
        showBranding?: boolean;
        allowDownloadHistory?: boolean;
    };
    thirdPartyIntegration?: ThirdPartyIntegrationConfig;
    tags?: Tag[];
}
```

### 5.2 智能体配置
```typescript
interface AgentConfig {
    id: string;
    name: string;
    type: string;
    content: any;
    createdAt: Date;
    updatedAt: Date;
}
```

## 6. 接口需求

### 6.1 智能体管理接口

| 接口名称 | 方法 | 路径 | 功能描述 |
|---------|------|------|----------|
| 智能体列表 | GET | /api/v1/ai/agent | 获取智能体列表 |
| 创建智能体 | POST | /api/v1/ai/agent | 创建新智能体 |
| 智能体详情 | GET | /api/v1/ai/agent/{id} | 获取智能体详情 |
| 更新智能体 | PUT | /api/v1/ai/agent/{id} | 更新智能体信息 |
| 删除智能体 | DELETE | /api/v1/ai/agent/{id} | 删除智能体 |
| 批量删除智能体 | POST | /api/v1/ai/agent/batch-delete | 批量删除智能体 |

### 6.2 智能体配置接口

| 接口名称 | 方法 | 路径 | 功能描述 |
|---------|------|------|----------|
| 配置列表 | GET | /api/v1/ai/agent/config | 获取配置列表 |
| 创建配置 | POST | /api/v1/ai/agent/config | 创建新配置 |
| 配置详情 | GET | /api/v1/ai/agent/config/{id} | 获取配置详情 |
| 更新配置 | PUT | /api/v1/ai/agent/config/{id} | 更新配置信息 |
| 删除配置 | DELETE | /api/v1/ai/agent/config/{id} | 删除配置 |

### 6.3 智能体发布接口

| 接口名称 | 方法 | 路径 | 功能描述 |
|---------|------|------|----------|
| 发布智能体 | POST | /api/v1/ai/agent/{id}/publish | 发布智能体 |
| 撤销发布 | POST | /api/v1/ai/agent/{id}/unpublish | 撤销智能体发布 |
| 发布历史 | GET | /api/v1/ai/agent/{id}/publish-history | 获取发布历史 |

## 7. 权限控制

### 7.1 角色与权限

| 角色 | 权限 |
|------|------|
| 管理员 | 所有智能体相关权限 |
| 开发者 | 创建、编辑、发布智能体 |
| 普通用户 | 使用智能体、查看对话记录 |

### 7.2 权限颗粒度
- 智能体级别的权限控制
- 操作级别的权限控制
- 资源级别的权限控制

## 8. 非功能需求

### 8.1 性能需求
- 页面加载时间 ≤ 2秒
- 并发用户数 ≥ 1000
- 数据响应时间 ≤ 500毫秒

### 8.2 安全性需求
- 所有接口必须支持认证和授权
- 敏感数据必须加密传输
- 操作必须记录审计日志

### 8.3 可用性需求
- 系统可用性 ≥ 99.9%
- 故障恢复时间 ≤ 30分钟

### 8.4 兼容性需求
- 支持主流浏览器（Chrome、Firefox、Safari、Edge）
- 支持响应式设计

## 9. 依赖关系

### 9.1 内部依赖
- 用户模块
- 权限模块
- 模型模块
- 知识库模块

### 9.2 外部依赖
- AI模型服务
- 知识库服务
- MCP服务

## 10. 验收标准

### 10.1 功能验收
- 所有功能必须按照需求实现
- 所有页面必须正常显示和交互
- 所有接口必须返回正确的结果

### 10.2 性能验收
- 页面加载时间必须 ≤ 2秒
- 数据响应时间必须 ≤ 500毫秒
- 并发用户数 ≥ 1000

### 10.3 安全验收
- 所有接口必须支持认证和授权
- 敏感数据必须加密传输
- 操作必须记录审计日志

### 10.4 兼容性验收
- 支持主流浏览器（Chrome、Firefox、Safari、Edge）
- 支持响应式设计
- 所有接口必须支持认证和授权
- 敏感数据必须加密传输

### 10.4 兼容性验收
- 支持主流浏览器
- 支持响应式设计

## 11. 附录

### 11.1 界面设计图
[此处放置界面设计图的链接]

### 11.2 原型图
[此处放置原型图的链接]

### 11.3 参考文档
- 智能体实体定义：ai-agent.entity.ts
- 智能体API：packages/api/src/modules/ai/agent
- 智能体前端页面：packages/web/buildingai-ui/app/pages/console/ai/agent

# 版本历史

| 版本 | 日期 | 作者 | 描述 |
|------|------|------|------|
| 1.0 | 2023-10-15 | AI团队 | 初始版本 |