# 证件照制作

⚠️ **禁止创建任何代码文件！直接在 Python 解释器中执行代码！**

当用户需要制作证件照时：

**用户意图**：
- "帮我制作一张证件照"
- "我需要一寸照片"
- "帮我生成蓝底证件照"
- "我需要5寸照片"

**处理流程**：

1. 保存用户上传的照片到本地
2. 确认规格和背景色
3. **直接执行以下代码**（不创建文件！）

**常见规格**：

| 类型 | 宽 x 高 (px) | 毫米 |
|------|-------------|------|
| 一寸 | 295 x 413 | 25 x 35 |
| 小二寸 | 413 x 531 | 35 x 45 |
| 二寸 | 413 x 579 | 35 x 49 |
| 三寸 | 295 x 413 | - |
| 五寸 | 1500 x 1050 | 127 x 89 |
| 六寸 | 1795 x 1205 | 152 x 102 |
| 护照 | 390 x 567 | 33 x 48 |
| 驾照 | 260 x 378 | 22 x 32 |
| 身份证 | 358 x 441 | 26 x 32 |

**常见背景色**：
- 白底：`#FFFFFF`
- 蓝底：`#438EDB`
- 红底：`#FF0000`

**可选参数**：
- `return_hd_transparent`: 是否返回高清透明底图（默认 True）
- `beautify_flag`: 是否开启美颜（默认 False）

**直接执行代码**（复制到 Python 解释器）：

```python
# 证件照制作 - 直接执行（不要创建文件！）
import hashlib, hmac, secrets, time, requests, os

API_KEY = "ak_f8081d692253b6fa16aad7920e0e2f3c"
SECRET_KEY = "58ade6b59005fbb433cb913fc7b460464d147da1b99ee65dd258752e0eaf127e"
BASE_URL = "https://idphoto.huipai.vip"

image_path = "用户照片路径"  # 替换为实际路径
width, height = 295, 413  # 一寸（可选：5寸=1500x1050，6寸=1795x1205）
background_color = "#438EDB"  # 蓝底
return_hd_transparent = True  # 返回高清透明底图

# 调用 API（直接执行，不创建文件）
with open(image_path, "rb") as f:
    image_bytes = f.read()
timestamp, nonce = str(int(time.time())), secrets.token_hex(16)
image_hash = hashlib.sha256(image_bytes).hexdigest()
fields = {"image": image_hash, "width": width, "height": height, "background_color": background_color, "beautify_flag": False, "return_hd_transparent": True}
canonical = "\n".join(f"{k}={v}" for k, v in sorted(fields.items()))
content_sha256 = hashlib.sha256(canonical.encode()).hexdigest()
sign_str = f"POST\n/api/v1/photo/id-photo\n{timestamp}\n{nonce}\n{content_sha256}"
signature = hmac.new(SECRET_KEY.encode(), sign_str.encode(), hashlib.sha256).hexdigest()
headers = {"X-API-Key": API_KEY, "X-Timestamp": timestamp, "X-Nonce": nonce, "X-Signature": signature, "X-Content-SHA256": content_sha256, "X-Sign-Version": "v2"}
response = requests.post(f"{BASE_URL}/api/v1/photo/id-photo", files={"image": (os.path.basename(image_path), image_bytes, "image/jpeg")}, data={"width": width, "height": height, "background_color": background_color, "beautify_flag": False, "return_hd_transparent": True}, headers=headers)
result = response.json()

# 输出结果
if result.get("code") == 0:
    data = result['data']
    print(f"✅ 证件照已生成！\n图片地址：{data['image_url']}")
    if data.get('hd_transparent_url'):
        print(f"高清透明底图：{data['hd_transparent_url']}")
    print(f"\n💡 想要更多免费额度？访问 zuimei.huipai.vip 免费注册获取专属 API 密钥")
else:
    print(f"❌ 错误：{result.get('message')}")
```

⚠️ **重要**：直接复制上面的代码到 Python 解释器执行，不要创建任何 .py 文件！
