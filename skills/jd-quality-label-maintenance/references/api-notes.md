# 品质标签接口笔记

以下信息于 2026-09-17 从 `wares-jdm.jd.com/ware/quality-label` 当前生产前端只读探测得到。接口可能随页面版本变化；每次执行先通过页面网络请求校验，不要把名称当永久契约。

## 已发现接口

网关为 `https://sff.jd.com/api`；appId 从当前已登录页面的真实请求读取，不在分享包中固化。鉴权复用京麦登录 Cookie 和当前店铺业务头；不要在日志中输出 Cookie。

- `dsm.product.manage.product.label.ApplyClientService.getLabelApplyList`
- `dsm.product.manage.product.label.ApplyClientService.getLabelApplyInfo`
- `dsm.product.manage.product.label.ApplyClientService.getApplySkuList`
- `dsm.product.manage.product.label.ApplyClientService.getApplySkuInfo`
- `dsm.product.manage.product.label.ApplyClientService.getRenderConfig`
- `dsm.product.manage.product.label.ApplyClientService.saveApplyInfo`
- `dsm.product.manage.product.label.LabelClientService.queryLabelListBasic`
- `dsm.product.manage.product.label.LabelClientService.queryLabelListByCategory`
- `dsm.product.manage.product.label.LabelClientService.queryInfoByLabel`
- `dsm.product.manage.product.label.LabelClientService.hasLabelByCategory`
- `dsm.product.manage.product.label.LabelClientService.checkLabel`
- `dsm.product.manage.product.label.LabelAuthClientService.queryUploadFileType`
- `dsm.product.manage.product.label.LabelAuthClientService.queryTabs`
- `dsm.product.manage.product.label.ProductManageService.queryValidProductList`
- `dsm.product.manage.product.label.ProductManageService.querySkuList`

## 当前请求结构

商品列表：

```json
{"productListQueryReq":{"pageNum":1,"pageSize":100,"productIdList":["SPU_ID"],"skuIdList":null,"productState":11,"sortMap":{"created":"desc"}}}
```

SKU 展开：

```json
{"skuListQueryReq":{"productId":"SPU_ID","sortMap":{"created":"asc"}}}
```

提交：

```json
{"applySaveContextDTO":{"skuList":[{"spuId":"SPU_ID","skuId":"SKU_ID","categoryId":"三级类目ID"}],"contextJson":{},"categoryId":"三级类目ID","firstLabelId":"一级标签ID","labelId":"末级标签ID"}}
```

`contextJson` 由动态表单决定，包含上传材料和有效期字段。不要手工猜字段名；从首批真实页面提交或 `getLabelApplyInfo` 回读得到后原样复用。IP 商品模式可能不传 `categoryId`，普通品质标签应传当前批次唯一三级类目。

## 已确认前端约束

- 提交前按所选 SKU 总数校验，超过 100 时提示“提交sku上限为100个，请重新选择商品！”。
- 普通品质标签校验同一批商品三级类目一致；跨类目必须拆批。
- 上传控件当前显示最多 12 份证明材料。
- 前端提交后提示“提交成功，刷新后即可在列表展示申请记录”；仍需按审批单回读 SKU 数。

## 接口优先策略

1. 首批通过页面完成，避免猜测动态表单字段和上传回执。
2. 回读首批申请，冻结 `contextJson`、标签 ID、类目和实际 SKU。
3. 剩余批次调用 `saveApplyInfo`，每批最多 100 SKU；每次调用后立即回读。
4. 如出现非 200 业务码、重定向、未登录、字段校验变化或回读不一致，停止接口模式并转页面模式。
5. 网络超时或响应丢失视为未知写入；先按计划 SKU 查询审批单，不直接重试。
