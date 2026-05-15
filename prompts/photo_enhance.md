# 照片修复

当用户需要修复模糊照片时：

**用户意图**：
- "这张照片太模糊了，帮我修复"
- "能把这个旧照片变清晰吗"

**处理流程**：

1. 确认是否需要放大（超分辨率）
2. 确认保真度（0.0-1.0，越小增强越强）
3. 调用 API：`/api/v1/photo/enhance`

**参数说明**：

| 参数 | 说明 |
|------|------|
| fidelity | 保真度 0.0-1.0，越小增强越强 |
| use_sr | 是否使用超分辨率放大 |
| sr_scale | 放大倍数 2 或 4 |

**示例代码**：

```python
# 基础修复
result = client.enhance("blurry.jpg", fidelity=0.3)

# 修复 + 放大
result = client.enhance("blurry.jpg", fidelity=0.3, use_sr=True)
```
