# Bank Fraud Detection System - 


****: Enterprise Bank Fraud Detection & Data Pipeline System

****: Verafin 

****: 
- 
- 
- AWS Postgres 
- 

---



| | |
|---------|---------|
| Java | Java |
| Postgres | Postgres |
| | ETL |
| AWS | AWS RDS (Postgres), S3, Lambda |
| Unix utilities | Shell |
| | JUnit 5 + Testcontainers |
| : Scala | Scala |

---


```

 (Data Sources) 
 CSV Files JSON APIs Legacy Banking Systems S3 Bucket 

 
 

 (Data Ingestion Layer) 
 - FileDataReader.java 
 - S3DataReader.java (AWS SDK) 
 - DataValidator.java 
 - Unix Shell Scripts (data preprocessing) 

 
 

 (Normalization Layer) 
 - TransactionNormalizer.java 
 - DataMapper.java () 
 - SchemaValidator.java 

 
 

 (Data Persistence Layer) 
 - PostgresRepository.java 
 - TransactionDAO.java 
 - JDBC Connection Pool (HikariCP) 
 - Flyway Database Migrations 

 
 

 (Fraud Detection Engine) 
 - RuleBasedDetector.java 
 - AnomalyDetector.java 
 - FraudScoringService.java 

 
 

 (Analytics & Reporting) 
 - FraudReportGenerator.java 
 - StatisticsService.java 
 - REST API (Spring Boot - Optional) 

```

---


```
BankFraudTest/
 src/
 main/
 java/
 com/
 bankfraud/
 ingestion/ # 
 FileDataReader.java
 S3DataReader.java
 DataValidator.java
 IngestionPipeline.java
 
 normalization/ # 
 TransactionNormalizer.java
 DataMapper.java
 SchemaValidator.java
 
 model/ # 
 Transaction.java
 Customer.java
 FraudAlert.java
 DataSource.java
 
 repository/ # 
 PostgresRepository.java
 TransactionDAO.java
 ConnectionPool.java
 
 detection/ # 
 FraudDetector.java
 RuleEngine.java
 AnomalyDetector.java
 FraudScoringService.java
 
 analytics/ # 
 StatisticsService.java
 ReportGenerator.java
 QueryOptimizer.java
 
 aws/ # AWS 
 S3Service.java
 RDSConnection.java
 LambdaHandler.java ()
 
 util/ # 
 DateUtil.java
 CsvParser.java
 JsonParser.java
 
 resources/
 application.properties # 
 db/
 migration/ # Flyway 
 V1__Create_transactions_table.sql
 V2__Create_customers_table.sql
 V3__Create_fraud_alerts_table.sql
 logback.xml # 
 
 scripts/ # Unix 
 preprocess_data.sh # 
 import_batch.sh # 
 setup_db.sh # 
 
 test/
 java/
 com/
 bankfraud/
 ingestion/
 IngestionPipelineTest.java
 normalization/
 TransactionNormalizerTest.java
 repository/
 PostgresRepositoryTest.java
 detection/
 FraudDetectorTest.java

 data/
 sample/ # Git
 transactions_bank_a.csv
 transactions_bank_b.json
 sample_fraud_cases.csv
 raw/ # 
 processed/ # 

 docs/
 SETUP.md # 
 API_DESIGN.md # API 
 DATA_SCHEMA.md # 
 AWS_DEPLOYMENT.md # AWS 

 docker/
 docker-compose.yml # 
 Dockerfile # 

 pom.xml # Maven 
 README.md # 
 PROJECT_PLAN.md # 
 .gitignore
```

---



1. **Java 17** (LTS )
 - 
 - Java Records, Pattern Matching, Stream API

2. **Maven 3.9+**
 - 
 - 

3. **PostgreSQL 15**
 - 
 - JSON 

4. **AWS Services**
 - **AWS RDS**: Postgres 
 - **AWS S3**: 
 - **AWS SDK for Java**: AWS 
 - **: AWS Lambda**: 


```xml
<!-- -->
- PostgreSQL JDBC Driver (42.6.0)
- HikariCP ()
- Flyway ()

<!-- AWS -->
- AWS SDK for Java 2.x
- S3 Client
- RDS Client

<!-- -->
- JUnit 5
- Mockito
- Testcontainers ()
- AssertJ ()

<!-- -->
- Apache Commons CSV
- Jackson (JSON )
- Apache Commons Math ()

<!-- -->
- SLF4J + Logback

<!-- -->
- Lombok ()
- Google Guava ()
```

### Unix 

- Bash 
- `awk`, `sed` 
- `psql` 
- cron 

---



