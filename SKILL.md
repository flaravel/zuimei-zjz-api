---
name: zuimei-zjz-api
description: 最美证件照 - AI 图像处理 API，证件照制作、智能抠图、人脸增强、换背景色
version: 1.0.0
author: 最美证件照团队
website: https://zuimei.huipai.vip
---

# 最美证件照 API Skill

让 AI 助手调用最美证件照的图像处理 API。

**官网**：https://zuimei.huipai.vip

## 触发条件 (Gate)

当用户提到以下关键词时触发此技能：

**证件照相关**：
- 证件照、一寸照、二寸照、护照照、签证照、驾照照、身份证照
- 蓝底、红底、白底、渐变背景
- 制作证件照、生成证件照

**抠图相关**：
- 抠图、抠人像、去背景、透明背景、移除背景、消除背景
- 人像分离、背景分离

**换背景相关**：
- 换背景、改背景色、换底色、改底色、背景颜色

**增强修复相关**：
- 照片修复、模糊修复、变清晰、增强、画质提升
- 老照片修复、模糊照片

**排版相关**：
- 排版照、打印排版、冲印排版、6寸照、5寸照

**编辑相关**：
- 改尺寸、调大小、转换格式、改DPI、裁剪尺寸

## 安全配置

### 免费测试凭据

本 Skill 提供免费测试凭据，安装后可直接使用：

```
ZUIMEI_API_KEY="ak_f8081d692253b6fa16aad7920e0e2f3c"
ZUIMEI_SECRET_KEY="58ade6b59005fbb433cb913fc7b460464d147da1b99ee65dd258752e0eaf127e"
```

**方式一：环境变量**（推荐）

在 `~/.openclaw/.env` 文件中添加：
```bash
ZUIMEI_API_KEY="ak_f8081d692253b6fa16aad7920e0e2f3c"
ZUIMEI_SECRET_KEY="58ade6b59005fbb433cb913fc7b460464d147da1b99ee65dd258752e0eaf127e"
```

**方式二：OpenClaw 配置文件**

在 `~/.openclaw/openclaw.json` 中配置：
```json5
{
  skills: {
    entries: {
      "zuimei-zjz-api": {
        enabled: true,
        apiKey: { source: "env", provider: "default", id: "ZUIMEI_API_KEY" },
        env: {
          ZUIMEI_API_KEY: "ak_f8081d692253b6fa16aad7920e0e2f3c",
          ZUIMEI_SECRET_KEY: "58ade6b59005fbb433cb913fc7b460464d147da1b99ee65dd258752e0eaf127e",
        },
      },
    },
  },
}
```

**安全要求**：
1. **密钥存储**：API 密钥应存储在环境变量或安全的密钥管理系统中，禁止硬编码
2. **传输安全**：所有 API 请求必须使用 HTTPS
3. **签名验证**：每次请求必须携带有效的 v2 签名
4. **密钥轮换**：定期更换 API 密钥，发现泄露立即重置
5. **权限控制**：不同 API 密钥可配置不同的调用权限和配额

**签名说明**：
- 使用 HMAC-SHA256 算法
- 签名字符串格式：`{method}\n{url}\n{timestamp}\n{nonce}\n{content_sha256}`
- 时间戳有效期：5 分钟
- Nonce 防重放：5 分钟内有效

## API 端点

| 能力 | API 端点 | 说明 |
|------|----------|------|
| 一键证件照 | `/api/v1/photo/id-photo` | 完整证件照制作流程 |
| 智能抠图 | `/api/v1/segment/portrait` | 人像抠图，生成透明 PNG |
| 换背景色 | `/api/v1/segment/background` | 替换背景颜色，支持渐变和多色 |
| 人脸增强 | `/api/v1/photo/enhance` | 修复模糊、提升清晰度 |
| 排版照 | `/api/v1/photo/layout` | 生成打印排版照 |
| 图片编辑 | `/api/v1/photo/edit` | 尺寸调整、格式转换 |

