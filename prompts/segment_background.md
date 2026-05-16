# 换背景色

当用户需要更换照片背景颜色时：

**用户意图**：
- "把这张照片的背景换成蓝色"
- "我需要白底照片"
- "帮我改成红底"
- "换一个渐变背景"
- "生成多个背景色的照片"

**处理流程**：

1. 确认目标背景色：纯色/渐变/多色
2. 确认是否需要高清输出（quality: hd-pro）
3. 调用 API：`/api/v1/segment/background`

**API 参数**：

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| image | file | 是 | 原始图片文件 |
| background_color | string | 是 | 背景颜色 |
| quality | string | 否 | 输出质量：standard/hd/hd-pro |

**背景色格式**：

| 类型 | 格式 | 示例 |
|------|------|------|
| 纯色 | #RRGGBB | #FFFFFF（白）、#438EDB（蓝）、#FF0000（红） |
| 上下渐变 | #颜色,updown | #438EDB,updown（从蓝渐变到白） |
| 中心渐变 | #颜色,center | #438EDB,center（从蓝向四周渐变） |
| 多色输出 | #颜色1;#颜色2;#颜色3 | #FFFFFF;#438EDB;#FF0000（输出3张图） |

**常见背景色**：

| 用途 | 颜色代码 |
|------|----------|
| 白底 | #FFFFFF |
| 蓝底 | #438EDB |
| 红底 | #FF0000 |
| 浅蓝 | #67B7DC |
| 浅灰 | #F0F0F0 |

**示例代码**：

```python
# 纯色背景
result = client.segment_background("photo.jpg", background_color="#438EDB")

# 上下渐变背景
result = client.segment_background(
    "photo.jpg",
    background_color="#438EDB,updown"
)

# 中心渐变背景
result = client.segment_background(
    "photo.jpg",
    background_color="#438EDB,center"
)

# 多色输出（同时生成白底、蓝底、红底）
result = client.segment_background(
    "photo.jpg",
    background_color="#FFFFFF;#438EDB;#FF0000",
    quality="hd-pro"
)
```

**适用场景**：
- 证件照换底
- 简历照片
- 社交媒体头像
- 电商产品图

**注意事项**：
- 图片需包含清晰的人像轮廓
- 渐变方向支持：0deg（从下到上）、90deg（从左到右）、180deg（从上到下）、270deg（从右到左）
- 多色输出时，返回结果包含多张图片
