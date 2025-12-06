# **AI Team Automation Prompts (AI 团队自动化提示词框架)**

这是一个**生产级**的 AI 辅助软件工程（AISE）框架。与普通的提示词库不同，本项目构建了一个完整的**虚拟开发团队**，涵盖从产品定义、架构设计、代码开发到质量保证的全流程。

本项目旨在通过标准化的 Prompt（提示词）、工作流模板和自动化脚本，结合 **Cursor**、**Gemini 1.5 Pro** 和 **Claude 3.5 Sonnet** 等现代 AI 模型，实现高质量、高一致性的软件交付。

## **🛠 默认技术栈约定 (Default Tech Stack)**

为了保证 AI 输出代码的一致性和可运行性，本框架预置了以下“黄金标准”。所有 Agent（智能体）均已针对此技术栈进行了微调：

| 领域 | 技术选型 | 关键库/工具 |  
| Backend | Python 3.10+ | FastAPI, SQLAlchemy (Async), Pydantic v2, Alembic |  
| Frontend | React 18+ (TS) | TypeScript, Vite, Tailwind CSS, Zustand/Context |  
| Testing | TDD/BDD | Pytest (后端), Jest \+ React Testing Library (前端) |  
| DevOps | CI/CD | GitHub Actions, Docker |  
| IDE | Cursor | .cursorrules 深度集成 |

## **📂 完整项目结构与文件清单**

以下列出了项目中所有的 Prompt 和工具文件，每个文件代表了 AI 团队中的一个原子能力。

.  
├── 01-system-prompts/                 \# \[核心\] AI 角色定义（身份卡）  
│   ├── supervisor-guidelines.md       \# 监督者：负责统筹上下文，防止前后端不一致  
│   ├── pm-agent.md                    \# 产品经理：负责需求澄清与用户故事拆解  
│   ├── architect-agent.md             \# 架构师：负责技术选型、DB设计与API定义  
│   ├── backend-developer.md           \# 后端工程师：FastAPI/Python 专家  
│   ├── frontend-developer.md          \# 前端工程师：React/TS 专家  
│   ├── qa-engineer.md                 \# 测试工程师：负责编写测试用例与 Bug 分析  
│   └── code-reviewer.md               \# 代码审查员：负责 CR 与安全性检查  
│  
├── 02-workflow-templates/             \# \[协议\] 团队协作的标准输入输出  
│   ├── project-kickoff.md             \# 项目启动模板（PM 使用）  
│   ├── task-breakdown.json            \# 任务拆解标准格式（连接 PM 与 Dev 的桥梁）  
│   ├── architecture-design.md         \# 架构设计文档模板  
│   ├── development-sprint.md          \# 迭代规划模板  
│   ├── code-review-checklist.md       \# 代码审查清单  
│   ├── project-completion.md          \# 项目交付验收单  
│   └── .github/                       \# GitHub 平台协作模板  
│       ├── ISSUE\_TEMPLATE/bug\_report.md  
│       └── PULL\_REQUEST\_TEMPLATE.md  
│  
├── 03-automation-tools/               \# \[驱动\] Python 脚本（自动化胶水层）  
│   ├── team-coordinator.py            \# 调度器：读取 JSON 任务并调用对应 Agent  
│   ├── prompt-manager.py              \# 提示词管理器：组合 System Prompt 与用户输入  
│   ├── progress-tracker.py            \# 进度追踪：更新任务状态  
│   ├── code-analyzer.py               \# 静态分析：检查代码是否符合 .cursorrules  
│   └── report-generator.py            \# 报告生成：生成日报周报  
│  
├── 04-examples/                       \# \[参考\] 标准化输出示例  
│   └── blog-system/                   \# 示例：博客系统全套文档与代码脚手架  
│  
├── 05-best-practices/                 \# \[指南\] AI 协作方法论  
│   ├── prompt-engineering.md          \# 提示词工程技巧  
│   ├── ai-team-management.md          \# 如何管理 AI 员工  
│   ├── code-quality.md                \# 代码质量标准  
│   └── documentation-guide.md         \# 文档编写规范  
│  
├── 06-cursor-rules/                   \# \[IDE\] 编辑器集成  
│   ├── .cursorrules                   \# Cursor 核心规则（自动加载）  
│   └── cursor-setup-guide.md          \# 配置指南  
│  
└── 07-claude-integration/             \# \[模型\] Claude 深度集成  
    ├── claude-project-setup.md        \# Claude Project 设置指南  
    └── claude-workspace-guide.md      \# 知识库管理指南

## **🤖 如何启动多 Agent 协同 (Multi-Agent Orchestration)**

