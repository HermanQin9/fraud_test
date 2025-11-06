# Bank Fraud Detection System - 项目实施计划

## 📋 项目概述

**项目名称**: Enterprise Bank Fraud Detection & Data Pipeline System

**目标**: 构建一个企业级的银行欺诈检测系统，重点展示数据迁移、标准化和导入能力，对标 Verafin 职位要求。

**核心价值**: 
- 模拟从多个银行平台迁移和标准化交易数据
- 实现可扩展的数据导入管道
- 使用 AWS 云服务和 Postgres 数据库
- 展示企业级代码质量和测试实践

---

## 🎯 职位要求匹配度

### 直接匹配的技能展示：

| 职位要求 | 项目实现 |
|---------|---------|
| ✅ Java 开发经验 | 核心业务逻辑全部使用 Java 实现 |
| ✅ Postgres 数据库 | 使用 Postgres 存储和查询交易数据 |
| ✅ 数据标准化和导入 | ETL 管道处理多种数据格式 |
| ✅ AWS 云服务 | AWS RDS (Postgres), S3, Lambda 集成 |
| ✅ Unix utilities | Shell 脚本进行数据预处理 |
| ✅ 自动化测试 | JUnit 5 + Testcontainers 集成测试 |
| ✅ 可选: Scala | 可以添加 Scala 数据处理模块 |

---

## 🏗️ 项目架构

```
┌─────────────────────────────────────────────────────────────┐
│                    数据源 (Data Sources)                      │
│  CSV Files │ JSON APIs │ Legacy Banking Systems │ S3 Bucket  │
└────────────────────────┬────────────────────────────────────┘
                         │
                         ▼
┌─────────────────────────────────────────────────────────────┐
│              数据导入层 (Data Ingestion Layer)                │
│  - FileDataReader.java                                       │
│  - S3DataReader.java (AWS SDK)                              │
│  - DataValidator.java                                        │
│  - Unix Shell Scripts (data preprocessing)                   │
└────────────────────────┬────────────────────────────────────┘
                         │
                         ▼
┌─────────────────────────────────────────────────────────────┐
│            数据标准化层 (Normalization Layer)                 │
│  - TransactionNormalizer.java                               │
│  - DataMapper.java (多格式转标准格式)                          │
│  - SchemaValidator.java                                      │
└────────────────────────┬────────────────────────────────────┘
                         │
                         ▼
┌─────────────────────────────────────────────────────────────┐
│           数据持久化层 (Data Persistence Layer)               │
│  - PostgresRepository.java                                   │
│  - TransactionDAO.java                                       │
│  - JDBC Connection Pool (HikariCP)                          │
│  - Flyway Database Migrations                               │
└────────────────────────┬────────────────────────────────────┘
                         │
                         ▼
┌─────────────────────────────────────────────────────────────┐
│          欺诈检测引擎 (Fraud Detection Engine)                │
│  - RuleBasedDetector.java                                    │
│  - AnomalyDetector.java                                      │
│  - FraudScoringService.java                                  │
└────────────────────────┬────────────────────────────────────┘
                         │
                         ▼
┌─────────────────────────────────────────────────────────────┐
│            分析报告层 (Analytics & Reporting)                 │
│  - FraudReportGenerator.java                                │
│  - StatisticsService.java                                    │
│  - REST API (Spring Boot - Optional)                        │
└─────────────────────────────────────────────────────────────┘
```

---

## 📁 项目目录结构

