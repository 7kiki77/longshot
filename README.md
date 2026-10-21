# 一页长图 / One Long Shot

手机优先的长截图拼接网页（PWA），适合通过 GitHub Pages 等纯静态 HTTPS 托管。

## 功能

- 支持导入 1–32 张截图，按顺序自动识别重叠部分；可调整顺序、删除素材。
- 支持导入 iPhone 屏幕录制视频（MOV / MP4 等设备浏览器可解码的格式），最多抽取约 46 帧、前 90 秒，跳过部分重复画面。
- 支持统一裁切顶部、底部重复区域，按设备内存限制分段生成 JPG 长图。
- 支持预览、逐张下载、通过手机分享菜单发送或保存到相册。
- 完全在浏览器本机处理，不上传用户图像，关闭网页后素材不保留。
- Manifest + Service Worker 缓存，可在 iPhone Safari 中加入主屏幕后离线打开（首次联网加载必需）。

## 发布到 GitHub Pages（与“我的厨房”项目一样）

1. 在 GitHub 新建一个 Public 仓库（例如 `longshot`）。
2. 把本 ZIP 解压，然后上传 `index.html`, `app.js`, `styles.css`, `manifest.webmanifest`, `sw.js` 和 `icons/` 整个文件夹到仓库根目录。**不要直接上传 ZIP。**
3. 打开仓库 **Settings → Pages → Build and deployment → Deploy from a branch → main / (root) → Save**。
4. 等 GitHub Pages 提供站点地址，格式为 `https://你的GitHub用户名.github.io/longshot/`。
5. iPhone 使用 Safari 打开 HTTPS 站点，点“分享”→“添加到主屏幕”→“添加”。新版 iOS 可开启“作为 Web App 打开”。

GitHub Pages 的仓库和网页可以公开被访问，**不要把用户图片保存在仓库里**；此版本不会将任何用户图片发送给服务器。

## 使用建议

- **图片模式**：截图顺序应是从上到下，**相邻两张保留 15%–50% 重叠区域**，动态内容可能导致识别失败。确认接缝后分享图片。
- **视频模式**：在 iPhone 控制中心开始录屏，切到聊天/文章 App，连续**缓慢**向下滑动，然后停止录制；回到本工具导入录屏。建议 60 秒内，超过 90 秒仅处理前 90 秒。
- 图片拼接是启发式匹配，对相似背景、重复聊天气泡、动画、快速滑动、视频帧模糊等可能出现误判或内容缺失。识别不确定会提示用户检查；并非像素级保证。
- 为照顾 iPhone 内存，输出宽度上限 800px，较长的图会按 3,600 / 8,000 / 12,000px 切成多张；部分老设备自动回退到 3,600px。很长的聊天记录建议分段处理。
- 图片分享优先通过系统分享菜单“存储图像”，若 iOS 版本或浏览器不支持分享文件，则使用下载；Safari 下载通常先保存到“文件”App 的下载位置。

## 限制

- iOS 网页不能可靠控制其他 App 自动滚动，也不能默默在后台录制其他 App。此版本**不会请求屏幕捕获权限**，用户需自行截图/录屏后导入。
- 不需要服务器、数据库、账号或付费大模型 API，也不需要 Apple Developer Program。
- 兼容性需要 iPhone 真机验证，特别是 Safari 对特定 HEIC、MOV/HEVC 的解码和高分辨率图片导出。

## 本地预览

需通过 HTTP 服务访问才能测试 Service Worker，例如：

```bash
python -m http.server 8000
```

然后访问 `http://localhost:8000`。生产环境需要 HTTPS（GitHub Pages 自带）。