本框架支持两种模式来运行多个 Agent：**手动协同模式**（适合复杂逻辑探索）和**脚本自动化模式**（适合批量任务）。

### **模式一：手动协同 (The "Chat Tabs" Strategy)**

在 Claude、ChatGPT 或 Gemini 中打开多个对话窗口（Chat Windows），每个窗口扮演一个角色。

1. **窗口 A (产品经理)**:  
   * **Prompt**: 发送 01-system-prompts/pm-agent.md 内容。  
   * **任务**: 输入 "设计一个电商购物车功能"，产出 task-breakdown.json。  
2. **窗口 B (架构师)**:  
   * **Prompt**: 发送 01-system-prompts/architect-agent.md 内容。  
   * **输入**: 粘贴窗口 A 的输出。  
   * **产出**: 数据库 Schema 和 API 接口定义。  
3. **窗口 C (监督者/Supervisor)**:  
   * **Prompt**: 发送 01-system-prompts/supervisor-guidelines.md。  
   * **任务**: 将窗口 A 的需求和窗口 B 的设计发给它，让它生成一份 "开发规范摘要"。  
4. **IDE (开发者)**:  
   * 在 Cursor 中，基于窗口 C 的规范摘要，开始编码。

### **模式二：自动化脚本驱动 (The "Auto-Pilot" Strategy)**

利用 03-automation-tools/team-coordinator.py 实现半自动流转。

* **原理**: 脚本读取 task-breakdown.json，根据任务类型 (type: backend/frontend) 自动拼接对应的 System Prompt，并调用 LLM API。  
* **命令示例**:  
  \# 自动领取任务并生成代码建议  
  python 03-automation-tools/team-coordinator.py \--task-file task-breakdown.json \--role backend

## **🧠 各平台模型使用指南**

不同的 AI 模型有不同的长处，建议组合使用：

### **1\. Claude 3.5 Sonnet (推荐用于：架构、复杂逻辑)**

* **最佳实践**: 使用 **Claude Projects** 功能。  
* **设置**:  
  1. 创建一个 Project 命名为 "AI Team"。  
  2. 将 01-system-prompts 和 02-workflow-templates 文件夹下的所有文件上传到 **Project Knowledge**。  
  3. **使用**: 在对话时，直接 @ 对应的文件（如 @backend-developer.md），Claude 会自动切换人格。

### **2\. Gemini 1.5 Pro (推荐用于：超长文档分析、代码审查)**

* **最佳实践**: 利用其 1M+ Token 的上下文窗口。  
* **场景**: 将整个代码库打包（使用 git dump 或类似工具），扔给 Gemini，加载 qa-engineer.md，让它进行全量代码审查。

### **3\. Cursor / Codex (推荐用于：实际编码)**

* **最佳实践**: 依赖 .cursorrules。  
* **自动化**: 只要将 06-cursor-rules/.cursorrules 放入项目根目录，Cursor 的 Cmd+K 和 Composer 功能就会自动遵循其中的技术栈约定（FastAPI/React），无需每次重复提示。

## **🔄 Sub-Agent 工作流示例**

以下是一个**让 Agent 自动工作**的完整流程演示：

**目标**: 开发 "用户登录 API"。

1. **Step 1 (PM \-\> Data)**:  
   * 用户对 **PM Agent** 说："需要登录功能"。  
   * **PM Agent** 输出结构化 JSON：  
     { "id": "TASK-001", "role": "backend", "desc": "Implement POST /login with JWT" }

2. **Step 2 (Coordinator \-\> Dispatch)**:  
   * 运行 team-coordinator.py。脚本检测到 role: backend。  
   * 脚本自动读取 01-system-prompts/backend-developer.md 作为系统提示词。  
   * 脚本将 TASK-001 的描述作为用户提示词。  
3. **Step 3 (Agent \-\> Code)**:  
   * **Backend Agent** 接收请求，生成 Python 代码。  
   * （可选）脚本将代码保存到 src/auth/router.py。  
4. **Step 4 (Supervisor \-\> QA)**:  
   * 用户调用 **QA Agent**。  
   * QA Agent 读取生成的代码，根据 qa-engineer.md 规则，自动生成 tests/test\_auth.py。

## **🤝 贡献指南 (Contributing)**

我们非常欢迎社区贡献！本项目遵循以下开发流程：

1. **Fork 本仓库**  
2. **创建功能分支** (git checkout \-b feat/add-amazing-feature)  
3. **提交更改** (git commit \-m 'Add some amazing feature')  
4. **推送到分支** (git push origin feat/add-amazing-feature)  
5. **提交 Pull Request**

## **📄 License**

This project is licensed under the MIT License \- see the [LICENSE](https://www.google.com/search?q=LICENSE) file for details.