#### 1. transactions ()
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
 source_system VARCHAR(50), -- 
 raw_data JSONB, -- JSON
 normalized_at TIMESTAMP,
 created_at TIMESTAMP DEFAULT NOW(),
 INDEX idx_customer_id (customer_id),
 INDEX idx_transaction_date (transaction_date),
 INDEX idx_amount (amount),
 INDEX idx_source_system (source_system)
);
```

#### 2. customers ()
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

#### 3. fraud_alerts ()
```sql
CREATE TABLE fraud_alerts (
 alert_id SERIAL PRIMARY KEY,
 transaction_id VARCHAR(50) REFERENCES transactions(transaction_id),
 customer_id VARCHAR(50) REFERENCES customers(customer_id),
 alert_type VARCHAR(50) NOT NULL,
 fraud_score DECIMAL(5, 2), -- 0-100 
 risk_level VARCHAR(20), -- LOW, MEDIUM, HIGH, CRITICAL
 rules_triggered TEXT[], -- 
 description TEXT,
 status VARCHAR(20) DEFAULT 'PENDING', -- PENDING, CONFIRMED, FALSE_POSITIVE
 created_at TIMESTAMP DEFAULT NOW(),
 reviewed_at TIMESTAMP,
 reviewed_by VARCHAR(100),
 INDEX idx_status (status),
 INDEX idx_risk_level (risk_level),
 INDEX idx_created_at (created_at)
);
```

#### 4. data_import_logs ()
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


### **Phase 1: ** ( 1-2 )

****: 

 ****:
- [ ] Maven 
- [ ] `pom.xml` 
- [ ] Postgres (Docker)
- [ ] Flyway 
- [ ] (Logback)
- [ ] 
- [ ] AWS (Free Tier)
- [ ] S3 Bucket

****:
- 
- 
- Docker Compose 

---

### **Phase 2: ** ( 3-5 )

****: ETL 

 ****:
- [ ] (Transaction, Customer)
- [ ] FileDataReader (CSV/JSON )
- [ ] S3DataReader (AWS S3 )
- [ ] TransactionNormalizer
- [ ] DataValidator
- [ ] Unix Shell 
- [ ] IngestionPipeline
- [ ] 

****:
```java
// 
public interface DataReader {
 List<RawTransaction> readData(String source);
}

public class TransactionNormalizer {
 public Transaction normalize(RawTransaction raw, DataSource source) {
 // 
 }
}

public class IngestionPipeline {
 public ImportResult process(String sourceFile, DataSource source) {
 // 1. 
 // 2. 
 // 3. 
 // 4. 
 // 5. 
 }
}
```

****:
- 
- 
- > 80%

---

### **Phase 3: ** ( 6-7 )

****: 

 ****:
- [ ] PostgresRepository
- [ ] HikariCP 
- [ ] TransactionDAO (CRUD )
- [ ] SQL EXPLAIN ANALYZE
- [ ] 
- [ ] 
- [ ] AWS RDS
- [ ] (Testcontainers)

****:
- JDBC Batch
- PreparedStatement
- 
- 

****:
- 
- AWS RDS
- 

---

### **Phase 4: ** ( 8-10 )

****: 

 ****:
- [ ] 
 - 
 - 
 - 
 - 
- [ ] 
- [ ] FraudAlert 
- [ ] 
- [ ] Z-Score, IQR

****:
```java
public class FraudRules {
 // 1: 3
 // 2: 305
 // 3: 1
 // 4: 
 // 5: 
}
```

****:
- 
- 
- 

---

### **Phase 5: ** ( 11-12 )

****: 

 ****:
- [ ] StatisticsService
 - 
 - 
 - 
- [ ] ReportGenerator
 - 
 - 
 - CSV/JSON 
- [ ] SQL 
- [ ] 

****:
- 
- 
- 

---

### **Phase 6: AWS ** ( 13-14 )

****: 

 ****:
- [ ] AWS RDS (Postgres)
- [ ] S3 /
- [ ] AWS 
- [ ] 
- [ ] () AWS Lambda 
- [ ] () CloudWatch 

**AWS **:
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

****:
- AWS RDS
- S3 
- 

---

### **Phase 7: ** ( 15-16 )

****: 

 ****:
- [ ] > 80%
- [ ] 
- [ ] 10 
- [ ] README.md
- [ ] API 
- [ ] /
- [ ] 
- [ ] 

****:
- README.md ()
- SETUP.md ()
- API_DESIGN.md (API )
- AWS_DEPLOYMENT.md (AWS )
- DATA_SCHEMA.md ()

---


### 1. ****
 - 
 - 
 - 

### 2. **AWS **
 - RDS (Postgres) 
 - S3 
 - 

### 3. ****
 - SQL 
 - 
 - Flyway

### 4. ****
 - 
 - 
 - Repository, Factory, Strategy

### 5. **Unix/Linux **
 - Shell 
 - 

### 6. ****
 - 
 - 
 - 

---


- **1000+ **
- < **0.1%**
- < **100ms** (95th percentile)
- > **85%**
- > **80%**
- **** 

---


### (MVP) - 1 
- 
- CSV 
- Postgres 
- 

### - 2-3 
- 
- AWS 
- 

### - 4 
- Scala 
- REST API (Spring Boot)
- 
- 

---


1. ****
 - " ETL Postgres"

2. **AWS **
 - " AWS RDS Postgres S3 "

3. ****
 - " 10 "

4. ****
 - ""

5. ****
 - " JUnit Testcontainers 80%+ "

---



- [ ] JDBC 
- [ ] AWS SDK for Java 
- [ ] Postgres 
- [ ] Java 


- Kaggle: Credit Card Fraud Detection
- IEEE-CIS Fraud Detection
- 

---


1. ****: 
2. ****: 
3. ****: 
4. **AWS **: Free Tier
5. ****: 

---


1. ****: 
2. ****: Git commit 
3. ****: 
4. ****: 
5. ****: 

---

****: 2-3 / 1-2 

****: 

Good luck! 
