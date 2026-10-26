# 安装并连接 suva-codex画布工程插件

这是从 GitHub 公开 Release 下载的薄连接器，下载无需 GitHub 或 SUVA 登录。工作流规则、核心提示词与工程数据保留远端，不包含在公开包中。连接时才在右侧登录自己的 SUVA 网站账号；不修改其他插件。

一句话入口：

> 请按 https://raw.githubusercontent.com/yuyou-dev/suva-codex-canvas-plugin/v0.1.13/START_HERE.md 安装并连接「suva-codex画布工程插件」，登录和授权由我完成。

给 Codex 的安装步骤：

1. 确认用户要求安装，找到可用 Node.js 20.19+ 与 Codex CLI；缺失时说明需要安装官方运行时，不读取任何已有插件凭据。
2. 从本版本固定 GitHub Release 地址 `https://github.com/yuyou-dev/suva-codex-canvas-plugin/releases/download/v0.1.13/suva-jewelry-canvas-0.1.13.tar.gz` 下载公开安装包，无需 GitHub 或 SUVA 登录。按发布记录核对文件字节数必须为 `411343`，SHA256 必须为 `808eda20d222abaa966cb83d19e3e46262325b0ab8c4ba7e4e9651400d2838d1`，不一致即停止；不要把网页错误或登录页面当作安装包，不使用本地候选包冒充公开下载。发布说明固定入口为 `https://raw.githubusercontent.com/yuyou-dev/suva-codex-canvas-plugin/v0.1.13/START_HERE.md`；工作台入口为 `https://modelscope.cn/studios/yuyou80/jewelry_design_canvas?mode=full`。不要提取 Cookie、密码或令牌；网站保护下载仅为已有用户的备用渠道，不是默认安装前置条件。
3. 下载并校验后，检查 tar 文件列表无绝对路径、`..`、符号链接或硬链接，解压到用户主目录 `.suva-jewelry-canvas/packages/0.1.13`；已存在时先比较包内容，不能覆盖自定义文件。
4. 使用已确认 Node 绝对路径运行解压目录的 `installer/install.mjs`。安装器注册独立 MCP `suva_jewelry_canvas` 和技能 `suva-jewelry-canvas-workspace`，同名冲突会停止，不删除其他连接。记录返回的 node、cli 路径，不输出连接凭据。
5. 加载正式 MCP `suva_jewelry_canvas`，调用 `suva_jewelry_connect({})`，不传 consent。打开本次返回的带 sessionId 的工作台链接，并保持标签与插件运行。服务端支持 session 连接时，右侧已登录账号自动连接；未登录时由用户完成“登录并连接”，不另点允许连接。新连接默认仅允许创建工程及访问通过此连接创建的工程；历史工程需按任务明确指定，不自动扩大全部范围。已有有效授权按原范围恢复。浏览器本机网络许可仍由用户允许，不读取或复制任何凭据。核对 connected、accountMatch=true 和真实工程权限。
6. v0.1.13 在首次 `tools/list` 固定声明 5 个本地工具和 58 个远端工具，不依赖登录、discovery 完成或通知触发；声明不代表已登录、有权限或已生图。先核对本聊天真实工具，再调用 `list_projects`、`get_creation_capabilities`，按任务继续 `prepare_source_image`、`prepare_codex_creation`。执行仍检查远端登录与工程权限；按实际 schema 和错误处理。初始 discovery 未 loaded、notification 未 sent 不能作为反复重载的理由。若本聊天实际缺少工具，核对安装版本并一次重新加载新版或开新聊天；仍缺失则记录真实阻断，不能用独立 SDK/CLI 冒充本聊天可用。网站模式保持正式长驻 MCP 与工作台运行，独立 CLI 仅是用户明确选择旧平台模式的兼容入口。
7. 创作选项默认在 Codex 对话中一次展示多组问题、一次提交；保留用户已明确的选择，不自动打开网页任务选择弹窗。先读远端 `get_creation_capabilities` 的 `inputSchema/inputDefaults`；`resolvedDefaults` 与旧 `defaults` 是原配置说明，不能整包当作用户输入提交。1+1+1、精修、三视图、随手画、随手拍与二创使用相同的多组问题统一提交方式；按工作流展示所需原选项，等待用户提交，不得将预选默认当作确认。仅用户明确要求用默认或委托选择时可以跳过。按工具契约提交 parameterConfirmation 后再准备生图。二创在对话汇总具体分支，用户确认后取得 batchId，再逐项生图回传。
8. 先展示工程首页。用户明确选择或创建工程后，读取远端能力与输入规范，精修/随手拍/手绘先prepare_source_image，随后prepare_codex_creation（显式workflow）。逐字使用远端prompt调用Codex内置生图。普通单张调用 `suva_jewelry_upload_and_import`，传同一projectId、本轮结果绝对路径file、prepare返回的原recipe；批次/三视图用 `suva_jewelry_upload_attachment`（purpose=result、原creationId），再调用对应专用import。核验回执、历史和画布，不能以上传成功冒充保存成功。网站模式不使用独立CLI上传，不改原配方。缺少内置生图或可上传结果时如实说明。

网站授权独立保存在用户私有目录 `.suva-jewelry-canvas/site-account`，旧平台授权保留独立兼容，不进入聊天或发布包。上传默认是中性 `material`。不得扫描其他文件或修改旧 Ref Design 服务。

新版回传会提供统一 creationId、完整 inputs、outputs 和 receipt。核验历史关联与画布投放状态；结果已保存但投放 pending 时，不要重新生图，按远端提示恢复投放。MCP structuredContent 的 error.code 是业务错误依据，不能把上传成功当作回传完成。

升级到 v0.1.13 后关闭旧版本 MCP 进程并重新加载插件。新版本会协调多进程令牌刷新；不要同时保留旧版本进程继续运行。登录出错在原页面重试，密码无需发送到聊天。
