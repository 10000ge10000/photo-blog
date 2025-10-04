
# 📷 EXIF Photo Blog（中文安装与使用指南）

> 本项目修改自 [sambecker/exif-photo-blog](https://github.com/sambecker/exif-photo-blog)，感谢原作者的杰出工作。

> English version: [README.en.md](./README.en.md) · 部署前检查清单：[`DEPLOYMENT-CHECKLIST.md`](./DEPLOYMENT-CHECKLIST.md)

<https://github.com/sambecker/exif-photo-blog/assets/169298/4253ea54-558a-4358-8834-89943cfbafb4>

演示站点：<https://photos.sambecker.com>

## ✨ 功能特性

- 内置登录认证
- 照片上传与 EXIF 元数据提取（相机/镜头/曝光等）
- 标签组织与浏览
- 无限滚动
- 明/暗色主题
- 自动生成 Open Graph（OG）社交分享图
- 内置 CMD-K 全局命令面板与照片搜索
- AI 生成标题/说明/语义摘要/标签（可选）
- RSS 与 JSON Feeds
- Fujifilm 胶片模拟与配方支持

预览图：`/readme/og-image-share.png`

---

## 🛠️ 安装部署

本项目推荐使用 Vercel 一键部署，它会自动处理数据库和对象存储的创建与连接。

### 1) 一键部署 (推荐)

1. 点击下方按钮一键部署：

    [![Deploy with Vercel](https://vercel.com/button)](https://vercel.com/new/clone?repository-url=https%3A%2F%2Fgithub.com%2F10000ge10000%2Fphoto-blog&repository-name=photo-blog&demo-title=Photo+Blog&demo-description=Store+photos+with+original+camera+data&demo-url=https%3A%2F%2Fphotos.sambecker.com&demo-image=https%3A%2F%2Fphotos.sambecker.com%2Ftemplate-image-tight&project-name=Photo+Blog&from=templates&skippable-integrations=1&teamCreateStatus=hidden&stores=%5B%7B%22type%22%3A%22postgres%22%7D%2C%7B%22type%22%3A%22blob%22%7D%5D)

2. 部署完成后，请继续参考下方的**手动配置**部分，完成身份认证和基础内容配置。

### 2) 手动配置

根据您的需求，配置以下环境变量。

#### 数据库与存储 (Database & Storage)

##### 数据库 (Database)

- **Postgres**: 创建您的数据库，并将连接字符串存入 `POSTGRES_URL` 环境变量。如果您使用 Vercel Postgres，此过程将自动完成。

##### 对象存储 (Storage)

您需要选择并配置以下其中一种对象存储方案：

- **Vercel Blob**: 创建 Blob 存储并连接到您的项目。如果您使用 Vercel 一键部署，此过程将自动完成。
- **Cloudflare R2**: 创建并配置 R2 Bucket。详见后文「可选对象存储提供商」章节。
- **AWS S3**: 创建并配置 S3 Bucket。详见后文「可选对象存储提供商」章节。
- **MinIO**: 搭建并配置您的 MinIO 服务。详见后文「可选对象存储提供商」章节。

#### 身份认证 (Authentication)

##### 设置 Auth

- 生成一个 32 位的随机字符串作为 `AUTH_SECRET`。您可以访问 <https://generate-secret.vercel.app/32> 快速生成。
- **环境变量**: `AUTH_SECRET`

##### 设置管理员用户

- 设置用于登录后台的管理员邮箱和密码。
- **环境变量**:
  - `ADMIN_EMAIL`
  - `ADMIN_PASSWORD`

#### 基础内容 (Content)

##### 配置语言

- 设置站点前台展示的语言。
- **值**: `zh-cn`
- **环境变量**: `NEXT_PUBLIC_LOCALE` (支持的语言请参考 README 后续章节)

##### 配置域名

- 设置您的站点域名，它将用于生成分享链接，并且在未设置导航标题时显示在导航栏。
- **值**: `photo.910501.xyz`
- **环境变量**: `NEXT_PUBLIC_DOMAIN`

##### 配置页面标题

- 设置在浏览器标签页和搜索引擎结果中显示的标题。
- **值**: `Photo Blog`
- **环境变量**: `NEXT_PUBLIC_META_TITLE`

### 3) 上传你的第一张照片 🎉

1. 访问 `/admin`
2. 使用您配置的管理员账号登录
3. 点击「Upload Photos」上传照片
4. 可选：填写标题/说明等信息
5. 点击「Create」创建

---

## 🔄 跟进模板更新

- 如果你不打算修改代码，或可以接受将更新公开，推荐直接 Fork 本仓库：<https://github.com/sambecker/exif-photo-blog/fork>，以便后续便捷同步上游更新。
- 若已在 Vercel 创建项目，参考英文版 README 的「How do I receive template updates?」迁移指引。

---

## 💻 本地开发（Windows / PowerShell）

1. 克隆代码仓库
2. 安装依赖：`pnpm i`
3. 可选：安装并登录 Vercel CLI：`vercel login`
4. 关联项目：`vercel link`
5. 启动开发服务：`vercel dev`
   - 使用 Vercel 托管的环境变量进行本地开发

提示：本地开发对对象存储有依赖，详见下方「常见问题」中关于本地开发限制的说明。

---

## 🎛️ 进阶自定义

### AI 文本生成（可选）

⚠️ 重要说明：启用 AI 功能会产生第三方费用（OpenAI）。请务必设置用量上限和速率限制，避免意外开销。不要把 OpenAI 密钥设置为以 `NEXT_PUBLIC_` 开头的变量。

1. 开通 OpenAI 并充值（如遇权限问题参见 Issues #110）
2. 生成 API Key，并在环境变量中设置：`OPENAI_SECRET_KEY`
3. 推荐：通过 Upstash Redis 添加「速率限制」
   - 在 Vercel 的 Storage 选项卡中创建 Upstash Redis，并绑定到项目
   - 若要求设置前缀，建议使用 `EXIF`
4. 可选：指定哪些字段在上传时自动生成（逗号分隔）：
   - `AI_TEXT_AUTO_GENERATED_FIELDS = title,semantic`
   - 可选值：
     - `all`
     - `title`（默认）
     - `caption`
     - `tags`（默认）
     - `semantic`（默认）
     - `none`
5. 可选：使用兼容 OpenAI 的其他服务
   - 设置 `OPENAI_BASE_URL`

### Vercel Analytics 与 Speed Insights（可选）

- Analytics：在 Vercel 控制台点击「Analytics」并启用（项目已包含 `@vercel/analytics`）
- Speed Insights：在 Vercel 控制台点击「Speed Insights」并启用（项目已包含 `@vercel/speed-insights`）

### 可选配置（环境变量）

以下环境变量用于控制站点内容、性能、分类、排序、显示、网格、设计、设置等行为。

#### 内容（Content）

- `NEXT_PUBLIC_META_TITLE`：用于搜索结果与浏览器标签
- `NEXT_PUBLIC_META_DESCRIPTION`：用于搜索结果
- `NEXT_PUBLIC_NAV_TITLE`：右上角导航标题（未配置时默认显示域名）
- `NEXT_PUBLIC_NAV_CAPTION`：右上角副标题
- `NEXT_PUBLIC_PAGE_ABOUT`：网格页侧栏简介（支持 `<b> <strong> <i> <em> <u> <br>` 标签）
- `NEXT_PUBLIC_DOMAIN_SHARE`：分享弹窗使用的短域名

#### 性能（Performance）

⚠️ 启用会增加平台用量。若遇到构建失败，可参考 FAQ 的排查建议。

- `NEXT_PUBLIC_STATICALLY_OPTIMIZE_PHOTOS = 1`：照片详情页 `p/[photoId]` 静态化（构建时预渲染）
- `NEXT_PUBLIC_STATICALLY_OPTIMIZE_PHOTO_OG_IMAGES = 1`：照片 OG 图静态化
- `NEXT_PUBLIC_STATICALLY_OPTIMIZE_PHOTO_CATEGORIES = 1`：分类页（如 `tag/[tag]`、`shot-on/[make]/[model]` 等）静态化
- `NEXT_PUBLIC_STATICALLY_OPTIMIZE_PHOTO_CATEGORY_OG_IMAGES = 1`：分类页 OG 图静态化
- `NEXT_PUBLIC_PRESERVE_ORIGINAL_UPLOADS = 1`：保留原图（上传前不压缩）
- `NEXT_PUBLIC_IMAGE_QUALITY = 1-100`：大图压缩质量（默认 75）
- `NEXT_PUBLIC_BLUR_DISABLED = 1`：禁用占位模糊数据（可减少 Postgres 使用量）

#### 分类（Categories）

- `NEXT_PUBLIC_CATEGORY_VISIBILITY`：控制侧栏与 CMD-K 中出现哪些集合及顺序（逗号分隔）。例如将相机置于标签前，并隐藏胶片模拟：`cameras,tags,lenses,recipes`。
  - 可选值：`recents`（默认）、`years`、`tags`（默认）、`cameras`（默认）、`lenses`（默认）、`recipes`（默认）、`films`（默认）、`focal-lengths`
- `NEXT_PUBLIC_HIDE_CATEGORIES_ON_MOBILE = 1`：移动端隐藏侧栏分类
- `NEXT_PUBLIC_HIDE_CATEGORY_IMAGE_HOVERS = 1`：悬停分类链接时不显示图片预览
- `NEXT_PUBLIC_EXHAUSTIVE_SIDEBAR_CATEGORIES = 1`：侧栏固定为展开状态
- `NEXT_PUBLIC_HIDE_TAGS_WITH_ONE_PHOTO = 1`：仅显示至少包含 2 张照片的标签

#### 排序（Sorting）

- `NEXT_PUBLIC_DEFAULT_SORT`：网格/满幅首页默认排序
  - 可选值：`taken-at`（默认）、`taken-at-oldest-first`、`uploaded-at`、`uploaded-at-oldest-first`
- `NEXT_PUBLIC_NAV_SORT_CONTROL`：首页排序控件显示方式
  - 可选值：`none`、`toggle`（默认）、`menu`
- 颜色排序（实验特性）
  - `NEXT_PUBLIC_SORT_BY_COLOR = 1`：启用基于颜色的排序（强制使用 `menu` 控件，并在后台标记缺少颜色数据的照片）
  - `NEXT_PUBLIC_COLOR_SORT_STARTING_HUE`：起始色相（0-360，默认 80）
  - `NEXT_PUBLIC_COLOR_SORT_CHROMA_CUTOFF`：色度阈值（0-0.37，默认 0.05）
- `NEXT_PUBLIC_PRIORITY_BASED_SORTING = 1`：排序时考虑优先级字段（可能影响性能）

#### 显示（Display）

- `NEXT_PUBLIC_HIDE_KEYBOARD_SHORTCUT_TOOLTIPS = 1`：隐藏快捷键提示
- `NEXT_PUBLIC_HIDE_EXIF_DATA = 1`：隐藏 EXIF 信息（适合以作品集为主的站点）
- `NEXT_PUBLIC_HIDE_ZOOM_CONTROLS = 1`：隐藏全屏大图缩放控件
- `NEXT_PUBLIC_HIDE_TAKEN_AT_TIME = 1`：隐藏拍摄时间（只保留日期）
- `NEXT_PUBLIC_HIDE_REPO_LINK = 1`：隐藏页脚仓库链接

#### 网格（Grid）

- `NEXT_PUBLIC_GRID_HOMEPAGE = 1`：首页使用网格布局
- `NEXT_PUBLIC_GRID_ASPECT_RATIO = 1.5`：网格缩略图显示比例（默认 1；设为 0 取消约束）
- `NEXT_PUBLIC_SHOW_LARGE_THUMBNAILS = 1`：强制使用较大缩略图（否则由比例决定密度）

#### 设计（Design）

- `NEXT_PUBLIC_DEFAULT_THEME = light | dark`：默认主题（未配置时为 `system`）
- `NEXT_PUBLIC_MATTE_PHOTOS = 1`：为照片加“画框”边距与边框（适合竖图；颜色可通过 `NEXT_PUBLIC_MATTE_COLOR` 与 `NEXT_PUBLIC_MATTE_COLOR_DARK` 设置）

#### 设置（Settings）

- `NEXT_PUBLIC_GEO_PRIVACY = 1`：隐私模式，不收集/展示地理信息（会重新压缩以移除 GPS）
- `NEXT_PUBLIC_ALLOW_PUBLIC_DOWNLOADS = 1`：允许所有访客下载原图（可能增加带宽消耗）
- `NEXT_PUBLIC_SOCIAL_NETWORKS`：分享弹窗中的社交平台列表（逗号分隔）
  - 可选值：`x`（默认）、`threads`、`facebook`、`linkedin`、`all`、`none`
- `NEXT_PUBLIC_SITE_FEEDS = 1`：启用 `/feed.json` 与 `/rss.xml`
- `NEXT_PUBLIC_OG_TEXT_ALIGNMENT = BOTTOM`：OG 文本底部对齐（默认顶部）

#### 页面脚本与分析（Scripts & Analytics）

- `PAGE_SCRIPT_URLS`：以逗号分隔的一组 URL，将注入至 body 底部（通过 next/script）
  - URL 必须以 `https` 开头
  - ⚠️ 将在每个页面执行任意脚本，请谨慎使用

---

## 🗄️ 可选对象存储提供商（单选）

仅支持同时启用一种存储：Vercel Blob、Cloudflare R2、AWS S3 或 MinIO。建议在上传任何照片之前完成配置。

- 若配置了多种，可通过 `NEXT_PUBLIC_STORAGE_PREFERENCE` 指定优先级：`aws-s3`、`cloudflare-r2`、`minio`、`vercel-blob`

### Cloudflare R2

1）创建 Bucket 并设置 CORS：

```json
[
  {
    "AllowedHeaders": ["*"],
    "AllowedMethods": ["GET", "PUT"],
    "AllowedOrigins": [
      "http://localhost:3000",
      "https://{VERCEL_PROJECT_NAME}*.vercel.app",
      "{PRODUCTION_DOMAIN}"
    ]
  }
]
```

- 启用公共访问：绑定自定义域（Cloudflare 域名）或允许使用 `r2.dev` 公共子域
- 设置公有端变量：
  - `NEXT_PUBLIC_CLOUDFLARE_R2_BUCKET`
  - `NEXT_PUBLIC_CLOUDFLARE_R2_ACCOUNT_ID`
  - `NEXT_PUBLIC_CLOUDFLARE_R2_PUBLIC_DOMAIN`（自定义域或 `*.r2.dev`）

2）创建 API Token（仅限目标 Bucket 的读写权限）并设置私有凭据（不要带 `NEXT_PUBLIC` 前缀）：

- `CLOUDFLARE_R2_ACCESS_KEY`
- `CLOUDFLARE_R2_SECRET_ACCESS_KEY`

### AWS S3

1）创建 Bucket：开启 ACL，关闭“阻止全部公共访问”；设置 CORS：

```json
[
  {
    "AllowedHeaders": ["*"],
    "AllowedMethods": ["GET", "PUT"],
    "AllowedOrigins": [
      "http://localhost:*",
      "https://{VERCEL_PROJECT_NAME}*.vercel.app",
      "{PRODUCTION_DOMAIN}"
    ],
    "ExposeHeaders": []
  }
]
```

- 设置公有端变量：
  - `NEXT_PUBLIC_AWS_S3_BUCKET`
  - `NEXT_PUBLIC_AWS_S3_REGION`（例如 `us-east-1`）

2）创建 IAM 策略 + 用户，分配读写权限并生成密钥（不要带 `NEXT_PUBLIC` 前缀）：

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "s3:PutObject",
        "s3:PutObjectACL",
        "s3:GetObject",
        "s3:ListBucket",
        "s3:DeleteObject"
      ],
      "Resource": [
        "arn:aws:s3:::{BUCKET_NAME}",
        "arn:aws:s3:::{BUCKET_NAME}/*"
      ]
    }
  ]
}
```

- 设置私有凭据：`AWS_S3_ACCESS_KEY`、`AWS_S3_SECRET_ACCESS_KEY`

### MinIO（自托管，兼容 S3）

1）部署 MinIO 并创建 Bucket；为 Bucket 设置只读公共策略（示例略）；

- 公有端变量：
  - `NEXT_PUBLIC_MINIO_BUCKET`
  - `NEXT_PUBLIC_MINIO_DOMAIN`
  - `NEXT_PUBLIC_MINIO_PORT`（可选）
  - `NEXT_PUBLIC_MINIO_DISABLE_SSL = 1`（禁用 SSL；默认使用 HTTPS）

2）创建具备仅限该 Bucket 读/写/列举/删除权限的用户，并设置私有凭据（不要带 `NEXT_PUBLIC` 前缀）：
- `MINIO_ACCESS_KEY`
- `MINIO_SECRET_ACCESS_KEY`


---

## 🐘 备选数据库提供商（实验特性）

- 通过设置 `POSTGRES_URL` 切换到其他 Postgres 兼容（支持连接池）的提供商。
- 某些提供商需要禁用 SSL，可设置：`DISABLE_POSTGRES_SSL = 1`。

Supabase 示例：

1. 连接串需使用“事务模式（Transaction Mode）”，端口为 `6543`
2. 设置 `DISABLE_POSTGRES_SSL = 1`

---

## 🌐 国际化（I18N）

- 通过 `NEXT_PUBLIC_LOCALE` 指定前台（非管理端）文案语言。
- 当前内置语言：
  - `bd-bn`、`en-gb`、`en-us`、`id-id`、`pt-br`、`pt-pt`、`tr-tr`、`zh-cn`

---

## ❓ 常见问题（FAQ）

- 为什么照片修改后没有立刻生效？
  - 首页与网格页默认使用静态优化，变更可能需要几分钟才能传播。可以到 `/admin/configuration` 点击「Clear Cache」清理缓存。

- 启用静态优化后，为什么生产环境构建失败？
  - 有报告显示：大于 30MB 的大图或在 Vercel 前使用 CDN（例如 Cloudflare）可能会影响稳定性。可暂时关闭相关静态优化开关或参考 Issues #184/#185。

- 为什么旧照片的效果/数据不一致？
  - 历史版本在模糊、镜头、AI/隐私等方面的处理方式有所演进。可在照片旁点击同步按钮，或前往 `/admin/photos/updates` 批量同步需要更新的照片。

- 分享链接时 OG 图不显示？
  - iMessage、Slack、X 等平台抓取超时很敏感。建议开启 `NEXT_PUBLIC_STATICALLY_OPTIMIZE_PHOTOS = 1` 与 `NEXT_PUBLIC_STATICALLY_OPTIMIZE_PHOTO_OG_IMAGES = 1` 预渲染，但这会增加平台用量。

- 网格缩略图太小？
  - 网格密度由长宽比决定（≤1 的比例每行更多）。可设置 `NEXT_PUBLIC_GRID_ASPECT_RATIO` 或强制大缩略图 `NEXT_PUBLIC_SHOW_LARGE_THUMBNAILS = 1`。

- 标记为私密的照片是否绝对安全？
  - 私密路径需要登录才能访问，但对象存储直链仍然可被访问（具有不可预测的随机 URL）。请谨慎对待。

- 本地开发是否必须依赖外部对象存储？
  - 目前是的。若你有可行策略能在本地模拟外部存储并配合 `next/image` 调试，欢迎提交 PR。

- 是否支持 HEIC？
  - 目前 `sharp` 与 `next/image` 尚不支持直接处理 HEIC。通过 Apple 系统的分享面板上传会自动转为 JPG。详见相关 issue 讨论。

- Fujifilm 模拟为何没有和 EXIF 一起导入？
  - 模拟数据存在厂商私有 Makernote 区。中间处理（如 iOS 裁剪）可能导致数据缺失。尽量使用相机原始文件，或在后台手动选择。

- 启用 AI 后连不上 OpenAI？
  - 需要预先充值/购买额度；如自定义了 API Key 权限，请确保开启 Responses API 写权限。

---

## 🧪 脚本与命令

- 主要命令（见 `package.json`）：
  - `pnpm dev`：开发模式（Next.js + Turbopack）
  - `pnpm build`：构建
  - `pnpm start`：启动生产服务
  - `pnpm lint`：ESLint 检查
  - `pnpm test`：Jest 测试（watch 模式）
  - `pnpm analyze`：打包分析（`ANALYZE=true next build`）

---

## 🔐 关键环境变量速查

- 认证与管理员：`AUTH_SECRET`、`ADMIN_EMAIL`、`ADMIN_PASSWORD`
- 站点域名：`NEXT_PUBLIC_DOMAIN`
- 数据库：`POSTGRES_URL`、`DISABLE_POSTGRES_SSL`
- 存储（四选一）：Vercel Blob（`BLOB_READ_WRITE_TOKEN`）、Cloudflare R2、AWS S3、MinIO（详见上文各自变量）
- AI：`OPENAI_SECRET_KEY`、`OPENAI_BASE_URL`（可选自定义）
- 本地化：`NEXT_PUBLIC_LOCALE`

---

## ✅ 建议的验证步骤

1. 部署完成后，访问首页确认能打开
2. 访问 `/admin` 使用管理员账号登录
3. 上传一张测试照片，查看照片详情与 EXIF
4. 如启用了 AI，验证上传后是否能生成标题/描述/标签
5. 配置自定义导航标题/简介等，确认能在前台显示

如遇问题，可以在后台的「Configuration」页点击「Clear Cache」再尝试，或前往上游仓库提交 issue。

---

祝使用顺利，拍摄愉快！📸
