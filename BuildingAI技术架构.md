# BuildingAI技术架构文档

## 1. 项目概述

BuildingAI是一个基于扩展的企业级AI平台，提供了AI对话、AI应用生成、模型管理、系统配置等核心功能。平台采用模块化设计，支持通过扩展系统灵活扩展功能。

## 2. 技术栈

| 模块   | 技术栈                     | 主要职责                  |
| ---- | ------------------------ | --------------------- |
| API层  | NestJS、TypeScript、PostgreSQL | 提供RESTful API接口      |
| Web层  | Nuxt.js、Vue3、TailwindCSS | 提供前端界面和用户交互        |
| Core层 | TypeScript、NestJS模块、TypeORM | 提供核心业务逻辑和服务        |
| 数据库  | PostgreSQL、TypeORM       | 数据存储和管理              |
| 扩展系统 | TypeScript、NestJS动态模块     | 支持功能扩展和插件机制           |

## 3. 整体架构

BuildingAI采用分层架构设计，主要分为API层、Web层、Core层和扩展系统。

### 3.1 分层架构图

```
┌─────────────┐
│   Web层      │
└─────────────┘
        ↓
┌─────────────┐
│   API层      │
└─────────────┘
        ↓
┌─────────────┐
│   Core层     │
└─────────────┘
        ↓
┌─────────────┐
│ 数据库层      │
└─────────────┘
        ↓
┌─────────────┐
│ 扩展系统      │
└─────────────┘
```

### 3.2 模块关系

- **API层**：提供RESTful API接口，处理前端请求和响应
- **Web层**：提供前端界面和用户交互
- **Core层**：提供核心业务逻辑和服务
- **数据库层**：数据存储和管理
- **扩展系统**：支持功能扩展和插件机制

## 4. API模块技术实现

API模块采用NestJS的模块化架构，提供了AI对话、模型管理、系统配置等核心功能的RESTful API接口。

### 4.1 入口文件配置

```typescript
import { NestFactory } from '@nestjs/core';
import { AppModule } from './modules/app.module';
import { ConfigService } from '@nestjs/config';

async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  const configService = app.get(ConfigService);
  const port = configService.get<number>('port') || 3000;
  await app.listen(port);
}
bootstrap();
```

### 4.2 动态模块配置

```typescript
@Module({
  imports: [
    ConfigModule.forRoot({ isGlobal: true }),
    RedisModule.registerAsync({
      useFactory: (configService: ConfigService) => ({
        url: configService.get<string>('redis.url'),
      }),
      inject: [ConfigService],
    }),
    // 其他核心模块导入
  ],
  controllers: [AppController],
  providers: [AppService],
})
export class AppModule {
  // 动态模块注册逻辑
}
```

### 4.3 核心模块集成

```typescript
@Module({
  imports: [
    CacheModule.register({ isGlobal: true }),
    EventEmitterModule.forRoot(),
    LoggerModule.forRoot(),
    QueueModule.forRoot(),
    SharedModule.forRoot(),
    StorageModule.forRoot(),
  ],
})
export class AppModule {}
```

### 4.4 安全防护机制

```typescript
@Module({
  providers: [
    {
      provide: APP_GUARD,
      useClass: DemoGuard,
    },
    {
      provide: APP_GUARD,
      useClass: AuthGuard,
    },
  ],
})
export class AppModule {}
```

### 4.5 AI核心模块

AI模块是API层的核心，包含以下子模块：

```
ai/
├── agent/           # AI智能体管理
├── chat/            # 对话管理
├── datasets/        # 数据集管理
├── mcp/             # MCP集成
├── model/           # 模型管理
├── provider/        # AI提供商管理
└── secret/          # 密钥管理
```

#### 4.5.1 AI Agent模块

- **功能**：创建和管理具有记忆的AI智能体
- **主要组件**：控制器、服务、DTO、验证器
- **关键特性**：
  - 支持智能体目标设置
  - 工具使用能力
  - 自主任务执行
  - 记忆管理

#### 4.5.2 AI Chat模块

- **功能**：多模态对话管理
- **主要组件**：控制器、服务、DTO
- **关键特性**：
  - 实时对话交互
  - 对话记录管理
  - 支持多种AI模型
  - 上下文保持

#### 4.5.3 AI Datasets模块

- **功能**：数据集管理和向量化
- **主要组件**：控制器、服务、处理器、向量化引擎
- **关键特性**：
  - 支持多种数据格式
  - 数据分段和标注
  - 数据集版本管理
  - 向量化处理

