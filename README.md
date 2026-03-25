# opencode-gpt-unlocked

OpenCode 会话优化工具箱，旨在解除 GPT 的限制并提升使用体验。

## 组件

- **OpenCode Patcher**: 一个 Python 脚本，通过修改 `opencode.db` 的方式实现：
    - 自动检测并移除 Assistant 的拒绝回答（Refusals）。
    - 清理 Reasoning（推理内容），防止推理链中断导致的模型异常。
- **Message Editor Plugin (WIP)**: 计划中的在线插件，支持在 UI 中直接编辑消息。

## 安装

### 1. 安装 Patcher (Python)

您可以直接通过 pip 安装：

```bash
git clone git@github.com:zhexulong/opencode-gpt-unlocked.git
cd opencode-gpt-unlocked
pip install -e .
```

安装后，可以直接在终端使用 `opencode-patcher` 命令。

### 2. 安装 Plugin (实时拦截)

将插件脚本添加至 OpenCode 配置文件 `~/.config/opencode/opencode.json` (Linux) 或 `%APPDATA%\opencode\opencode.json` (Windows) 中：

```json
{
  "plugin": [
    "/absolute/path/to/opencode-gpt-unlocked/refusal-patcher.ts"
  ]
}
```

> **Note**: 请将 `/absolute/path/to/` 替换为您克隆仓库后的实际绝对路径。

## 功能 (Patcher)

- 自动定位 OpenCode 数据库（默认从 XDG data 路径推断）
- 支持按最新会话、交互选择、日期、会话 ID 进行定位
- 替换最后一条 assistant 拒绝文本为肯定占位回复
- 删除当前会话中的 reasoning 分片
- 可清理 assistant error 字段
- 修改前自动创建 .bak 备份
- 支持 dry-run 预览

## 使用方法

```bash
# 安装后直接运行
opencode-patcher

# 交互选择会话
opencode-patcher --select

# 指定日期会话
opencode-patcher --date 2026-03-25

# 指定会话 ID
opencode-patcher --session-id <session_id>

# 预览模式
opencode-patcher --dry-run --show-content

# 执行后直接进入该会话
opencode-patcher --auto-resume
```

## 参数

- --select: 交互式选择会话
- --date YYYY-MM-DD: 选择指定日期最新会话
- --session-id ID: 选择指定会话
- --db-file PATH: 指定 OpenCode 数据库路径
- --data-dir PATH: 指定 OpenCode 数据目录（用于自动搜库）
- --include-archived: 选择时包含归档会话
- --auto-resume: 处理后执行 opencode --session <id>
- --no-backup: 跳过备份（不推荐）
- --dry-run: 只预览，不写入
- --show-content: 显示替换前后文本摘要
- -v, --verbose: 输出调试日志

## 默认数据库位置

若未指定 --db-file，会按以下规则查找：

1. 如果设置了 XDG_DATA_HOME，则使用 XDG_DATA_HOME/opencode
2. 否则使用 ~/.local/share/opencode
3. 在目录中匹配 opencode*.db，按最近修改时间选最新

## 安全说明

- 仅本地读写，不发送网络请求
- 默认会先备份数据库再修改
- 建议在重要会话前手动备份数据库文件

## 许可证

MIT
