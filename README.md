# 抖音 AI 夜校 · 网站源码展示版

## 完整源码与公司审查

**[下载完整脱敏源码（182个文件，31.5 MB）](https://github.com/liuleah1102-star/ai-night-school/releases/download/review-v10-2026-11-12/ai-night-school-review-source.zip)** · [查看发布说明](https://github.com/liuleah1102-star/ai-night-school/releases/tag/review-v10-2026-11-12)

当前 main 分支仅展开部分入口和审查文档，尚不是可直接构建的完整目录。完整前后端源码和图片资源均在上述附件中，请下载解压后审查/运行；GitHub 自动生成的 Source code ZIP 也只包含 main 的部分文件。

- [公司审查指南](REVIEW-GUIDE.md)
- [API 清单与测试样例](API-REVIEW.md)
- [完整源码文件及 SHA-256 清单](REVIEW-MANIFEST.json)

附件 SHA-256：`f71bc23562d8137f551ab45ae261014f52bb294c0655a0b27cba82efe2987f7e`。

基于 2026-11-11 已发布第 10 版整理。包含前端页面、卡片轮换动效、课程展示、Skill 社区、留言展示、证书接口和可视化管理后台代码。

## 技术栈
TypeScript / JavaScript、React、Vinext / Vite、CSS、Cloudflare Workers、D1（SQLite）与 R2。

## 本地开发
需要 Node.js >=22.13.0。

```sh
npm ci
npm run dev
```

生产构建：`npm run build`。运行与发布需要自行配置 Cloudflare/Sites 对应运行环境、D1/R2 和身份认证。

## 目录
- `lib/learning-pages.ts`：首页、课程页、留言页 HTML。
- `public/visual.css`：视觉排版；`public/course-deck.js`：翻卡动效。
- `public/learning.js`：课程和留言渲染。
- `public/community.js`：社区展示与收藏交互。
- `app/api/`：内容读取、后台发布、收藏与证书查询接口。
- `lib/content-store.ts`：内容校验及管理员身份限制。

## 公开版与线上站点的区别
- 删除证书名单与哈希索引，证书数据文件为空。
- 留言替换为演示数据，删除学员上传图片与视频。
- 删除管理员邮箱、原站项目标识、内部同步记录、数据库文件、部署凭证、Git 历史和本地环境。
- 管理员校验要求运行环境配置 `ADMIN_EMAIL`，并由可信认证网关注入用户身份；缺少配置默认拒绝，不提供默认密码或免登录后台。
- 不包含线上 D1/R2 数据、草稿、收藏记录及上传文件；不是生产数据备份。
- 保留公开课程和已公开社区资料及其来源，外部资源仍受原站权限限制。

## 使用与权利
代码仅供查看、评审与交流，尚未授予开源许可证。官方品牌、课程海报及第三方内容不因展示源码而转让权利，使用前请取得相应授权。依赖组件许可证保留在对应目录。
