# 最美证件照 API SDK

这里提供的是最小可运行的 Python / TypeScript 接入示例，方便开发者快速验证接口、签名和结果结构。

如果你是第一次接入，建议顺序：

1. 先跑 `一键证件照`
2. 再跑 `智能抠图`
3. 最后串 `换背景色 / 排版 / 增强`

## API 端点

| 接口 | 说明 |
|------|------|
| `/api/v1/photo/id-photo` | 一键证件照 |
| `/api/v1/segment/portrait` | 智能抠图 |
| `/api/v1/segment/background` | 换背景色 |
| `/api/v1/photo/enhance` | 人脸增强 |
| `/api/v1/photo/layout` | 排版照 |
| `/api/v1/photo/edit` | 图片编辑 |

## 快速开始

### Python

```bash
pip install requests

export ZUIMEI_API_KEY="your_api_key"
export ZUIMEI_SECRET_KEY="your_secret_key"

python python_sdk.py
```

### TypeScript

```bash
npm install axios form-data

export ZUIMEI_API_KEY="your_api_key"
export ZUIMEI_SECRET_KEY="your_secret_key"

npx tsc typescript_sdk.ts && node typescript_sdk.js
```

## 使用示例

```python
from python_sdk import ZuimeiZjzClient

client = ZuimeiZjzClient()

# 一寸证件照（蓝底+美颜）
result = client.id_photo("photo.jpg", width=295, height=413, background_color="#438EDB", beautify=True)

# 智能抠图
result = client.segment_portrait("photo.jpg")

# 换背景色（多色）
result = client.segment_background("photo.jpg", background_color="#FFFFFF;#438EDB", quality="hd-pro")

# 人脸增强
result = client.enhance("blurry.jpg", fidelity=0.3, use_sr=True)

# 排版照
result = client.layout("id_photo.jpg", layout_type="6inch")

# 图片编辑
result = client.edit("photo.jpg", width=800, height=600)
```

## 建议验证顺序

- 一键证件照：确认尺寸映射和背景色逻辑
- 智能抠图：确认透明图输出质量
- 换背景色：确认纯色、渐变、多背景输出
- 排版照：确认打印规格和裁剪线
- 人脸增强：确认清晰度提升效果

## 返回结果关注点

接入时重点关注这些字段：

- `image_url`：处理后的主结果图
- `hd_transparent_url`：高清透明底图
- `images`：多背景色返回列表
- `photo_count`：排版照输出张数

如果你做的是工作流集成，通常只需要保存这些 URL 并在前端回显即可。
