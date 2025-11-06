# 🏦 Enterprise Bank Fraud Detection & Data Pipeline System

[![Java](https://img.shields.io/badge/Java-17-orange.svg)](https://www.oracle.com/java/)
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-15-blue.svg)](https://www.postgresql.org/)
[![AWS](https://img.shields.io/badge/AWS-RDS%20%7C%20S3-orange.svg)](https://aws.amazon.com/)
[![Maven](https://img.shields.io/badge/Maven-3.9+-red.svg)](https://maven.apache.org/)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

> **A production-grade fraud detection system demonstrating enterprise data migration, normalization, and ingestion capabilities with AWS cloud integration.**

## 📋 Project Overview

This project simulates an **enterprise-level banking fraud detection system** with a focus on **data pipeline engineering**. It showcases the ability to migrate and normalize transaction data from multiple banking platforms into a centralized cloud-based PostgreSQL database, then perform real-time fraud detection and analytics.

### 🎯 Key Objectives

- ✅ Implement **ETL pipelines** for multi-source data ingestion
- ✅ Demonstrate **data normalization** and schema validation
- ✅ Showcase **AWS cloud integration** (RDS, S3)
- ✅ Build **scalable data processing** with Java and PostgreSQL
- ✅ Apply **enterprise-grade coding practices** with comprehensive testing

---

## 🏗️ System Architecture

```
┌─────────────────────────────────────────────────────────────┐
│           Data Sources (CSV, JSON, Legacy Systems)           │
└────────────────────────┬────────────────────────────────────┘
                         │
                         ▼
┌─────────────────────────────────────────────────────────────┐
│                 Data Ingestion Layer (Java)                  │
│  • FileDataReader  • S3DataReader  • DataValidator          │
└────────────────────────┬────────────────────────────────────┘
                         │
                         ▼
┌─────────────────────────────────────────────────────────────┐
│              Normalization Layer (Java)                      │
│  • TransactionNormalizer  • SchemaValidator                  │
└────────────────────────┬────────────────────────────────────┘
                         │
                         ▼
┌─────────────────────────────────────────────────────────────┐
│          PostgreSQL Database (AWS RDS / Local)               │
│  • Optimized schema  • Indexes  • Migrations (Flyway)       │
└────────────────────────┬────────────────────────────────────┘
                         │
                         ▼
┌─────────────────────────────────────────────────────────────┐
│          Fraud Detection Engine & Analytics                  │
│  • Rule-based detection  • Anomaly detection  • Reports     │
└─────────────────────────────────────────────────────────────┘
```

---

## 🚀 Key Features

### 1. **Enterprise Data Migration**
- Import data from multiple formats (CSV, JSON, XML)
- Handle inconsistent schemas from different banking systems
- Comprehensive data validation and error handling
- Batch processing with transaction support

### 2. **Data Normalization & Validation**
- Standardize transaction data from diverse sources
- Schema validation and data quality checks
- Currency conversion and timezone handling
- Duplicate detection and data deduplication

### 3. **AWS Cloud Integration**
- **AWS RDS (PostgreSQL)**: Managed database hosting
- **AWS S3**: Raw data file storage and archival
- **AWS SDK for Java**: Seamless cloud service integration
- Infrastructure-as-code ready architecture

### 4. **High-Performance Database Operations**
- Connection pooling (HikariCP)
- Batch insert optimization (1000+ records/sec)
- Query optimization with proper indexing
- Database migrations with Flyway

### 5. **Fraud Detection Engine**
- Rule-based fraud detection (5+ detection rules)
- Anomaly detection using statistical methods
- Fraud scoring system (0-100 risk score)
- Real-time alert generation

### 6. **Unix/Linux Integration**
- Shell scripts for data preprocessing
- Automated batch processing
- Log aggregation and monitoring

### 7. **Comprehensive Testing**
- Unit tests with JUnit 5
- Integration tests with Testcontainers
- 80%+ code coverage target
- Performance benchmarking

---

## 🛠️ Technology Stack

### Core Technologies
- **Java 17** (LTS) - Primary programming language
- **PostgreSQL 15** - Relational database
- **Maven 3.9+** - Build and dependency management
- **AWS SDK 2.x** - Cloud services integration

### Key Dependencies
```xml
<!-- Database -->
- PostgreSQL JDBC Driver
- HikariCP (connection pooling)
- Flyway (database migrations)

<!-- AWS Services -->
- AWS SDK for Java 2.x (S3, RDS)

<!-- Data Processing -->
- Apache Commons CSV
- Jackson (JSON processing)
- Apache Commons Math (statistics)

<!-- Testing -->
- JUnit 5
- Mockito
- Testcontainers
- AssertJ

<!-- Utilities -->
- Lombok
- SLF4J + Logback
- Google Guava
```

### Development Tools
- **Docker & Docker Compose** - Local development environment
- **Git** - Version control
- **IntelliJ IDEA / VS Code** - IDEs
- **DBeaver / pgAdmin** - Database management

---

## 📁 Project Structure

```
BankFraudTest/
├── src/
│   ├── main/
│   │   ├── java/com/bankfraud/
│   │   │   ├── ingestion/          # Data import modules
│   │   │   ├── normalization/      # Data standardization
│   │   │   ├── model/              # Domain models
│   │   │   ├── repository/         # Data access layer
│   │   │   ├── detection/          # Fraud detection engine
│   │   │   ├── analytics/          # Reporting and analytics
│   │   │   ├── aws/                # AWS integration
│   │   │   └── util/               # Utilities
│   │   ├── resources/
│   │   │   ├── application.properties
│   │   │   └── db/migration/       # Flyway SQL scripts
│   │   └── scripts/                # Unix shell scripts
│   └── test/                       # Unit and integration tests
├── data/
│   ├── sample/                     # Sample datasets
│   └── raw/                        # Raw data files
├── docs/                           # Documentation
├── docker/                         # Docker configurations
├── pom.xml                         # Maven configuration
└── PROJECT_PLAN.md                 # Detailed implementation plan
```

---

## 🚦 Quick Start

### Prerequisites
- **Java 17** or higher
- **Maven 3.9+**
- **Docker & Docker Compose** (for local PostgreSQL)
- **Git**
- **AWS Account** (for cloud deployment, optional)

### Installation

1. **Clone the repository**
   ```bash
   git clone https://github.com/YOUR_USERNAME/BankFraudTest.git
   cd BankFraudTest
   ```

2. **Start PostgreSQL database (Docker)**
   ```bash
   docker-compose up -d
   ```

3. **Build the project**
   ```bash
   mvn clean install
   ```

4. **Run database migrations**
   ```bash
   mvn flyway:migrate
   ```

5. **Run the application**
   ```bash
   mvn exec:java -Dexec.mainClass="com.bankfraud.Main"
   ```

### Running Tests
```bash
# Run all tests
mvn test

# Run with coverage
mvn test jacoco:report
```

---

## 📊 Database Schema

### Core Tables

#### transactions
- Stores normalized transaction data
- Supports multiple source systems
- Includes JSONB for raw data retention
- Optimized indexes for performance

#### customers
- Customer master data
- Risk profiling
- Transaction history aggregates

#### fraud_alerts
- Fraud detection results
- Risk scoring (0-100)
- Rule tracking and audit trail

#### data_import_logs
- ETL job tracking
- Performance metrics
- Error logging

**See [docs/DATA_SCHEMA.md](docs/DATA_SCHEMA.md) for detailed schema documentation.**

---

## 🔄 Data Ingestion Pipeline

### Supported Data Sources
1. **CSV Files** - Bank A format
2. **JSON Files** - Bank B format
3. **AWS S3** - Cloud storage integration
4. **Legacy Database Exports** - XML format

### Pipeline Workflow
```java
1. Read data from source (File, S3, API)
2. Validate data format and completeness
3. Normalize to standard schema
4. Validate business rules
5. Insert into PostgreSQL (batch mode)
6. Log import results
7. Trigger fraud detection
```

### Example Usage
```java
IngestionPipeline pipeline = new IngestionPipeline();
ImportResult result = pipeline.process("data/raw/bank_a.csv", DataSource.BANK_A);
System.out.println("Imported: " + result.getSuccessCount() + " records");
```

---

## 🛡️ Fraud Detection Rules

The system implements multiple detection strategies:

1. **High-Value Transaction Rule** - Transactions > 3σ above user average
2. **Velocity Rule** - > 5 transactions in 30 minutes
3. **Geographic Anomaly** - Transactions from different countries within 1 hour
4. **Time-Based Rule** - Large transactions during unusual hours (2-5 AM)
5. **Behavioral Anomaly** - Deviation from historical patterns

### Fraud Scoring Algorithm
```
Fraud Score = Σ(rule_weight × rule_confidence)
Risk Level = LOW (0-25) | MEDIUM (26-50) | HIGH (51-75) | CRITICAL (76-100)
```

---

## ☁️ AWS Deployment

### AWS Services Used
- **AWS RDS PostgreSQL** - Production database
- **AWS S3** - Data lake for raw files
- **AWS SDK for Java** - Service integration

### Configuration
```properties
# application.properties
aws.region=us-east-1
aws.s3.bucket=bank-fraud-data
aws.rds.endpoint=your-db.rds.amazonaws.com
aws.rds.database=frauddb
```

**See [docs/AWS_DEPLOYMENT.md](docs/AWS_DEPLOYMENT.md) for deployment guide.**

---

## 📈 Performance Metrics

| Metric | Target | Achieved |
|--------|--------|----------|
| Data Ingestion Rate | 1000 records/sec | ✅ TBD |
| Query Response Time (p95) | < 100ms | ✅ TBD |
| Import Error Rate | < 0.1% | ✅ TBD |
| Test Coverage | > 80% | ✅ TBD |
| Fraud Detection Accuracy | > 85% | ✅ TBD |

---

## 🧪 Testing Strategy

### Test Types
- **Unit Tests** - Individual component testing
- **Integration Tests** - Database and AWS integration
- **Performance Tests** - Load testing with 100K+ records
- **End-to-End Tests** - Complete pipeline validation

### Test Infrastructure
```bash
# Uses Testcontainers for isolated testing
- PostgreSQL container
- Mocked AWS services (LocalStack)
- In-memory data fixtures
```

---

## 📚 Documentation

- **[PROJECT_PLAN.md](PROJECT_PLAN.md)** - Comprehensive implementation plan
- **[docs/SETUP.md](docs/SETUP.md)** - Environment setup guide
- **[docs/API_DESIGN.md](docs/API_DESIGN.md)** - API documentation
- **[docs/DATA_SCHEMA.md](docs/DATA_SCHEMA.md)** - Database design
- **[docs/AWS_DEPLOYMENT.md](docs/AWS_DEPLOYMENT.md)** - Cloud deployment guide

---

## 🎯 Skills Demonstrated

This project showcases the following technical competencies:

✅ **Data Engineering**
- ETL pipeline design and implementation
- Data normalization and validation
- Multi-source data integration
- Error handling and data quality

✅ **Java Development**
- Object-oriented design principles
- Design patterns (Repository, Factory, Strategy)
- Concurrent programming
- Exception handling and logging

✅ **Database Engineering**
- PostgreSQL advanced features
- Query optimization
- Schema design and indexing
- Database migrations (Flyway)

✅ **Cloud Computing (AWS)**
- RDS database management
- S3 object storage
- AWS SDK integration
- Cloud-native architecture

✅ **DevOps & Testing**
- Unit and integration testing
- Test-driven development (TDD)
- Docker containerization
- CI/CD readiness

✅ **Unix/Linux**
- Shell scripting
- Data processing pipelines
- Command-line tools (awk, sed)

---

## 🎓 Learning Resources

This project was built using knowledge from:
- [PostgreSQL Documentation](https://www.postgresql.org/docs/)
- [AWS SDK for Java Developer Guide](https://docs.aws.amazon.com/sdk-for-java/)
- [Effective Java by Joshua Bloch](https://www.oreilly.com/library/view/effective-java/9780134686097/)
- [Kaggle Credit Card Fraud Detection Dataset](https://www.kaggle.com/mlg-ulb/creditcardfraud)

---

## 🗓️ Development Timeline

- **Week 1**: Infrastructure setup, data ingestion, normalization
- **Week 2**: Database optimization, fraud detection, AWS integration
- **Week 3**: Testing, documentation, performance tuning

**Total Development Time**: ~3 weeks (part-time)

---

## 🚧 Future Enhancements

- [ ] Add Scala modules for advanced data processing
- [ ] Implement machine learning models (Python integration)
- [ ] Create REST API with Spring Boot
- [ ] Build web dashboard for fraud monitoring
- [ ] Add Kafka for real-time streaming
- [ ] Implement GraphQL API
- [ ] Add Kubernetes deployment manifests

---

## 📝 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## 👤 Author

**Your Name**
- GitHub: [@YOUR_USERNAME](https://github.com/YOUR_USERNAME)
- LinkedIn: [Your LinkedIn](https://linkedin.com/in/YOUR_PROFILE)
- Email: your.email@example.com

---

## 🙏 Acknowledgments

- Verafin for inspiring this project through their job posting
- Kaggle community for fraud detection datasets
- Open-source community for the excellent tools and libraries

---

## 📞 Contact

For questions, feedback, or collaboration opportunities:
- Open an issue on GitHub
- Email: your.email@example.com
- LinkedIn: [Your Profile](https://linkedin.com/in/YOUR_PROFILE)

---

**⭐ If you find this project useful, please consider giving it a star on GitHub!**

---

### 🎯 How This Project Aligns with Job Requirements

| Job Requirement | Project Demonstration |
|----------------|----------------------|
| **Data normalization and ingestion in AWS** | ✅ Complete ETL pipeline with S3/RDS integration |
| **PostgreSQL expertise** | ✅ Advanced schema design, query optimization, migrations |
| **Java development** | ✅ 100% Java codebase with modern practices |
| **Scala (nice to have)** | 🔄 Can be added as extension module |
| **Unix utilities** | ✅ Shell scripts for data preprocessing |
| **Automated testing** | ✅ Comprehensive test suite with high coverage |
| **Cloud-based environments** | ✅ AWS deployment-ready architecture |
| **Financial technology** | ✅ Fraud detection in banking context |

---

**Built with ❤️ for enterprise-grade data engineering**
