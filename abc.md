graph TD
    subgraph "用户层"
        UI["用户界面 (Web UI - React/Vue)"]
    end

    subgraph "网关层"
        Gateway["API 网关 (API Gateway - Nginx/Kong)\n(路由、认证、限流、日志)"]
    end

    subgraph "应用与服务层"
        subgraph "微服务层"
            direction LR
            ML["[机器学习平台服务]\n- 工作流管理\n- 组件管理\n- 任务调度与执行"]
            Model["[模型超市服务]\n- 模型注册与管理\n- 模型部署与调用\n- 评价与互动"]
            KG["[知识图谱服务]\n- 数据抽取与映射\n- 图谱构建与融合\n- 图谱查询与可视化"]
            Script["[脚本开发工具服务]\n- 在线编辑器后端\n- 代码执行与调试\n- 版本控制"]
        end

        subgraph "共享服务/中间件"
            direction LR
            Auth["用户与认证中心 (OAuth2/JWT)"]
            MQ["消息队列 (RabbitMQ/Kafka)"]
            Compute["分布式计算引擎\n(Spark, Hive, MR)"]
            Config["配置中心 (Consul/Nacos)"]
            LogSvc["日志服务 (ELK Stack)"]
        end
    end
    
    subgraph "数据与存储层"
        direction LR
        SQL_DB["关系型数据库 (PostgreSQL)\n- 用户, 项目, 权限\n- 模型元数据, 评分\n- 脚本, 版本信息"]
        Object_Storage["对象存储 (MinIO/S3)\n- 数据集, 模型文件\n- 日志文件"]
        Graph_DB["图数据库 (Neo4j/JanusGraph)\n- 知识图谱实例数据"]
        Search_Engine["搜索引擎 (Elasticsearch)\n- 知识图谱全文检索\n- 日志索引"]
    end

    %% 定义连接关系
    UI --> Gateway
    Gateway --> ML
    Gateway --> Model
    Gateway --> KG
    Gateway --> Script
    
    ML --- Compute
    ML --- MQ
    Script --- Compute
    
    ML --> SQL_DB
    ML --> Object_Storage
    Model --> SQL_DB
    Model --> Object_Storage
    KG --> Graph_DB
    KG --> Search_Engine
    Script --> SQL_DB
    
    subgraph Legend ["(--- 表示内部调用/依赖)"]
        direction LR
    end