```
BankFraudTest/
├── src/
│   ├── main/
│   │   ├── java/
│   │   │   └── com/
│   │   │       └── bankfraud/
│   │   │           ├── ingestion/          # 数据导入模块
│   │   │           │   ├── FileDataReader.java
│   │   │           │   ├── S3DataReader.java
│   │   │           │   ├── DataValidator.java
│   │   │           │   └── IngestionPipeline.java
│   │   │           │
│   │   │           ├── normalization/      # 数据标准化
│   │   │           │   ├── TransactionNormalizer.java
│   │   │           │   ├── DataMapper.java
│   │   │           │   └── SchemaValidator.java
│   │   │           │
│   │   │           ├── model/              # 数据模型
│   │   │           │   ├── Transaction.java
│   │   │           │   ├── Customer.java
│   │   │           │   ├── FraudAlert.java
│   │   │           │   └── DataSource.java
│   │   │           │
│   │   │           ├── repository/         # 数据访问层
│   │   │           │   ├── PostgresRepository.java
│   │   │           │   ├── TransactionDAO.java
│   │   │           │   └── ConnectionPool.java
│   │   │           │
│   │   │           ├── detection/          # 欺诈检测
│   │   │           │   ├── FraudDetector.java
│   │   │           │   ├── RuleEngine.java
│   │   │           │   ├── AnomalyDetector.java
│   │   │           │   └── FraudScoringService.java
│   │   │           │
│   │   │           ├── analytics/          # 分析和报告
│   │   │           │   ├── StatisticsService.java
│   │   │           │   ├── ReportGenerator.java
│   │   │           │   └── QueryOptimizer.java
│   │   │           │
│   │   │           ├── aws/                # AWS 集成
│   │   │           │   ├── S3Service.java
│   │   │           │   ├── RDSConnection.java
│   │   │           │   └── LambdaHandler.java (可选)
│   │   │           │
│   │   │           └── util/               # 工具类
│   │   │               ├── DateUtil.java
│   │   │               ├── CsvParser.java
│   │   │               └── JsonParser.java
│   │   │
│   │   ├── resources/
│   │   │   ├── application.properties      # 配置文件
│   │   │   ├── db/
│   │   │   │   └── migration/              # Flyway 数据库迁移
│   │   │   │       ├── V1__Create_transactions_table.sql
│   │   │   │       ├── V2__Create_customers_table.sql
│   │   │   │       └── V3__Create_fraud_alerts_table.sql
│   │   │   └── logback.xml                 # 日志配置
│   │   │
│   │   └── scripts/                        # Unix 脚本
│   │       ├── preprocess_data.sh          # 数据预处理
│   │       ├── import_batch.sh             # 批量导入
│   │       └── setup_db.sh                 # 数据库设置
│   │
│   └── test/
│       └── java/
│           └── com/
│               └── bankfraud/
│                   ├── ingestion/
│                   │   └── IngestionPipelineTest.java
│                   ├── normalization/
│                   │   └── TransactionNormalizerTest.java
│                   ├── repository/
│                   │   └── PostgresRepositoryTest.java
│                   └── detection/
│                       └── FraudDetectorTest.java
│
├── data/
│   ├── sample/                             # 示例数据（提交到 Git）
│   │   ├── transactions_bank_a.csv
│   │   ├── transactions_bank_b.json
│   │   └── sample_fraud_cases.csv
│   ├── raw/                                # 原始数据（不提交）
│   └── processed/                          # 处理后数据（不提交）
│
├── docs/
│   ├── SETUP.md                            # 环境配置指南
│   ├── API_DESIGN.md                       # API 设计文档
│   ├── DATA_SCHEMA.md                      # 数据库架构
│   └── AWS_DEPLOYMENT.md                   # AWS 部署指南
│
├── docker/
│   ├── docker-compose.yml                  # 本地开发环境
│   └── Dockerfile                          # 应用容器化
│
├── pom.xml                                 # Maven 配置
├── README.md                               # 项目说明
├── PROJECT_PLAN.md                         # 本文件
└── .gitignore
```

---

## 🛠️ 技术栈详细说明

### 核心技术

1. **Java 17** (LTS 版本)
   - 主要开发语言
   - 使用现代 Java 特性（Records, Pattern Matching, Stream API）

2. **Maven 3.9+**
   - 依赖管理和构建工具
   - 统一项目结构

3. **PostgreSQL 15**
   - 主要数据库
   - 使用高级特性（JSON 类型、窗口函数、索引优化）

4. **AWS Services**
   - **AWS RDS**: Postgres 云数据库托管
   - **AWS S3**: 存储原始数据文件和备份
   - **AWS SDK for Java**: 与 AWS 服务交互
   - **可选: AWS Lambda**: 无服务器数据处理

### 关键依赖库

```xml
<!-- 数据库相关 -->
- PostgreSQL JDBC Driver (42.6.0)
- HikariCP (连接池)
- Flyway (数据库迁移)

<!-- AWS -->
- AWS SDK for Java 2.x
- S3 Client
- RDS Client

<!-- 测试 -->
- JUnit 5
- Mockito
- Testcontainers (集成测试)
- AssertJ (流式断言)

<!-- 数据处理 -->
- Apache Commons CSV
- Jackson (JSON 处理)
- Apache Commons Math (统计分析)

<!-- 日志 -->
- SLF4J + Logback

<!-- 工具 -->
- Lombok (减少样板代码)
- Google Guava (工具类)
```

### Unix 工具集成

