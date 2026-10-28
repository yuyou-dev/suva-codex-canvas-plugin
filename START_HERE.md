# 安装并连接 suva-codex画布工程插件

> v0.1.17 云端版本 / 说明书 v1.15。前版已在当前环境完成无本机端口的原生生图、上传、保存和画布显示；本版新增上传安全重试及界面修复，其他电脑待复测。外部 Chrome 社区嵌入页空白仍可复现，用户手动刷新后也为空白，根因未确认；用户已决定发布当前模块并在其他电脑复测，不保证全部浏览器或网络均可用。

这是从 GitHub 公开 Release 下载的薄连接器，下载无需 GitHub 或 SUVA 登录。工作流规则、核心提示词与工程数据保留远端，不包含在公开包中。连接时才在右侧登录自己的 SUVA 网站账号。v0.1.17 默认使用魔搭云端通信，不开启本机 HTTP 监听或 127 端口，无需本机网络许可。仍需本地 stdio 薄适配器转发工具调用和上传本轮图片；不等于无需插件。保留固定独立 HTTPS 服务作为显式备用，不修改其他插件。此源文件为发布模板，发布时须替换版本与校验占位符，未替换的模板不得直接用于安装。

一句话入口：

> 请按 https://raw.githubusercontent.com/yuyou-dev/suva-codex-canvas-plugin/v0.1.17/START_HERE.md 安装并连接「suva-codex画布工程插件」，登录和授权由我完成。

给 Codex 的安装步骤：

1. 确认用户要求安装，找到可用 Node.js 20.19+ 与 Codex CLI；缺失时说明需要安装官方运行时，不读取任何已有插件凭据。
2. 从本版本固定 GitHub Release 地址 `https://github.com/yuyou-dev/suva-codex-canvas-plugin/releases/download/v0.1.17/suva-jewelry-canvas-0.1.17.tar.gz` 下载公开安装包，无需 GitHub 或 SUVA 登录。按发布记录核对文件字节数必须为 `438149`，SHA256 必须为 `4d26499469522d0aae656d7457691117977874568275255bde0ba7661562243e`，不一致即停止；不要把网页错误或登录页面当作安装包，不使用本地候选包冒充公开下载。发布说明固定入口为 `https://raw.githubusercontent.com/yuyou-dev/suva-codex-canvas-plugin/v0.1.17/START_HERE.md`；工作台入口为 `https://modelscope.cn/studios/yuyou80/jewelry_design_canvas?mode=full`。不要提取 Cookie、密码或令牌；网站保护下载仅为已有用户的备用渠道，不是默认安装前置条件。
3. 下载并校验后，检查 tar 文件列表无绝对路径、`..`、符号链接或硬链接，解压到用户主目录 `.suva-jewelry-canvas/packages/0.1.17`；已存在时先比较包内容，不能覆盖自定义文件。
4. 使用已确认 Node 绝对路径运行解压目录的 `installer/install.mjs`。安装器注册独立 MCP `suva_jewelry_canvas` 和技能 `suva-jewelry-canvas-workspace`，同名冲突会停止，不删除其他连接。记录返回的 node、cli 路径，不输出连接凭据。
5. 加载正式 MCP `suva_jewelry_canvas`，调用 `suva_jewelry_connect({})`，不传 consent，默认等同 `connectionTransport:'cloud'`。核对发布 profile 的固定 `cloudMcpUrl`；缺失或不可达即报告错误，不退回本机监听。打开本次返回的带 sessionId 的工作台链接，保持标签与插件运行。用户只在网站登录 SUVA，已登录则关联当前账号，无需魔搭账号或本机网络权限。平台部署令牌只存在运营者托管服务的私密运行环境，不向终端用户索取或在客户端读取。sessionID 不代替登录、PKCE、CSRF 或权限核验。新连接默认仅允许创建工程及访问本连接创建的工程，历史工程按明确需求另行请求；有效授权按原范围恢复。核对 connected、accountMatch=true 和真实工程权限，不能把“已登录”当作连接成功。需要备用时明确调用 `suva_jewelry_connect({connectionTransport:'https',restart:true})`，打开新链接切换固定加密备用；恢复云端用 `connectionTransport:'cloud',restart:true`。`local` 仅用于明确选择的旧版兼容，不自动降级，不扫描端口、不复制凭据、不循环重装。
6. v0.1.17 在首次 `tools/list` 固定声明 5 个本地工具和 58 个远端工具；58 是工具数量，不是历史工程数量。工具声明不依赖登录、discovery 完成或通知触发；声明不代表已登录、有权限或已生图。先核对本聊天真实工具，再调用 `list_projects`、`get_creation_capabilities`，按任务继续 `prepare_source_image`、`prepare_codex_creation`。执行仍检查远端登录与工程权限；按实际 schema 和错误处理。初始 discovery 未 loaded、notification 未 sent 不能作为反复重载的理由。若本聊天实际缺少工具，核对安装版本并一次重新加载新版或开新聊天；仍缺失则记录真实阻断，不能用独立 SDK/CLI 冒充本聊天可用。网站模式保持正式长驻 MCP 与工作台运行，独立 CLI 仅是用户明确选择旧平台模式的兼容入口。
7. 创作选项默认在 Codex 对话中一次展示多组问题、一次提交；保留用户已明确的选择，不自动打开网页任务选择弹窗。先读远端 `get_creation_capabilities` 的 `inputSchema/inputDefaults`；`resolvedDefaults` 与旧 `defaults` 是原配置说明，不能整包当作用户输入提交。1+1+1、精修、三视图、随手画、随手拍与二创使用相同的多组问题统一提交方式；按工作流展示所需原选项，等待用户提交，不得将预选默认当作确认。仅用户明确要求用默认或委托选择时可以跳过。按工具契约提交 parameterConfirmation 后再准备生图。二创在对话汇总具体分支，用户确认后取得 batchId，再逐项生图回传。
8. 先展示工程首页。用户明确选择或创建工程后，读取远端能力与输入规范，精修/随手拍/手绘先prepare_source_image，随后prepare_codex_creation（显式workflow）。逐字使用远端prompt调用Codex内置生图。普通单张调用 `suva_jewelry_upload_and_import`，传同一projectId、本轮结果绝对路径file、prepare返回的原recipe；批次/三视图用 `suva_jewelry_upload_attachment`（purpose=result、原creationId），再调用对应专用import。核验回执、历史和画布，不能以上传成功冒充保存成功。网站模式不使用独立CLI上传，不改原配方。缺少内置生图或可上传结果时如实说明。

