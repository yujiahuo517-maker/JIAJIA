# 京麦商品品质标签批量维护 Skill

面向有京麦商品管理权限的同事，按 SPU 底表批量申请商品品质标签。

## 一句话安装

把下面这段发给 Codex：

> 请使用 skill-installer，从 https://github.com/yujiahuo517-maker/JIAJIA/tree/main/skills/jd-quality-label-maintenance 安装 jd-quality-label-maintenance Skill。安装后检查 Python 3、openpyxl、o2 与 webcli 的可用性；缺少依赖时先告诉我需要安装什么并按环境要求确认。不要执行任何真实商品提交。

安装成功后，在下一轮对话中使用该 Skill。

## 使用前准备

- 已安装 Codex，且可使用本地 Skill、Python 3 和浏览器自动化能力。
- 已安装并配置 o2 / webcli；先用 `o2 --help`、`o2 skill webcli` 检查。此仓库不附带这些工具，不应把安装 Skill 当作完成运行环境配置。
- 已登录本人京麦账号，具备目标店铺、商品与品质标签申请权限。不要发送密码或 Cookie。
- 准备 SPU 底表（CSV、TSV、XLSX 或 XLSM）和证明材料。旧版 XLS 请先转换；读取 Excel 需要 `python -m pip install openpyxl`。

## 开始使用

上传 SPU 底表和证明材料，并发送：

> 使用 jd-quality-label-maintenance，按我上传的 SPU 底表维护品质标签。标签由我在页面选择，证明材料用本次附件，有效期默认一年。请先展示 SKU 范围和批次预览，等我确认后再提交，并逐批回读审批单。

## 工作方式与边界

- 用户在页面自行筛选标签；默认有效期为执行日起一年。
- 自动展开 SKU，按三级类目分组，每批最多 100 SKU；材料当前最多 12 份，以实时页面限制为准。
- 首批页面验证后，才按实际表单和材料回执处理后续批次。
- 接口异常时回退页面；未知提交结果先对账，不盲目重复提交。
- 输出批次、SKU、审批单和失败或未处理项。申请提交成功不等于平台审核通过。
- 本包是助手执行说明和本地分批脚本，不是脱离助手与登录环境即可运行的一键提交程序。

## 本地分批脚本

```shell
python scripts/prepare_batches.py source.xlsx --output parsed.json
python scripts/prepare_batches.py source.xlsx --resolved resolved.json --output batches.json
```

`resolved.json` 为经过授权查询的 SKU 明细数组，每行包含 `spuId`、`skuId` 和三级类目 `categoryId`。脚本本身不登录、不联网、不提交申请。

已完成基础校验与 101 SKU 拆分为 100+1 的本地测试；不代表已在所有同事环境或全部标签场景中验证。接口说明可能随平台更新变化，执行时以当前页面核验为准。

本分享包不含业务底表、证明材料、登录态或执行账本。
