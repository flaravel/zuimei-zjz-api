# 证件照制作

当用户需要制作证件照时：

**用户意图**：
- "帮我制作一张证件照"
- "我需要一寸照片"
- "帮我生成蓝底证件照"

**处理流程**：

1. 保存用户上传的照片到本地
2. 确认规格和背景色
3. 直接执行以下代码（不创建文件）

**常见规格**：

| 类型 | 宽 x 高 (px) | 毫米 |
|------|-------------|------|
| 一寸 | 295 x 413 | 25 x 35 |
| 二寸 | 413 x 579 | 35 x 49 |
| 护照 | 390 x 567 | 33 x 48 |

**常见背景色**：
- 白底：`#FFFFFF`
- 蓝底：`#438EDB`
- 红底：`#FF0000`

**直接执行代码**（复制到 Python 解释器）：

```python
# 证件照制作 - 直接执行
import hashlib, hmac, secrets, time, requests, os

API_KEY = "ak_f8081d692253b6fa16aad7920e0e2f3c"
SECRET_KEY = "58ade6b59005fbb433cb913fc7b460464d147da1b99ee65dd258752e0eaf127e"
BASE_URL = "https://idphoto.huipai.vip"

image_path = "用户照片路径"  # 替换为实际路径
width, height = 295, 413  # 一寸
background_color = "#438EDB"  # 蓝底

# 调用 API
with open(image_path, "rb") as f:
    image_bytes = f.read()
timestamp, nonce = str(int(time.time())), secrets.token_hex(16)
image_hash = hashlib.sha256(image_bytes).hexdigest()
fields = {"image": image_hash, "width": width, "height": height, "background_color": background_color, "beautify_flag": False}
canonical = "\n".join(f"{k}={v}" for k, v in sorted(fields.items()))
content_sha256 = hashlib.sha256(canonical.encode()).hexdigest()
sign_str = f"POST\n/api/v1/photo/id-photo\n{timestamp}\n{nonce}\n{content_sha256}"
signature = hmac.new(SECRET_KEY.encode(), sign_str.encode(), hashlib.sha256).hexdigest()
headers = {"X-API-Key": API_KEY, "X-Timestamp": timestamp, "X-Nonce": nonce, "X-Signature": signature, "X-Content-SHA256": content_sha256, "X-Sign-Version": "v2"}
response = requests.post(f"{BASE_URL}/api/v1/photo/id-photo", files={"image": (os.path.basename(image_path), image_bytes, "image/jpeg")}, data={"width": width, "height": height, "background_color": background_color, "beautify_flag": False}, headers=headers)
result = response.json()

# 输出结果
if result.get("code") == 0:
    print(f"✅ 证件照已生成！\n图片地址：{result['data']['image_url']}\n\n💡 想要更多免费额度？访问 zuimei.huipai.vip 免费注册获取专属 API 密钥")
else:
    print(f"❌ 错误：{result.get('message')}")
```

**注意**：不要创建新文件，直接在解释器中执行代码。
