# 公司代码审查交付说明

交付日期：2026-11-12。生产基线：已发布第10版，提交 `b12bb85c3d8bd0fce1c93b042be59a4c5e85e661`。
这是脱敏源码快照，供公司评审。生产数据库、密钥、证书名单、真实留言及生产Git历史不在交付范围内；文件清单和SHA-256见REVIEW-MANIFEST.json。

## 项目与运行
网站提供课程回放/资料、Skill共享社区与收藏、留言展示、证书记录查询及内容管理后台。TypeScript/JavaScript、React、Vinext/Vite、Cloudflare Workers、D1和R2。
Node.js >=22.13.0；`npm ci` 后 `npm run dev`，生产构建 `npm run build`。独立运行仍需配置D1/R2与可信身份认证入口，不能仅将文件上传到普通静态空间。

## 重点阅读
- 页面与样式：lib/learning-pages.ts、public/visual.css、public/learning.js、public/course-deck.js。
- 社区：public/community.html、public/community.js、lib/community.ts。
- 后台：lib/admin-page.ts、public/admin.js、lib/content-store.ts、app/api/admin/。
- 数据结构：db/schema.ts、drizzle/；接口清单：API-REVIEW.md。

## 与生产代码/数据的差异
1. lib/certificates-data.json：真实证书索引替换为{}，不含学员记录。
2. public/messages-data.json：替换为演示留言；public/mb-imgs/及历史账号截图删除。
3. lib/content-store.ts：固定生产管理员邮箱替换为ADMIN_EMAIL环境变量读取；为空时拒绝管理员权限。其余权限与内容校验逻辑保留。审查生产鉴权时请将该差异纳入考虑。
4. .openai/hosting.json：移除生产项目ID，保留DB/BUCKET声明。
5. public/graduation-wall-preview.svg：替换为无个人信息示意图。
6. 删除内部同步/迁移记录、生产Git历史及本地环境；新增README和审查材料。
7. 不含CMS中人工发布/草稿、R2上传及收藏数据库；源码默认JSON不能等同线上最新内容。11月12日核实的电商第五课尚为待合并资料，不包含在本次第10版基线。

## 审查关注点与已知限制
- 管理鉴权依赖平台注入的oai-authenticated-user-id/email；公司环境必须由可信网关注入并移除外部伪造同名头，不可直接暴露Worker后信任任意请求头。
- 证书按抖音号精确匹配并哈希索引；尚无本人账号绑定/验证码校验，查询不等于验证本人。证书图片未迁移，不能开放整表入口。
- 收藏使用访客身份标记，不等于实名账号；应评估刷量与限流。
- 历史投稿GET仍存在；匿名投稿POST返回405，正式投稿走飞书审核。
- 需重点复核输入/XSS、Origin校验、上传类型与大小、并发版本冲突、访问控制、内容发布和审核流程、依赖漏洞与数据最小化。
- 此说明不代表已通过公司安全审查。第10版生产构建与页面检查曾通过；脱敏交付包未在公司部署环境重新构建或做渗透测试。

## 权利与保密
代码用于查看、评审和交流，未指定开源许可。官方品牌、海报及第三方素材权利归各自权利人；对外复用需取得相应授权。请勿把生产数据库、证书名单或凭证追加到本交付库。