#### 4.5.4 AI Model模块

- **功能**：AI模型管理
- **主要特性**：
  - 模型配置管理
  - 模型版本控制
  - 模型调用接口

#### 4.5.5 AI Provider模块

- **功能**：AI服务提供商管理
- **主要特性**：
  - 多提供商支持
  - 提供商配置管理
  - 负载均衡和故障转移

## 5. Web模块技术实现

### 5.1 前端框架配置

```typescript
export default defineBuildingAIConfig({
  ssr: false,
  pages: {
    '*': {
      // 页面配置
    },
  },
  components: {
    global: true,
    dirs: [
      // 组件目录
    ],
  },
  app: {
    // 应用配置
  },
  css: [
    // CSS样式
  ],
  vite: {
    // Vite配置
  },
});
```

### 5.2 组件设计

```typescript
export default defineComponent({
  // 组件定义
  setup() {
    // 组件逻辑
  },
});
```

## 6. Core模块技术实现

### 6.1 核心服务设计

```typescript
@Injectable()
export class CoreService {
  // 核心业务逻辑
}
```

### 6.2 工具类设计

```typescript
export class CoreUtils {
  // 工具方法
}
```

### 6.3 事件系统

```typescript
export class EventBus {
  // 事件发布和订阅
}
```

## 7. 数据库架构

### 7.1 数据模型设计

BuildingAI使用TypeORM作为ORM框架，定义了BaseEntity和SoftDeleteBaseEntity两个抽象基类。

```typescript
import { PrimaryGeneratedColumn, Column, CreateDateColumn, UpdateDateColumn } from 'typeorm';

@Entity()
export abstract class BaseEntity {
  @PrimaryGeneratedColumn('uuid')
  id: string;

  @CreateDateColumn()
  createdAt: Date;

  @UpdateDateColumn()
  updatedAt: Date;
}
```

### 7.2 主要实体列表

BuildingAI包含35个数据库实体，涵盖了核心业务领域：

| 实体名称               | 主要职责            |
| ------------------ | ----------------- |
| account-log.entity | 记录账户操作日志        |
| ai-agent.entity    | 存储AI代理信息         |
| ai-app.entity      | 存储AI应用信息         |
| ai-conversation.entity | 存储AI对话历史       |
| ai-dataset.entity  | 存储数据集信息         |
| ai-message.entity  | 存储消息内容          |
| ai-model.entity    | 存储AI模型信息         |
| extension.entity   | 存储扩展信息          |
| user.entity        | 存储用户信息          |
| workflow.entity    | 存储工作流信息         |

### 7.3 实体关系说明

BuildingAI实体之间存在多种关系：

- User与AiApp、AiAgent、AiDataset等实体存在一对多关系
- AiApp与AiConversation、AiMessage等实体存在一对多关系
- Extension与ExtensionConfig存在一对多关系
- AiDataset与AiDatasetItem存在一对多关系

## 8. 扩展系统

### 8.1 扩展加载流程

1. **扩展发现**：系统在启动时扫描extensions目录
2. **配置解析**：读取extensions.json配置文件，确定哪些扩展需要加载
3. **模块加载**：使用动态导入加载扩展模块
4. **路由注册**：为扩展注册API路由
5. **扩展初始化**：调用扩展的初始化方法

### 8.2 扩展开发规范

- **语言**：TypeScript
- **入口**：扩展必须导出AppModule
- **配置**：扩展可以包含package.json和manifest.json
- **依赖**：扩展依赖必须声明在package.json中

### 8.3 扩展加载实现

extension加载是通过 `extension.utils.ts` 中的一系列函数实现的，完整流程包括：

1. **扩展列表获取**：
```typescript
export async function getExtensionList(extensionsDir?: string): Promise<ExtensionInfo[]> {
  const targetDir = extensionsDir || join(process.cwd(), '..', '..', 'extensions');
  const extensions: ExtensionInfo[] = [];

  try {
    const entries = await readdir(targetDir, { withFileTypes: true });
    for (const entry of entries) {
      if (!entry.isDirectory() || entry.name.startsWith('.')) continue;
      
      const extensionName = entry.name;
      const extensionPath = join(targetDir, extensionName);
      const buildPath = join(extensionPath, 'build');
      const indexPath = join(buildPath, 'index.js');
      
      if (!existsSync(indexPath)) {
        TerminalLogger.log('Extension', `Extension "${extensionName}" build not found, skipping...`);
        continue;
      }
      
      // 读取 package.json 获取元数据
      const packageJsonPath = join(extensionPath, 'package.json');
      let version = '0.0.0';
      let description: string | undefined;
      let author: { name: string; avatar?: string; homepage?: string } | undefined;
      
      if (existsSync(packageJsonPath)) {
        const packageJson = JSON.parse(await readFile(packageJsonPath, 'utf-8'));
        version = packageJson.version || '0.0.0';
        description = packageJson.description;
        // 解析 author 字段
      }
      
      extensions.push({ name: extensionName, identifier: extensionName, path: extensionPath, enabled: true, version, description, author });
    }
    return extensions;
  } catch (error) {
    const errorMessage = error instanceof Error ? error.message : String(error);
    TerminalLogger.error('Extensions', `Failed to load extensions: ${errorMessage}`);
    return extensions;
  }
}
```

