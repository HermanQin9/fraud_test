# GitHub 连接指南

## 📝 步骤说明

按照以下步骤将本地项目连接到 GitHub 仓库。

---

## 1️⃣ 在 GitHub 上创建新仓库

### 方式 A: 通过 GitHub 网站创建

1. 登录 [GitHub](https://github.com)
2. 点击右上角的 **"+"** 按钮，选择 **"New repository"**
3. 填写仓库信息：
   - **Repository name**: `BankFraudTest` 或 `enterprise-fraud-detection`
   - **Description**: `Enterprise Bank Fraud Detection & Data Pipeline System - Java, PostgreSQL, AWS`
   - **Visibility**: 选择 **Public** (方便雇主查看)
   - ⚠️ **不要** 勾选 "Initialize this repository with a README" (因为我们已经有了)
   - ⚠️ **不要** 添加 .gitignore 或 License (我们已经创建了)
4. 点击 **"Create repository"**

### 方式 B: 通过 GitHub CLI 创建 (如果已安装)

```powershell
# 登录 GitHub CLI
gh auth login

# 创建仓库
gh repo create BankFraudTest --public --source=. --remote=origin --push
```

---

## 2️⃣ 连接本地仓库到 GitHub

### 在 PowerShell 中执行以下命令：

```powershell
# 确保在项目目录下
cd "d:\Jupyter notebook\Project\BankFraudTest"

# 添加所有文件到暂存区
git add .

# 创建第一次提交
git commit -m "Initial commit: Project structure and documentation"

# 添加 GitHub 远程仓库（替换 YOUR_USERNAME 为你的 GitHub 用户名）
git remote add origin https://github.com/YOUR_USERNAME/BankFraudTest.git

# 设置默认分支为 main
git branch -M main

# 推送到 GitHub
git push -u origin main
```

### ⚠️ 重要提示

- 将 `YOUR_USERNAME` 替换为你的 GitHub 用户名
- 如果你的仓库名不是 `BankFraudTest`，请修改仓库名称
- 如果使用 SSH 连接，URL 格式为：`git@github.com:YOUR_USERNAME/BankFraudTest.git`

---

## 3️⃣ 验证连接

```powershell
# 检查远程仓库配置
git remote -v

# 应该看到类似输出：
# origin  https://github.com/YOUR_USERNAME/BankFraudTest.git (fetch)
# origin  https://github.com/YOUR_USERNAME/BankFraudTest.git (push)
```

---

## 4️⃣ 后续提交流程

每次修改代码后，使用以下命令提交：

```powershell
# 查看修改的文件
git status

# 添加修改的文件
git add .
# 或添加特定文件
git add src/main/java/com/bankfraud/SomeFile.java

# 提交修改
git commit -m "描述你的修改内容"

# 推送到 GitHub
git push
```

---

## 5️⃣ 优化 GitHub 仓库展示

### 添加仓库主题 (Topics)

在 GitHub 仓库页面点击 "Add topics"，添加：
- `java`
- `postgresql`
- `aws`
- `fraud-detection`
- `data-engineering`
- `etl-pipeline`
- `data-migration`
- `enterprise-software`

### 设置仓库描述

在仓库设置中添加描述：
```
Enterprise-grade fraud detection system with data pipeline engineering. Demonstrates Java, PostgreSQL, AWS RDS/S3, ETL, and data normalization skills.
```

### 启用 GitHub Pages (可选)

如果未来想添加项目文档网站：
1. 进入 Settings → Pages
2. Source 选择 `main` 分支
3. Folder 选择 `/docs`

---

## 6️⃣ 创建专业的 Git 提交历史

### 提交信息最佳实践

使用清晰的提交信息格式：

```powershell
git commit -m "feat: Add data ingestion pipeline for CSV files"
git commit -m "fix: Resolve PostgreSQL connection pool timeout issue"
git commit -m "docs: Update README with AWS deployment instructions"
git commit -m "test: Add unit tests for TransactionNormalizer"
git commit -m "refactor: Optimize batch insert performance"
git commit -m "chore: Update dependencies and Maven configuration"
```

### 提交类型前缀：
- `feat:` - 新功能
- `fix:` - 修复 bug
- `docs:` - 文档更新
- `test:` - 添加测试
- `refactor:` - 代码重构
- `perf:` - 性能优化
- `chore:` - 构建/工具配置

---

## 7️⃣ 保护敏感信息

### ⚠️ 在推送前确保已添加到 .gitignore：

```gitignore
# 已在 .gitignore 中配置
*.properties (除了模板文件)
.env
aws-credentials.txt
*.pem
*.ppk
```

### 如果不小心提交了敏感信息：

```powershell
# 从历史中移除文件
git filter-branch --force --index-filter \
"git rm --cached --ignore-unmatch path/to/sensitive/file" \
--prune-empty --tag-name-filter cat -- --all

# 强制推送（谨慎使用）
git push origin --force --all
```

---

## 8️⃣ 设置 GitHub Actions CI/CD (可选)

创建 `.github/workflows/ci.yml`：

```yaml
name: Java CI with Maven

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  build:
    runs-on: ubuntu-latest
    
    steps:
    - uses: actions/checkout@v3
    - name: Set up JDK 17
      uses: actions/setup-java@v3
      with:
        java-version: '17'
        distribution: 'temurin'
    - name: Build with Maven
      run: mvn clean install
    - name: Run tests
      run: mvn test
```

---

## 9️⃣ 创建 LICENSE 文件 (可选)

```powershell
# MIT License (推荐用于展示项目)
echo "MIT License" > LICENSE
echo "" >> LICENSE
echo "Copyright (c) 2025 Your Name" >> LICENSE
# ... (完整 MIT License 文本)

git add LICENSE
git commit -m "docs: Add MIT License"
git push
```

---

## 🔟 在简历和求职信中引用

### 在简历中添加：

**项目经验**
- **Enterprise Fraud Detection System** | Java, PostgreSQL, AWS
  - 构建 ETL 数据管道，处理多源银行交易数据的导入和标准化
  - 使用 AWS RDS 和 S3 实现云原生数据存储架构
  - 实现批量数据处理，优化性能至 1000+ 记录/秒
  - 单元测试覆盖率达到 80%+，使用 JUnit 5 和 Testcontainers
  - **GitHub**: github.com/YOUR_USERNAME/BankFraudTest

### 在 Cover Letter 中提及：

> To demonstrate my ability to work with data migration and normalization in cloud environments, I developed an enterprise-grade fraud detection system. The project showcases my skills in Java development, PostgreSQL database optimization, and AWS integration (RDS, S3) - directly aligned with Verafin's technology stack. You can review the full implementation and documentation at: github.com/YOUR_USERNAME/BankFraudTest

---

## 📊 GitHub 统计和展示

### 添加 GitHub Stats 到 README (可选)

在 README 底部添加：

```markdown
## 📊 Repository Stats

![GitHub stars](https://img.shields.io/github/stars/YOUR_USERNAME/BankFraudTest?style=social)
![GitHub forks](https://img.shields.io/github/forks/YOUR_USERNAME/BankFraudTest?style=social)
![GitHub issues](https://img.shields.io/github/issues/YOUR_USERNAME/BankFraudTest)
![GitHub last commit](https://img.shields.io/github/last-commit/YOUR_USERNAME/BankFraudTest)
```

---

## ✅ 检查清单

完成以下项目后，你的 GitHub 仓库就准备好了：

- [ ] GitHub 仓库已创建
- [ ] 本地代码已推送到 GitHub
- [ ] README.md 正确显示
- [ ] .gitignore 正在工作（敏感文件未上传）
- [ ] 添加了仓库描述和主题
- [ ] 至少有 3-5 次有意义的提交
- [ ] LICENSE 文件已添加
- [ ] 在简历中添加了项目链接

---

## 🆘 常见问题

### Q: 推送时要求输入用户名和密码？

A: GitHub 已不再支持密码认证，需要使用 Personal Access Token：
1. 前往 GitHub Settings → Developer settings → Personal access tokens
2. 生成新 token，勾选 `repo` 权限
3. 使用 token 代替密码

### Q: 推送被拒绝 (rejected)?

A: 可能是远程仓库有更新，先拉取：
```powershell
git pull origin main --rebase
git push origin main
```

### Q: 如何更改仓库名称？

A: 在 GitHub 仓库 Settings → Repository name 修改后：
```powershell
git remote set-url origin https://github.com/YOUR_USERNAME/NEW_NAME.git
```

---

## 📞 需要帮助？

- [GitHub 官方文档](https://docs.github.com/)
- [Git 教程](https://git-scm.com/book/zh/v2)
- [GitHub 支持](https://support.github.com/)

---

**祝你顺利完成项目并找到理想工作！🚀**
