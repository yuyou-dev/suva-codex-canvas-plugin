# 安装并连接 suva-codex画布工程插件 v0.1.9

公开固定版本 v0.1.9。安装包和本说明可匿名下载；网站登录与授权由用户完成。

## 一句话开始

> 请按 https://raw.githubusercontent.com/yuyou-dev/suva-codex-canvas-plugin/v0.1.9/START_HERE.md 安装并连接「suva-codex画布工程插件」，登录和授权由我完成。

这句话只请求安装和连接，不默认授予全部现有/未来工程或创建工程权限。有效授权优先恢复；需要新授权或扩大范围时，先由当前用户在对话明确选择。网站账号、首次登录与必要浏览器权限由用户操作，无需登录 GitHub。

## 给 Codex 的执行步骤

### 1. 匿名下载固定附件

核对 Node.js >=20.19.0 和可用的 Codex CLI。缺少运行时使用官方渠道，不要求安装或登录 `gh`。在新建的空临时目录，通过普通 HTTPS GET 自动下载以下五个真实附件，不用用户手动点击：

| 文件 | 固定匿名下载地址 |
| --- | --- |
| 分发说明 | `https://github.com/yuyou-dev/suva-codex-canvas-plugin/releases/download/v0.1.9/README.md` |
| 安装说明 | `https://github.com/yuyou-dev/suva-codex-canvas-plugin/releases/download/v0.1.9/START_HERE.md` |
| 发布清单 | `https://github.com/yuyou-dev/suva-codex-canvas-plugin/releases/download/v0.1.9/release.json` |
| 校验清单 | `https://github.com/yuyou-dev/suva-codex-canvas-plugin/releases/download/v0.1.9/SHA256SUMS` |
| 正式归档 | `https://github.com/yuyou-dev/suva-codex-canvas-plugin/releases/download/v0.1.9/suva-jewelry-canvas-0.1.9.tar.gz` |

用户入口的 raw.githubusercontent.com 固定标签说明应与同版本 Release 附件 START_HERE.md 逐字一致。不要用 `latest`、GitHub 自动生成的 Source code 压缩包或旧 ZIP 代替。

下载不读取钥匙串、Git 凭据或连接器令牌，不发送 Authorization/Cookie，不执行 GitHub 设备授权。curl 首参数使用 `-q` 避免隐藏配置；Node fetch 不附加凭据。仅允许 HTTPS，重定向最多 5 次，只接受 `github.com`、`raw.githubusercontent.com`、`release-assets.githubusercontent.com`、`objects.githubusercontent.com`、`github-releases.githubusercontent.com` 的发布链，不跟随登录页或其他来源。

### 2. 校验后才能解压和执行

只接受最终 HTTP 200。归档 MIME 必须是 `application/gzip`、`application/x-gzip` 或 `application/octet-stream`；JSON 接受 `application/json`、`text/plain` 或 `application/octet-stream`；说明和校验文本接受 `text/plain`、`text/markdown` 或 `application/octet-stream`。忽略 MIME 的 charset 参数；缺失/未知 MIME、HTML、登录页、错误页都拒绝。另检查实际文件内容：gzip 标识、可解析 JSON、合法校验清单，不因扩展名或 HTTP 成功就继续。

固定安装包必须为 `suva-jewelry-canvas-0.1.9.tar.gz`，版本 `0.1.9`，长度 `369159` 字节，实际 SHA256：

```text
7ea871efb1e61c7cc8701f00281db767d8f0d04143b44d2b914341637c624495
```

解析 release.json，核对版本、文件名、长度、固定地址、最低 Node 版本和上述固定哈希。发布状态应为 `published`，其他状态不得安装。SHA256SUMS 不允许重复文件名、路径或非法行，必须覆盖 README.md、START_HERE.md、release.json 与正式归档；逐个计算本地文件真实 SHA256。任意不一致先停止，不解压、不执行、不转向 GitHub 登录。

哈希清单不是独立数字签名，不宣称已经验证发布者签名。固定版本发布后不静默替换字节；改包必须使用新版本与新哈希。

### 3. 规范归档与受管安装

先检查 tar 有效条目。仅允许以下普通文件与目录，拒绝其他类型或路径：