2. **扩展模块加载**：
```typescript
export async function loadExtensionModule(extensionInfo: ExtensionInfo): Promise<DynamicModule | null> {
  try {
    const buildPath = join(extensionInfo.path, 'build');
    const indexPath = join(buildPath, 'index.js');
    const fileUrl = pathToFileURL(indexPath).href;
    const extensionModule = await import(fileUrl);

    if (!extensionModule.AppModule) {
      TerminalLogger.warn('Extension', `Extension "${extensionInfo.name}" does not export AppModule, skipping...`);
      return null;
    }

    TerminalLogger.success('Extension', `Loaded extension: ${extensionInfo.name}`);
    return extensionModule.AppModule;
  } catch (error) {
    const errorMessage = error instanceof Error ? error.message : String(error);
    TerminalLogger.error('Extension', `Failed to load extension "${extensionInfo.name}": ${errorMessage}`);
    return null;
  }
}
```

### 8.4 扩展DB配置与Schema管理

extension系统为每个extension创建独立的数据库Schema，确保数据隔离：

```typescript
export function getExtensionSchemaName(identifier: string): string {
  // Replace invalid characters with underscores
  // PostgreSQL schema names must start with a letter or underscore
  // and can only contain letters, numbers, and underscores
  let sanitized = identifier.toLowerCase().replace(/[^a-z0-9_]/g, '_');

  // Ensure it starts with a letter or underscore
  if (!/^[a-z_]/.test(sanitized)) {
    sanitized = `ext_${sanitized}`;
  }

  return sanitized;
}
```

### 8.5 extension包名称解析

系统提供了从调用栈中确定控制器所属extension包的机制：

```typescript
export function getExtensionPackNameFromControllerSync(): string {
  const extensions = getCachedExtensionList();

  // 如果只有一个插件,直接返回
  if (extensions.length === 1) {
    return extensions?.[0]!.name;
  }

  // 如果有多个插件,需要通过调用栈来确定
  if (!stackFinderFn) {
    throw new Error('未设置堆栈查找函数,请先调用 setStackFinderFn() 设置查找函数');
  }

  // 查找调用栈中的文件
  const callerFiles = stackFinderFn([".ts", ".js"]) || [];

  if (callerFiles.length === 0) {
    throw new Error("无法从调用栈中找到文件");
  }

  // 尝试从每个文件路径中提取插件名称
  for (const file of callerFiles) {
    const extensionName = extractExtensionNameFromPath(file);
    if (extensionName) {
      // 验证插件名称是否在缓存列表中
      const extension = extensions.find((p) => p.name === extensionName);
      if (extension) {
        return extension.name;
      }
    }
  }

  throw new Error("无法从调用栈中确定插件名称");
}
```

## 9. 部署方式

- **开发环境**：使用本地开发服务器
- **生产环境**：使用Docker容器化部署

## 10. 系统配置

- **数据库配置**：通过.env文件配置数据库连接信息
- **Redis配置**：通过.env文件配置Redis连接信息
- **日志配置**：通过.env文件配置日志级别和输出方式

## 11. 监控与维护

- **日志监控**：使用Winston日志框架记录系统日志
- **错误处理**：全局异常过滤器处理和返回统一的错误格式
- **性能监控**：使用监控工具监测系统性能
- **定期维护**：定期备份数据库和清理日志文件

## 12. 安全设计

- **认证机制**：JWT认证和权限控制
- **数据加密**：敏感数据加密存储
- **安全审计**：记录系统操作日志
- **输入验证**：对API请求参数进行验证
- **防护措施**：防止SQL注入、XSS攻击等安全威胁

---

**文档版本**：1.0.0
**作者**：BuildingAI团队
**创建时间**：2023年12月1日