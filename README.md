# 最美证件照 API Skill

把“证件照制作”整理成一个 AI 可直接调用的标准化工作流，而不只是零散的图片接口。

它适合这几类高频需求：

- 生成一寸、二寸、护照、结婚照等标准证件照
- 把任意人像照片抠成透明底图
- 把白底、红底、蓝底互相切换，或生成渐变背景
- 修复模糊人脸、提升证件照清晰度
- 生成 5 寸、6 寸、A4 排版照用于打印

**官网**：https://zuimei.huipai.vip  
**API 服务**：https://idphoto.huipai.vip

## 为什么这个 Skill 值得装

- 🎯 **场景完整**：覆盖抠图、换底、增强、排版、一键证件照全链路
- 🧠 **适合理解自然语言**：支持把“帮我做一张一寸蓝底证件照”自动映射成正确参数
- 🔌 **可直接接入业务**：附带 OpenAPI、Python/TypeScript 示例和 Prompt 模板
- 🚀 **安装后可体验**：Skill 内置演示凭据，便于快速试用和二次接入

## 功能特性

- 🎯 **一键证件照**：完整证件照制作流程，支持多种规格、主体模式和背景色
- ✂️ **智能抠图**：生成透明背景 PNG，可继续接入换底、排版等流程
- 🎨 **换背景色**：支持纯色、渐变、多背景色输出
- ✨ **人脸增强**：修复模糊、提升清晰度，可选超分辨率
- 📄 **排版照**：生成 6 寸、5 寸、A4 等打印排版结果
- 🔧 **图片编辑**：支持尺寸调整、格式转换、DPI 设置

## API 端点

| 接口 | 端点 | 说明 |
|------|------|------|
| 一键证件照 | `/api/v1/photo/id-photo` | 完整证件照制作流程 |
| 智能抠图 | `/api/v1/segment/portrait` | 人像抠图，生成透明 PNG |
| 换背景色 | `/api/v1/segment/background` | 替换背景颜色，支持渐变和多色 |
| 人脸增强 | `/api/v1/photo/enhance` | 修复模糊、提升清晰度 |
| 排版照 | `/api/v1/photo/layout` | 生成打印排版照 |
| 图片编辑 | `/api/v1/photo/edit` | 尺寸调整、格式转换 |

## 常见使用方式

用户只需要表达意图，例如：

- “帮我生成一张一寸蓝底证件照”
- “把这张照片抠成透明底”
- “把白底换成红底”
- “帮我做一版 6 寸排版照”
- “把这张模糊人脸增强一下”

Skill 会自动进入对应接口，并补全常见规格和参数。

## 安装

### 从 ClawHub 安装

```bash
clawhub install zuimei-zjz-api
```

### 从 GitHub 安装

```bash
clawhub install --git https://github.com/flaravel/zuimei-zjz-api.git
```

## 快速开始

### 环境变量

```bash
export ZUIMEI_API_KEY="your_api_key"
export ZUIMEI_SECRET_KEY="your_secret_key"
```

### Python 示例

```python
from python_sdk import ZuimeiZjzClient

client = ZuimeiZjzClient()

# 一寸证件照（蓝底，默认不开启美颜）
result = client.id_photo("photo.jpg", width=295, height=413,
                         background_color="#438EDB")

# 智能抠图
result = client.segment_portrait("photo.jpg")

# 换背景色
result = client.segment_background("photo.jpg", background_color="#FFFFFF")

# 人脸增强
result = client.enhance("blurry.jpg", fidelity=0.3, use_sr=True)

# 排版照
result = client.layout("id_photo.jpg", layout_type="6inch")

# 图片编辑
result = client.edit("photo.jpg", width=800, height=600)
```

## 返回结果

不同接口会返回处理结果地址，例如：

- `image_url`：最终结果图地址
- `hd_transparent_url`：高清透明底图地址（证件照接口可选）
- `images[]`：多背景色输出列表
- `photo_count`：排版照张数

调用侧只需要关心成功状态和结果 URL，即可快速集成到站点、小程序或工作流中。

## 文档

- [API.md](./API.md) - API 详细文档
- [openapi.yaml](./openapi.yaml) - OpenAPI 3.0 规范
- [examples/](./examples/) - Python/TypeScript SDK 示例
- [prompts/](./prompts/) - AI 提示词模板

## 认证方式

使用 v2 签名认证（HMAC-SHA256）：

```python
import hmac
import hashlib

def generate_signature(method, url, timestamp, nonce, content_sha256, secret_key):
    sign_str = f"{method}\n{url}\n{timestamp}\n{nonce}\n{content_sha256}"
    return hmac.new(secret_key.encode(), sign_str.encode(), hashlib.sha256).hexdigest()
```

## 许可证

MIT

## 适用人群

- 需要接入证件照能力的开发者
- 正在做 AI 图像工具、SaaS、小程序的人
- 想快速验证证件照业务闭环的独立开发者
- 希望把自然语言能力接到真实图像处理接口上的 AI 应用开发者

## 联系方式

- 官网：https://zuimei.huipai.vip