- Bash 脚本进行数据预处理
- `awk`, `sed` 进行文本处理
- `psql` 命令行工具
- cron 任务调度（可选）

---

## 📊 数据库设计

### 核心表结构

#### 1. transactions (交易表)
```sql
CREATE TABLE transactions (
    transaction_id VARCHAR(50) PRIMARY KEY,
    customer_id VARCHAR(50) NOT NULL,
    transaction_date TIMESTAMP NOT NULL,
    amount DECIMAL(12, 2) NOT NULL,
    currency VARCHAR(3) DEFAULT 'USD',
    merchant_name VARCHAR(255),
    merchant_category VARCHAR(50),
    transaction_type VARCHAR(20),
    card_last_four VARCHAR(4),
    location_country VARCHAR(2),
    location_city VARCHAR(100),
    ip_address INET,
    device_fingerprint VARCHAR(255),
    is_online BOOLEAN DEFAULT TRUE,
    source_system VARCHAR(50),      -- 来源银行系统
    raw_data JSONB,                 -- 原始数据（JSON格式）
    normalized_at TIMESTAMP,
    created_at TIMESTAMP DEFAULT NOW(),
    INDEX idx_customer_id (customer_id),
    INDEX idx_transaction_date (transaction_date),
    INDEX idx_amount (amount),
    INDEX idx_source_system (source_system)
);
```

#### 2. customers (客户表)
```sql
CREATE TABLE customers (
    customer_id VARCHAR(50) PRIMARY KEY,
    first_name VARCHAR(100),
    last_name VARCHAR(100),
    email VARCHAR(255),
    phone VARCHAR(20),
    account_created_date DATE,
    risk_level VARCHAR(20) DEFAULT 'LOW',
    total_transactions INTEGER DEFAULT 0,
    lifetime_value DECIMAL(15, 2) DEFAULT 0,
    last_transaction_date TIMESTAMP,
    created_at TIMESTAMP DEFAULT NOW(),
    updated_at TIMESTAMP DEFAULT NOW()
);
```

#### 3. fraud_alerts (欺诈警报表)
```sql
CREATE TABLE fraud_alerts (
    alert_id SERIAL PRIMARY KEY,
    transaction_id VARCHAR(50) REFERENCES transactions(transaction_id),
    customer_id VARCHAR(50) REFERENCES customers(customer_id),
    alert_type VARCHAR(50) NOT NULL,
    fraud_score DECIMAL(5, 2),      -- 0-100 风险分数
    risk_level VARCHAR(20),          -- LOW, MEDIUM, HIGH, CRITICAL
    rules_triggered TEXT[],          -- 触发的规则列表
    description TEXT,
    status VARCHAR(20) DEFAULT 'PENDING',  -- PENDING, CONFIRMED, FALSE_POSITIVE
    created_at TIMESTAMP DEFAULT NOW(),
    reviewed_at TIMESTAMP,
    reviewed_by VARCHAR(100),
    INDEX idx_status (status),
    INDEX idx_risk_level (risk_level),
    INDEX idx_created_at (created_at)
);
```

#### 4. data_import_logs (数据导入日志)
```sql
CREATE TABLE data_import_logs (
    log_id SERIAL PRIMARY KEY,
    source_system VARCHAR(50),
    file_name VARCHAR(255),
    records_processed INTEGER,
    records_success INTEGER,
    records_failed INTEGER,
    start_time TIMESTAMP,
    end_time TIMESTAMP,
    duration_seconds INTEGER,
    error_message TEXT,
    status VARCHAR(20),
    created_at TIMESTAMP DEFAULT NOW()
);
```

---

## 🔄 实施阶段

### **Phase 1: 基础设施搭建** (第 1-2 天)

**目标**: 建立项目基础和开发环境

✅ **任务清单**:
- [ ] 创建 Maven 项目结构
- [ ] 配置 `pom.xml` 添加所有依赖
- [ ] 设置本地 Postgres 数据库 (Docker)
- [ ] 创建 Flyway 数据库迁移脚本
- [ ] 配置日志系统 (Logback)
- [ ] 创建基础配置文件
- [ ] 设置 AWS 账户和凭证 (Free Tier)
- [ ] 创建 S3 Bucket

**交付物**:
- 可运行的项目骨架
- 数据库表结构完成
- Docker Compose 本地环境

---

### **Phase 2: 数据导入和标准化** (第 3-5 天)

**目标**: 实现核心 ETL 功能

