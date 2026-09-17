# JD Pictured Review Automation

京东京喜带图评价便携 Skill 安装包。

## 下载与校验

- 文件：`jd-pictured-review-automation-portable-20260916.zip`
- SHA-256：`9564D482F6205BFB8273ED9628F6D56B49A5BB6C84A77C9819260D8CC53CD889`

## 安装

1. 下载并完整解压 ZIP。
2. 双击解压目录中的 `安装.cmd`。
3. 重启 Codex。
4. 双击 `首次登录.cmd` 完成 ERP 登录。
5. 在 Codex 中使用 `$jd-pictured-review-automation`。

安装包自带 Windows x64 便携 Python、离线依赖、带图评价核心、轻量 MCP 服务和本地 CLI 兜底。无需预装其他 Skill、Blacklight、MCP 或系统 Python。

当前版本已加强真实评价图文校验：拦截真伪/品牌质疑、跨品牌评价、非京东晒单图、同条及同批重复图片，并扩大历史图片判重范围。

当前版本新增 EasyBI 未完成明细查询：未指定 SKU/SPU 时，先按销售员 ERP 查询 T-1（无数据回退 T-2）的未完成 SPU，再把该清单作为 OSW 硬范围；EasyBI 查询失败时停止，不会误扫全量待办。

模型权限不是必需项：有 `llm-gw` 权限时优先 AI 生图；没有权限时自动调用包内真实评价图文采集器，不会停在“请申请模型权限”。

ZIP 不包含账号、Cookie、密钥、日志或历史业务数据。仅限获得京东内部系统授权的人员使用。
