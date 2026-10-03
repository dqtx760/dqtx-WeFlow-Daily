# dqtx-WeFlow-Daily

输入微信群名，让 AI 通过本机 WeFlow MCP 读取消息，生成固定模板的 HTML 群聊日报。

默认使用 **Asia/Shanghai 当天、完整版**。支持指定日期和简化版，保留模板的深浅色切换、热门词云、目录导航和二维码弹窗。

## 安装

1. 点击本仓库的 **Code → Download ZIP**，下载后完整解压。
2. 打开解压目录的 `scripts` 文件夹，右键 `Install.ps1`，选择 **使用 PowerShell 运行**。
3. 脚本会创建桌面 `dqtx-WeFlow-Daily` 文件夹，并安装到 `%USERPROFILE%\.agents\skills\dqtx-WeFlow-Daily`。
4. 重开 Codex，确保下面的 WeFlow MCP 配置已生效。

已有同名文件夹时脚本停止并保留原内容；请先自行重命名已有目录。也可以手动将整个目录复制到技能目录，最终应包含 `dqtx-WeFlow-Daily/SKILL.md`。

这是给 AI 客户端使用的技能包，生成日报需要 AI 执行技能以及可用的 WeFlow MCP。

## 连接 WeFlow MCP

先打开 WeFlow，连接微信数据库，在设置中启用 API 服务，并取得自己的 Access Token。

Codex 的 `config.toml` 示例（已有同名配置时修改对应项）：

```toml
[mcp_servers.weflow]
command = "npx"
args = ["-y", "weflow-mcp"]

[mcp_servers.weflow.env]
WEFLOW_BASE_URL = "http://127.0.0.1:5031"
WEFLOW_ACCESS_TOKEN = "填写你自己的本地令牌"
```

需要本机安装 Node.js/npm。其他支持 MCP 的客户端可参考 [WeFlow 官方配置](https://github.com/L-Chris/weflow-mcp)，并安装本技能到各自技能目录。配置完成后重启客户端。

每位使用者连接自己的 WeFlow 和微信数据。本仓库不包含 Access Token 或真实群聊天记录。

## 使用

```text
$dqtx-WeFlow-Daily AI交流群
```

```text
使用 dqtx-WeFlow-Daily，给 AI交流群生成昨天的 HTML 日报。
```

```text
使用 dqtx-WeFlow-Daily，AI交流群，2026-10-02，简化版。
```

同名群会先让你选择。读取中断或无法确认分页完整性时，日报会注明“部分记录”。没有消息会生成空日报；连接失败不生成虚构内容。

完整版包含词云、热点、资源、答疑、消息汇总、金句和数据看板。简化版只保留词云、最多三个热点、消息汇总及前三名话唠榜。

## 模板与校验

- `assets/daily-template.html`：生成日报的固定模板。
- `assets/reference-preview.html`：视觉参考，含模板占位符，不是真实群日报。
- `references/`：消息读取和内容生成规则。
- `scripts/check_report.py`：检查样式、脚本、二维码、锚点及残留占位符。

```powershell
python scripts/check_report.py "你的日报.html"
```

页尾的公众号、赞赏入口和二维码来自原始模板，当前版本原样保留；二维码显示需要联网。技能只读取指定群并生成文件，不自动发送日报。

## 验证状态

已通过技能结构、资源边界、安装脚本语法及五项模板校验。尚未完成真实 WeFlow 群数据的端到端测试与浏览器交互测试。校验脚本检查结构，统计和总结仍需基于真实消息核对。

本技能按 yao-meta-skill 方法封装，模板基于用户提供的修复版文件。
