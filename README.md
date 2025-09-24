# THSS 汇编与编译原理（2025）实验文档

![mkdocs](https://img.shields.io/badge/docs-mkdocs-blue.svg)
![material](https://img.shields.io/badge/theme-material-orange.svg)

本仓库为清华大学软件学院2025年秋季学期"汇编与编译原理"课程实验与作业文档仓库。

## 🛠️ 环境配置

### 基础环境要求

- Python 3.6+
- pip 包管理工具

### 安装依赖

```bash
pip install mkdocs mkdocs-material
```
## 🚀 快速使用

### 本地开发预览

```bash
mkdocs serve
```

访问 http://127.0.0.1:8000 查看实时更新的文档

### 简易部署

如果你想**简单地**在服务器上部署一个网站，可以使用下面的命令

```bash
nohup mkdocs serve --dev-addr=0.0.0.0:8000 > output.log 2>&1 &
```

## 🗂 项目结构

    mkdocs.yml    # 配置文件
    docs/         # 文档目录
        index.md  # 文档首页
        ...       # 其他Markdown文档和资源