```text
.codex-plugin/
.codex-plugin/plugin.json
README.md
package.json
profile.json
skills/
skills/design-workspace/
skills/design-workspace/SKILL.md
installer/
installer/install.mjs
installer/install-core.mjs
dist/
dist/start-mcp.mjs
dist/cli.mjs
dist/THIRD-PARTY-NOTICES.txt
```

v0.1.9 使用规范 USTAR，uid/gid=0、uname/gname 为空、mtime=0；无 AppleDouble、PAX、扩展属性或原属主信息。逐项拒绝绝对路径、盘符/反斜杠路径、`.`/`..`、重复或规范化冲突、符号链接、硬链接、设备、FIFO、未知类型、PAX 路径重写及异常展开大小。必须在解压前检查；解压不继承拥有者或特殊权限。

先解压到新的临时目录，检查 package.json、profile.json、plugin.json 均为 0.1.9。正式安装到当前用户 `.suva-jewelry-canvas/packages/0.1.9`。同版本目录存在时先比较，发现不同内容或用户自定义文件就停止，不清空或强行覆盖。目标及父路径不得为指向别处的符号链接。

使用确认过的 Node 绝对路径执行包内 `installer/install.mjs`。无需 npm install，不使用 curl 管道直接执行远程脚本。安装器只注册 `suva_jewelry_canvas` MCP 和 `suva-jewelry-canvas-workspace` 技能；同名非受管配置或自定义技能冲突时保留原配置并报告，不改其他插件。

保留已有网站登录、授权及服务端工程、作品和历史。升级后只关闭本插件旧 MCP 连接进程并在 Codex 重新加载；不要结束其他插件。当前对话未加载正式 MCP 时，明确要求重载，不用独立 CLI 冒充网站通道。

### 4. 连接网站并核对权限

正式 MCP 加载后调用 `suva_jewelry_connect`，默认网站模式、不传 consent，优先恢复已有有效授权。右侧打开工具返回的工作台链接，保持浏览器标签与同一长驻 MCP 在线。首次 SUVA 网站登录、身份验证和浏览器本机网络许可由用户完成；魔搭平台账号无需退出或重新登录。

仅当当前用户在本次对话明确允许全部现有/未来工程、修改和创建工程时，才传 `consent:"all-projects-create"`。普通连接、安装说明和其他用户的授权不是扩权同意。先列工程，让用户选择或按明确需求创建，不擅自建立示例工程。

核对 `suva_jewelry_connection_status` 的 connected、authorizationSummary 与工程数量并展示实际范围。当前版本仍不直接返回 grant 用户身份，不能声称自动比较网页账号已实现。先确认网页登录账号并核对可见工程；身份或归属不确定时停止写入、重新核对网站授权，不打开其他账号旧工程试探。

### 5. 能力与实际回传验收

读取远端真实工具 schema、能力目录和原默认值，核对设计、图片上传、结果导入能力。需要选择参数时，在 Codex 对话一次展示多组问题统一提交，不默认弹网页表单。核心提示词与默认值保留远端，原 prompt 逐字交给 Codex 内置生图，不改写、不替换生成模型或原创作流程。

只有用户指定测试工程和素材后才实际写入验收。精修、随手拍、手绘先做原预处理；普通单张用同一个长驻 MCP 的 `suva_jewelry_upload_and_import`，传同一工程、实际结果文件和原 recipe。批次、三视图、试戴中间图保留专用上传/导入。网站模式不以独立 CLI 上传代替。

核对 SHA256、字节数、projectId、creationId、receipt、history、来源、画布显示及刷新恢复。生成成功但保存失败时保留原文件和配方，只重试保存，不自动重复生图。安装成功、连接成功、工具可用和实际端到端通过分别说明，未执行的步骤标未测试；产品形状和宝石保真单独验收。

网站入口仍为 `https://modelscope.cn/studios/yuyou80/jewelry_design_canvas?mode=full`。此公开仓库仅分发薄连接器，JavaScript 可读；原源码仓库保持私有，用户工程和图片不会随包公开。停止通道不等于撤销授权，撤销请使用网站授权管理入口。