网站授权独立保存在用户私有目录 `.suva-jewelry-canvas/site-account`，旧平台授权保留独立兼容，不进入聊天或发布包。上传默认是中性 `material`。不得扫描其他文件或修改旧 Ref Design 服务。

新版回传会提供统一 creationId、完整 inputs、outputs 和 receipt。核验历史关联与画布投放状态；结果已保存但投放 pending 时，不要重新生图，按远端提示恢复投放。MCP structuredContent 的 error.code 是业务错误依据，不能把上传成功当作回传完成。

完整连接链接是临时连接凭据，只在当前聊天右侧打开，不保存、分享或写入日志。独立 HTTPS 中转仅处理加密消息，不接收解密密钥；ModelScope 外层 `app_path` 会携带编码后的临时链接，因此不声称整个链接对所有服务器不可见。sessionID 仍须配合网站登录、PKCE、CSRF 和工程权限核验。

验收以实际结果为准：默认 cloud 须完成匿名 MCP 握手、SUVA 登录和账号匹配、受限工程权限、本聊天原生工具调用、宿主生图、上传回执、历史与画布保存及刷新恢复；同时确认插件未建立本机 HTTP 监听。显式 HTTPS 备用单独记录，不代表默认云端已通过。空闲保持、会话过期及重连也须实测。前版已验证当前环境原生生图及上传保存，空闲修复后连接超过 10 分钟；UI 自动同步竞态已修复部署，重新加载后图片和历史可见；外部 Chrome 嵌入页空白仍可复现，手动刷新也未恢复，根因未确认。用户决定发布当前模块后在其他电脑复测，不以当前环境成功保证其他电脑成功。测试 adapter 的隔离回归不能替代另一台电脑的真实宿主和完整创作验收。

升级到 v0.1.17 后关闭旧版本 MCP 进程并重新加载插件。新版本会协调多进程令牌刷新；不要同时保留旧版本进程继续运行。登录出错在原页面重试，密码无需发送到聊天。

故障须分三类诊断：聊天工具未加载、云端会话通信失败、账号或工程权限不匹配。分别核对实际工具、分阶段错误码或具体授权；不要统称安装失败。正常重连无需卸载，建立新 cloud 会话后先核对已有保存结果，避免重复生图。

会话已经结束或返回 HTTP 410 时，旧通道不能靠页面“重试”恢复。让 Codex 调用 `suva_jewelry_connect({connectionTransport:"cloud",restart:true})` 建立新会话并打开新链接；无需卸载，也不要重新生成已保存作品。此情况与社区嵌入页空白分别诊断。

## v0.1.17 更新核验

本版补齐能力发现、幂等准备与安全二进制上传的429有限恢复，保留阶段、HTTP状态、Retry-After及失败重试统计。普通业务写入不自动重放。上传失败带transfer.operationId时，在同一有效授权、同工程、同文件和用途下重试suva_jewelry_upload_attachment并复用operationId；新文件或新操作不可复用。换授权后先核对工程与回执，不承诺跨grant去重。

三视图优先原样使用prepare返回hostPrompt，包含原业务prompt与明确1:1整图约束。实际非方图也必须上传并调用import_threeview_result，服务端保留原图为画布待修正草稿；不裁图冒充合格。草稿保存后最多追加一次比例修正生成，仍不合格也保留，不停在聊天图片。详情遵循包内技能。

标题置顶及黑猫Logo保留，底部状态压成单行，过期或受阻等提示可关闭并由连接详情重新打开；关闭不清授权、不伪造连接成功。

正常更新无需先卸载。确认实际加载v0.1.17后建立新会话；当前聊天仍用旧工具时重新加载插件或在新聊天继续。先读取原工程核对已保存图片，再测试缺失上传和三视图生成保存，避免重复创建。第三方429不能保证永不发生，其他电脑完整验收仍需复测。
