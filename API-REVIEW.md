# API List 与测试样例

所有示例均为虚构数据。写接口的测试请在独立测试环境执行，避免修改生产内容。`ADMIN_EMAIL`只是交付版配置入口，不是登录绕过方式。

| 方法 | 路径 | 权限及行为 |
|---|---|---|
| GET | /api/content/courses | 公开，读取已发布课程 |
| GET | /api/content/community | 公开，隐藏作品不返回 |
| GET | /api/content/messages | 公开，隐藏留言不返回 |
| GET | /api/admin/content?kind=courses | 管理员，返回草稿、已发布、revision及最近记录；kind可为community/messages |
| POST | /api/admin/content | 管理员+同源；draft/publish/restore；revision不匹配409 |
| POST | /api/admin/upload | 管理员+同源；原始二进制PNG/JPG/WebP，最大8MB，返回/media/{id} |
| GET | /media/{key} | 公开读取允许格式海报，来自R2 |
| GET | /api/favorites | 访客身份，返回公开计数与本访客收藏 |
| POST | /api/favorites | 同源；按作品ID添加/取消收藏 |
| GET | /api/shares | 公开读取最多300条历史投稿，生产数据需单独审核 |
| POST | /api/shares | 禁止匿名投稿，405 |
| POST | /api/certificate | 公开查询精确匹配记录；不返回整表、源表链接或图片 |

## 证书查询
```json
{"douyinId":"review-demo-001"}
```
脱敏索引为空，正常应返回found:false、certificates:[]、imageDownloadAvailable:false；空值或过长ID应400。生产环境无本人身份验证，测试时不可使用他人真实抖音号。

## 收藏
```json
{"id":"demo-skill-001","action":"add"}
```
action可为add/remove。实际测试需先准备相同ID的测试作品；不存在ID应404，缺少正确Origin应403。

## 后台内容
先GET获得当前revision与data，再合并修改，不能猜测revision或覆盖人工草稿。示意：
```json
{"kind":"messages","action":"draft","revision":"<GET取得的版本>","data":[{"title":"测试课程","messages":[{"name":"测试同学","mood":"测试留言","gain":"测试收获","images":[],"videos":[]}]}]}
```
返回新版revision；错误权限403、格式错误400、版本过期409。publish才改变已发布内容；restore与其他动作均由代码中的版本校验约束。

## 建议测试身份
匿名访客、非管理员已登录用户、管理员，分别核对前台可读与后台拒绝/允许。测试账号由公司测试环境维护，不在源码内提供密码、默认管理员或密钥。