✅ **任务清单**:
- [ ] 创建数据模型类 (Transaction, Customer)
- [ ] 实现 FileDataReader (CSV/JSON 解析)
- [ ] 实现 S3DataReader (AWS S3 集成)
- [ ] 实现 TransactionNormalizer
- [ ] 实现 DataValidator
- [ ] 编写 Unix Shell 脚本进行数据预处理
- [ ] 实现完整的 IngestionPipeline
- [ ] 创建测试数据集（模拟多个银行系统格式）

**核心类实现**:
```java
// 示例接口设计
public interface DataReader {
    List<RawTransaction> readData(String source);
}

public class TransactionNormalizer {
    public Transaction normalize(RawTransaction raw, DataSource source) {
        // 不同来源的标准化逻辑
    }
}

public class IngestionPipeline {
    public ImportResult process(String sourceFile, DataSource source) {
        // 1. 读取数据
        // 2. 验证
        // 3. 标准化
        // 4. 存储到数据库
        // 5. 记录日志
    }
}
```

**交付物**:
- 可以从多种格式导入数据
- 数据标准化工作正常
- 单元测试覆盖率 > 80%

---

### **Phase 3: 数据持久化和查询优化** (第 6-7 天)

**目标**: 实现高效的数据库操作

✅ **任务清单**:
- [ ] 实现 PostgresRepository
- [ ] 配置 HikariCP 连接池
- [ ] 实现 TransactionDAO (CRUD 操作)
- [ ] 优化 SQL 查询（使用索引、EXPLAIN ANALYZE）
- [ ] 实现批量插入优化
- [ ] 添加数据库事务管理
- [ ] 实现数据库连接到 AWS RDS
- [ ] 编写集成测试 (Testcontainers)

**性能优化重点**:
- 批量插入（JDBC Batch）
- 准备语句（PreparedStatement）
- 索引优化
- 查询计划分析

**交付物**:
- 高效的数据访问层
- 支持本地和 AWS RDS
- 集成测试通过

---

### **Phase 4: 欺诈检测引擎** (第 8-10 天)

**目标**: 实现业务逻辑和欺诈检测

✅ **任务清单**:
- [ ] 实现基于规则的欺诈检测
  - 异常金额检测
  - 高频交易检测
  - 地理位置异常检测
  - 时间模式异常检测
- [ ] 实现欺诈评分系统
- [ ] 创建 FraudAlert 生成逻辑
- [ ] 实现统计分析功能
- [ ] 添加异常检测算法（Z-Score, IQR）

**检测规则示例**:
```java
public class FraudRules {
    // 规则1: 单笔交易超过正常金额3倍标准差
    // 规则2: 30分钟内超过5笔交易
    // 规则3: 不同国家在1小时内有交易
    // 规则4: 深夜大额交易
    // 规则5: 与历史行为模式不符
}
```

**交付物**:
- 工作的欺诈检测系统
- 生成准确的欺诈警报
- 可配置的规则引擎

---

### **Phase 5: 分析和报告** (第 11-12 天)

**目标**: 实现数据分析和报告生成

✅ **任务清单**:
- [ ] 实现 StatisticsService
  - 交易统计（总量、平均值、中位数）
  - 欺诈率计算
  - 客户风险分析
- [ ] 实现 ReportGenerator
  - 日报、周报、月报
  - 欺诈趋势分析
  - 导出 CSV/JSON 格式
- [ ] SQL 查询优化
- [ ] 创建数据可视化准备（预留接口）

**交付物**:
- 完整的统计分析功能
- 可生成专业报告
- 优化的复杂查询

---

### **Phase 6: AWS 集成和部署** (第 13-14 天)

**目标**: 展示云服务集成能力

✅ **任务清单**:
- [ ] 配置 AWS RDS (Postgres)
- [ ] 实现 S3 数据上传/下载
- [ ] 配置 AWS 凭证管理
- [ ] 创建部署文档
- [ ] (可选) 实现 AWS Lambda 函数进行数据处理
- [ ] (可选) 设置 CloudWatch 监控

**AWS 架构**:
```
Local Java App / Lambda
    ↓
AWS S3 (Raw Data Storage)
    ↓
Java ETL Pipeline
    ↓
AWS RDS (PostgreSQL)
    ↓
Query & Analysis
```

**交付物**:
- 应用可连接 AWS RDS
- S3 集成工作正常
- 部署文档完整

---

### **Phase 7: 测试和文档** (第 15-16 天)

**目标**: 确保代码质量和项目可展示性