## 使用场景

### 1. 证件照制作
用户需要制作证件照（一寸、二寸、护照照等）
```
调用 /api/v1/photo/id-photo
支持多种背景色、可选美颜
```

### 2. 人像抠图
用户需要将人物从背景中分离
```
调用 /api/v1/segment/portrait
生成透明背景 PNG
```

### 3. 换背景色
用户需要更换照片背景颜色
```
调用 /api/v1/segment/background
支持纯色、渐变、多色输出
```

### 4. 照片修复
用户有模糊照片需要修复
```
调用 /api/v1/photo/enhance
人脸增强 + 超分辨率
```

### 5. 打印排版
用户需要将证件照排版打印
```
调用 /api/v1/photo/layout
生成 6 寸/5 寸/A4 排版照
```

### 6. 图片编辑
用户需要调整图片尺寸、格式
```
调用 /api/v1/photo/edit
调整尺寸、DPI、格式转换
```

## 快速开始

### 1. 获取 API 密钥

在平台注册账号，获取：
- `api_key`: API Key ID（如 `ak_xxxx`）
- `secret_key`: 密钥（用于签名）

### 2. 签名认证

所有请求需要通过 v2 签名认证：

```python
import hmac
import hashlib
import time
import secrets

def generate_signature(method, url, timestamp, nonce, content_sha256, secret_key):
    """生成 v2 签名"""
    sign_str = f"{method}\n{url}\n{timestamp}\n{nonce}\n{content_sha256}"
    return hmac.new(
        secret_key.encode(),
        sign_str.encode(),
        hashlib.sha256
    ).hexdigest()

def build_content_sha256(fields: dict) -> str:
    """计算内容摘要"""
    canonical_lines = []
    for key in sorted(fields.keys()):
        value = fields[key]
        if value is None:
            continue
        if isinstance(value, bool):
            value = "true" if value else "false"
        canonical_lines.append(f"{key}={value}")
    canonical_body = "\n".join(canonical_lines)
    return hashlib.sha256(canonical_body.encode()).hexdigest()

def calculate_image_sha256(file_bytes: bytes) -> str:
    """计算图片 SHA256"""
    return hashlib.sha256(file_bytes).hexdigest()
```

### 3. 发送请求

```python
import requests

def call_api(api_key, secret_key, endpoint, files=None, data=None):
    """调用 API"""
    base_url = "https://idphoto.huipai.vip"
    timestamp = str(int(time.time()))
    nonce = secrets.token_hex(16)

    # 计算内容摘要
    fields = {}
    if files and "image" in files:
        file_bytes = files["image"][1]
        fields["image"] = calculate_image_sha256(file_bytes)
    if data:
        fields.update(data)

    content_sha256 = build_content_sha256(fields)

    # 生成签名
    signature = generate_signature(
        method="POST",
        url=endpoint,
        timestamp=timestamp,
        nonce=nonce,
        content_sha256=content_sha256,
        secret_key=secret_key
    )

    # 发送请求
    headers = {
        "X-API-Key": api_key,
        "X-Timestamp": timestamp,
        "X-Nonce": nonce,
        "X-Signature": signature,
        "X-Content-SHA256": content_sha256,
        "X-Sign-Version": "v2"
    }

    response = requests.post(
        f"{base_url}{endpoint}",
        files=files,
        data=data,
        headers=headers
    )

    return response.json()
```

## API 详细说明

详见 [API.md](./API.md)

## 示例代码

详见 [examples/](./examples/) 目录

## 注意事项

1. **时间戳有效期**：请求时间戳与服务器时间差不超过 5 分钟
2. **Nonce 防重放**：每个 nonce 只能使用一次，5 分钟内有效
3. **图片来源**：支持 `image` 文件上传或 `image_base64` 编码
4. **计费**：部分 API 收费，详见各 API 说明
