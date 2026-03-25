# OpenCode Session Patcher

一个独立的本地 Python 工具，用于重写 OpenCode 的 SQLite 会话数据，清理拒绝回复并移除 reasoning 分片。

## 功能

- 自动定位 OpenCode 数据库（默认从 XDG data 路径推断）
- 支持按最新会话、交互选择、日期、会话 ID 进行定位
- 替换最后一条 assistant 拒绝文本为肯定占位回复
- 删除当前会话中的 reasoning 分片
- 可清理 assistant error 字段
- 修改前自动创建 .bak 备份
- 支持 dry-run 预览

## 快速开始

```bash
# 直接运行（自动定位 DB）
python opencode_patcher.py

# 交互选择会话
python opencode_patcher.py --select

# 指定日期会话
python opencode_patcher.py --date 2026-03-25

# 指定会话 ID
python opencode_patcher.py --session-id <session_id>

# 指定数据库文件
python opencode_patcher.py --db-file ~/.local/share/opencode/opencode.db

# 预览模式
python opencode_patcher.py --dry-run --show-content

# 执行后直接进入该会话
python opencode_patcher.py --auto-resume
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
