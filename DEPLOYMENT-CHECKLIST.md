# 部署前检查清单（Vercel）

> 本清单帮助你稳定上线 EXIF Photo Blog，并规避常见配置遗漏。请按顺序完成，勾选后再继续下一项。

## 1. 代码与项目

- [ ] 仓库就绪（Fork 或自有仓库），默认分支为 main
- [ ] Vercel 项目已创建并与仓库连接
- [ ] 包管理器使用 pnpm（Vercel 会自动识别）

## 2. 存储与数据库

- [ ] Vercel Postgres 已创建并绑定到项目（或自定义 POSTGRES_URL 已确认可连通）
- [ ] 若使用自定义 Postgres：DISABLE_POSTGRES_SSL=1（按需）
- [ ] 选择一个对象存储并完成创建与权限配置（四选一）
  - Vercel Blob（默认，无需额外域名）
  - AWS S3（桶公共读 + CORS）
  - Cloudflare R2（Public Domain + CORS）
  - MinIO（公共读策略 + 专用用户策略）
- [ ] 已设置相应的环境变量（见 .env.example）
- [ ] CORS 已包含本地与线上来源：
  - <http://localhost:3000>
  - <https://{VERCEL_PROJECT_NAME}*.vercel.app>
  - <https://{PRODUCTION_DOMAIN}>

## 3. 环境变量（最小可用集）

- [ ] NEXT_PUBLIC_DOMAIN=photos.example.com
- [ ] AUTH_SECRET=（使用 <https://generate-secret.vercel.app/32> 生成）
- [ ] ADMIN_EMAIL、ADMIN_PASSWORD
- [ ] 存储凭据：根据选择的存储提供商设置（严禁给私钥加 NEXT_PUBLIC 前缀）
- [ ]（可选）NEXT_PUBLIC_LOCALE、AI 相关、显示/性能调优变量

## 4. AI（可选）

- [ ] OPENAI_SECRET_KEY 已设置，且账户有额度
- [ ]（可选）OPENAI_BASE_URL（使用兼容提供商）
- [ ] 使用限制与速率限制：建议在 Vercel 中接入 Upstash Redis
- [ ] AI_TEXT_AUTO_GENERATED_FIELDS（如 title,tags,semantic）

## 5. 域名与网络

- [ ] NEXT_PUBLIC_DOMAIN 与实际生产域名一致
- [ ] 若使用自定义 CDN（如 Cloudflare）已验证与 Vercel 的兼容性
- [ ] 如启用构建期静态化，注意大文件与 CDN 组合可能导致构建失败（见 README.zh-CN FAQ）

## 6. 构建与运行

- [ ] 能在本地以 vercel dev 启动并访问首页与 /admin
- [ ] Vercel 构建产出通过，无报错

## 7. 上线后验证

- [ ] 访问 /admin 使用 ADMIN_EMAIL/ADMIN_PASSWORD 登录成功
- [ ] 上传 1 张测试照片，确认：
  - [ ] 上传完成，EXIF 正常解析
  - [ ] 首页与 /grid 能看到新图（如有延迟，/admin/configuration -> Clear Cache）
  - [ ] /p/[photoId] 详情可打开
  - [ ] 分享时 OG 图片正常（必要时考虑开启静态化：NEXT_PUBLIC_STATICALLY_OPTIMIZE_PHOTOS=1 和 NEXT_PUBLIC_STATICALLY_OPTIMIZE_PHOTO_OG_IMAGES=1）
- [ ] /feed.json 与 /rss.xml 可访问（若启用 NEXT_PUBLIC_SITE_FEEDS=1）
- [ ] 主题切换、暗色/亮色正常
- [ ] 若启用下载或隐私选项，确认显示行为符合预期

## 8. 排错建议

- 构建失败或页面报错：先到 /admin/configuration 点击 Clear Cache
- 图片显示不全或跨域：检查存储 CORS 与公开访问域
- AI 报错：检查额度与权限（Responses API 写权限）
- 旧图数据不一致：在 /admin/photos/updates 同步或单图 Sync

---

附：关键文件

- README.zh-CN.md：完整中文说明
- .env.example：环境变量模板
- src/app/config.ts：前端公开配置项参考
