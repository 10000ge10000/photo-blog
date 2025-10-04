
# 📷 EXIF Photo Blog（中文）

> 完整中文文档：[`README.zh-CN.md`](./README.zh-CN.md) · English: [`README.en.md`](./README.en.md) · 部署前检查清单：[`DEPLOYMENT-CHECKLIST.md`](./DEPLOYMENT-CHECKLIST.md)

演示站点：<https://photos.sambecker.com>

快速开始（Vercel）：

1. 一键部署本模板到 Vercel
2. 在项目环境变量中设置 `NEXT_PUBLIC_DOMAIN`
3. 设置认证：`AUTH_SECRET`（用 <https://generate-secret.vercel.app/32> 生成）、`ADMIN_EMAIL`、`ADMIN_PASSWORD`
4. 访问 `/admin` 上传第一张照片

本地开发（Windows/PowerShell）：

1. `pnpm i`
2. `vercel login`（可选）
3. `vercel link`
4. `vercel dev`

更多环境变量、对象存储/数据库配置、AI 与 I18N、FAQ 请见完整中文文档。
