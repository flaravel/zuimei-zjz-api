# 图片编辑

当用户需要调整图片尺寸、格式时：

**用户意图**：
- "帮我把图片尺寸改成800x600"
- "调整一下DPI"
- "转换成PNG格式"
- "裁剪图片尺寸"

**处理流程**：

1. 确认目标尺寸（可选）
2. 确认 DPI 设置
3. 确认输出格式
4. 调用 API：`/api/v1/photo/edit`

**API 参数**：

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| image | file | 是 | 原始图片文件 |
| width | int | 否 | 目标宽度（像素） |
| height | int | 否 | 目标高度（像素） |
| dpi | int | 否 | 分辨率，默认 300 |
| output_format | string | 否 | 输出格式：jpeg/png，默认 jpeg |

**常见尺寸**：

| 用途 | 宽 x 高 (px) |
|------|-------------|
| 一寸 | 295 x 413 |
| 二寸 | 413 x 579 |
| 护照 | 390 x 567 |
| 驾照 | 260 x 378 |
| 社交头像 | 800 x 800 |

**DPI 设置**：

| 用途 | DPI |
|------|-----|
| 屏幕显示 | 72 |
| 普通打印 | 150 |
| 高清打印 | 300 |
| 专业印刷 | 600 |

**示例代码**：

```python
# 调整尺寸
result = client.edit("photo.jpg", width=800, height=600)

# 调整 DPI（打印用途）
result = client.edit("photo.jpg", dpi=300)

# 转换格式
result = client.edit("photo.jpg", output_format="png")

# 组合：调整尺寸 + DPI + 格式
result = client.edit(
    "photo.jpg",
    width=295,
    height=413,
    dpi=300,
    output_format="jpeg"
)
```

**适用场景**：
- 证件照尺寸调整
- 打印前处理
- 格式转换
- 社交媒体适配

**注意事项**：
- 不指定宽高时保持原始尺寸
- 宽高可只指定一个，自动等比缩放
- DPI 影响打印质量
- PNG 格式支持透明背景