✅ **任务清单**:
- [ ] 完善单元测试（目标覆盖率 > 80%）
- [ ] 编写集成测试
- [ ] 性能测试（处理 10 万条记录）
- [ ] 编写完整的 README.md
- [ ] 创建 API 文档
- [ ] 创建演示视频/截图
- [ ] 代码审查和重构
- [ ] 添加代码注释

**文档清单**:
- ✅ README.md (项目介绍、快速开始)
- ✅ SETUP.md (详细环境配置)
- ✅ API_DESIGN.md (API 设计说明)
- ✅ AWS_DEPLOYMENT.md (AWS 部署指南)
- ✅ DATA_SCHEMA.md (数据库设计文档)

---

## 🎨 项目亮点（展示给雇主）

### 1. **企业级数据迁移能力**
   - 处理多种数据格式和来源
   - 数据标准化和验证
   - 完整的错误处理和日志记录

### 2. **AWS 云服务实践经验**
   - RDS (Postgres) 数据库管理
   - S3 对象存储集成
   - 云原生架构设计

### 3. **数据库专业技能**
   - 复杂 SQL 查询
   - 性能优化（索引、批量操作）
   - 数据库迁移管理（Flyway）

### 4. **代码质量**
   - 高测试覆盖率
   - 清晰的代码结构
   - 设计模式应用（Repository, Factory, Strategy）

### 5. **Unix/Linux 技能**
   - Shell 脚本自动化
   - 数据处理管道

### 6. **文档和沟通能力**
   - 详细的技术文档
   - 清晰的架构设计
   - 易于理解的代码注释

---

## 📈 性能目标

- 每秒处理 **1000+ 条交易记录**
- 数据导入错误率 < **0.1%**
- 数据库查询响应时间 < **100ms** (95th percentile)
- 欺诈检测准确率 > **85%**
- 单元测试覆盖率 > **80%**
- 可扩展到 **百万级** 交易数据

---

## 🚀 快速开始时间表

### 最小可行产品 (MVP) - 1 周
- 基础项目结构
- 简单的 CSV 导入
- Postgres 存储
- 基础欺诈检测规则

### 完整项目 - 2-3 周
- 所有功能模块完成
- AWS 集成
- 完整测试和文档

### 增强版 - 4 周
- 添加 Scala 模块
- REST API (Spring Boot)
- 前端展示界面
- 机器学习模型集成

---

## 📝 面试准备要点

在面试中可以强调的技术点：

1. **数据迁移经验**
   - "我构建了一个 ETL 管道，能够从多个不同格式的数据源导入和标准化交易数据到 Postgres"

2. **AWS 实践**
   - "使用 AWS RDS 托管 Postgres 数据库，S3 存储原始数据文件，确保数据的可靠性和可扩展性"

3. **数据库优化**
   - "通过批量插入、连接池和索引优化，将数据导入性能提升了 10 倍"

4. **问题解决能力**
   - "处理了多种数据格式和不一致性问题，实现了灵活的数据标准化策略"

5. **测试驱动开发**
   - "使用 JUnit 和 Testcontainers 实现了 80%+ 的测试覆盖率，确保代码质量"

---

## 🔗 相关资源

### 学习资源
- [ ] JDBC 最佳实践
- [ ] AWS SDK for Java 文档
- [ ] Postgres 性能优化指南
- [ ] Java 并发编程（批量处理）

### 数据集
- Kaggle: Credit Card Fraud Detection
- IEEE-CIS Fraud Detection
- 自己生成的模拟数据

---

## ⚠️ 注意事项

1. **不要过度设计**: 先完成核心功能，再添加高级特性
2. **保持代码简洁**: 重点展示关键技能，不是所有功能
3. **注重文档**: 好的文档和注释比复杂的代码更重要
4. **AWS 成本控制**: 使用 Free Tier，记得关闭不用的资源
5. **数据隐私**: 使用模拟数据，不要使用真实客户数据

---

## 📞 下一步行动

1. **立即开始**: 创建项目基础结构
2. **每日提交**: 保持 Git commit 历史
3. **边做边学**: 遇到问题及时查文档和教程
4. **寻求反馈**: 定期让朋友或导师审查代码
5. **准备演示**: 项目完成后录制演示视频

---

**预计完成时间**: 2-3 周（兼职）/ 1-2 周（全职投入）

**优先级**: ⭐⭐⭐⭐⭐ 高度对标职位要求！

Good luck! 🚀
