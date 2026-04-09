# Plugin Marketplace 快速上手教程

让你的 AI Agent（Claude Code、Cursor、Codex CLI 等）发现、安装和使用 Skills。

---

## 第一步：添加市场

```bash
/plugin marketplace add bondzhan/bond-marketplace
```

Agent 会自动克隆仓库并注册市场。

---

## 第二步：安装技能

```bash
/plugin install dev-browser@bondzhan/bond-marketplace
```

> `@` 前面是技能名，后面是市场名（格式：`技能名@用户名/仓库`）

---

## 第三步：重载插件

```bash
/reload-plugins
```

使新安装的技能生效。

---

## 第四步：使用技能

安装成功后，直接用自然语言描述你的需求，Agent 会自动调用对应的技能。

**示例：**
- "navigate to www.google.com"
- "take a screenshot of this page"
- "fill the login form with username and password"
- "extract all product names from this page"

---

## 查看已安装的技能

```bash
/skills
```

或在对话中直接说"列出所有已安装的技能"。

---

## 目录结构说明

```
skills/
├── automation/      # 自动化类（如 dev-browser）
├── tools/          # 工具类
└── workflow/       # 工作流类
```

每个技能目录下包含：
- `SKILL.md` — 技能说明和使用方法
- `.claude-plugin/plugin.json` — 技能元数据

---

## 贡献新技能

### 方式一：原生技能（直接维护在市场仓库）

1. Fork 本仓库
2. 在 `skills/<category>/<skill-name>/` 下创建技能
3. 更新 `sources.yaml` 添加技能条目
4. 更新 `AGENTS.md` 添加技能说明
5. 提交 PR

### 方式二：外部同步技能（从其他仓库同步）

在 `sources.yaml` 中添加外部技能源，GitHub Actions 会自动同步。

---

## 常用命令速查

| 命令 | 说明 |
|------|------|
| `/plugin marketplace add <url>` | 添加市场 |
| `/plugin install <skill>@<marketplace>` | 安装技能 |
| `/plugin list` | 列出已安装的技能 |
| `/reload-plugins` | 重载插件 |
| `/skills` | 查看技能列表 |

---

## 故障排除

**技能安装后不生效？**
```bash
/reload-plugins
```

**找不到技能？**
- 确认市场已添加：`/plugin marketplace list`
- 检查技能名是否正确（区分大小写）

**安装失败？**
- 检查网络连接
- 确认仓库地址正确
