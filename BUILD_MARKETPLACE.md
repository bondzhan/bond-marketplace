# 如何搭建自己的 Plugin Marketplace

本教程指导你创建一个类似 n-skills 的 Plugin Marketplace，让 AI Agent 能够发现、安装和使用你的技能。

---

## 目标

搭建一个 Git 仓库结构的 Marketplace，支持：
- `/plugin marketplace add` 添加市场
- `/plugin install skill@marketplace` 安装技能
- 开放贡献模式

---

## 第一步：初始化仓库

### 1.1 创建 Git 仓库

在 GitHub 上创建新仓库，克隆到本地：

```bash
git clone https://github.com/yourusername/your-marketplace.git
cd your-marketplace
```

### 1.2 创建基础文件

**package.json**
```json
{
  "name": "your-marketplace",
  "version": "1.0.0",
  "description": "Curated plugin marketplace for AI agents",
  "scripts": {
    "sync": "bash scripts/sync-external-skills.sh"
  },
  "keywords": ["plugins", "skills", "marketplace", "claude-code", "agents"],
  "license": "Apache-2.0"
}
```

**README.md**
```markdown
# Your Marketplace Name

Curated plugin marketplace for AI agents — works with Claude Code, Codex, OpenCode, Gemini CLI, and more.

## Quick Start

```bash
/plugin marketplace add username/your-marketplace
/plugin install your-skill@username/your-marketplace
```

## Available Skills

See [AGENTS.md](AGENTS.md) for the full list.
```

**LICENSE** (Apache-2.0)

---

## 第二步：创建核心元数据文件

### 2.1 sources.yaml — 技能清单

```yaml
# Marketplace Sources
# ===================
# 定义市场中所有技能的元数据

version: 1

skills: []
```

### 2.2 AGENTS.md — Agent 发现入口

```markdown
# Available Skills

No skills available yet. See [CONTRIBUTING.md](CONTRIBUTING.md) to add one.
```

### 2.3 CLAUDE.md — 维护指南

```markdown
# Marketplace Maintenance

## Adding a Native Skill

1. 创建技能目录：`skills/<category>/<skill-name>/`
2. 在 `sources.yaml` 添加条目
3. 在 `AGENTS.md` 添加说明
4. 提交 PR

## Syncing External Skills

通过 `.github/workflows/sync.yaml` 自动同步外部技能。
```

### 2.4 CONTRIBUTING.md — 贡献指南

```markdown
# Contributing

## Adding a Skill

1. Fork 本仓库
2. 在 `skills/<category>/<skill-name>/` 下添加技能
3. 更新 `sources.yaml` 和 `AGENTS.md`
4. 提交 PR

## 技能分类

- `automation` — 浏览器/应用自动化
- `tools` — 开发者工具
- `workflow` — 工作流编排
- `data` — 数据处理
- `documentation` — 文档生成
```

---

## 第三步：创建目录结构

```bash
mkdir -p skills/automation skills/tools skills/workflow scripts
mkdir -p .github/workflows
```

---

## 第四步：创建 Agent 发现清单

### 4.1 根目录 .claude-plugin/marketplace.json

```bash
mkdir -p .claude-plugin
```

```json
{
  "name": "your-marketplace",
  "description": "Your marketplace description",
  "owner": {
    "name": "Your Name",
    "email": "you@example.com"
  },
  "plugins": [],
  "metadata": {
    "version": "1.0.0",
    "generated_at": "2026-04-09T00:00:00.000Z",
    "skill_count": 0
  }
}
```

### 4.2 每个技能的 .claude-plugin/plugin.json

每个技能目录下：

```bash
mkdir -p skills/automation/your-skill/.claude-plugin
```

```json
{
  "name": "your-skill",
  "description": "Skill description for AI agents",
  "version": "1.0.0",
  "author": {
    "name": "Author Name",
    "url": "https://github.com/author"
  },
  "repository": "https://github.com/author/repo",
  "license": "MIT",
  "homepage": "https://github.com/author/repo",
  "keywords": ["keyword1", "keyword2"]
}
```

---

## 第五步：创建技能内容

### 5.1 SKILL.md — 技能定义

```markdown
# your-skill

技能描述。说明这个技能做什么，什么时候使用。

## Usage

使用说明。

## Triggers

- "trigger phrase 1"
- "trigger phrase 2"
```

---

## 第六步：添加示例技能

参考 `dev-browser` 添加一个示例技能：

```
skills/
└── automation/
    └── your-skill/
        ├── SKILL.md
        └── .claude-plugin/
            └── plugin.json
```

同时更新 `sources.yaml` 和 `AGENTS.md`。

---

## 第七步：创建 GitHub Actions 自动同步

**.github/workflows/sync.yaml**

```yaml
name: Sync External Skills

on:
  schedule:
    - cron: "0 0 * * *"  # 每天午夜
  workflow_dispatch:  # 手动触发

jobs:
  sync:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Sync external skills
        run: |
          echo "Reading sources.yaml..."
          # 在此添加同步逻辑

      - name: Configure git
        run: |
          git config user.name "github-actions[bot]"
          git config user.email "github-actions[bot]@users.noreply.github.com"

      - name: Commit if changed
        run: |
          if git diff --quiet; then
            echo "No changes."
          else
            git add -A
            git commit -m "chore: sync external skills"
            git push
          fi
```

---

## 第八步：提交并推送

```bash
git add .
git commit -m "feat: initial marketplace setup"
git push -u origin main
```

---

## 验证

在 Claude Code 中测试：

```bash
/plugin marketplace add yourusername/your-marketplace
/plugin install your-skill@yourusername/your-marketplace
/reload-plugins
# 使用技能
```

---

## 目录结构最终形态

```
your-marketplace/
├── .claude-plugin/
│   └── marketplace.json      # Agent 发现清单
├── .github/
│   └── workflows/
│       └── sync.yaml         # 自动同步
├── skills/
│   ├── automation/
│   │   └── your-skill/
│   │       ├── SKILL.md
│   │       └── .claude-plugin/
│   │           └── plugin.json
│   ├── tools/
│   └── workflow/
├── scripts/
│   └── sync-external-skills.sh
├── sources.yaml              # 技能清单
├── AGENTS.md                 # Agent 入口
├── CLAUDE.md                 # 维护指南
├── CONTRIBUTING.md           # 贡献指南
├── README.md
├── LICENSE
└── package.json
```

---

## 进阶功能

### 外部技能同步

在 `sources.yaml` 中定义外部技能源：

```yaml
skills:
  - name: external-skill
    source:
      repo: author/external-repo
      path: skills/external-skill
      ref: main
    target:
      category: automation
      path: skills/automation/external-skill
```

### 版本管理

为技能添加 `version` 字段，支持版本号管理。

### 分类扩展

根据需要添加新分类（如 `data`、`documentation` 等）。
