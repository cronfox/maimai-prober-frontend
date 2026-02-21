# 前端 API 文档

---

本文档列举了查分器前端调用的所有 API 接口。使用个人 API 密钥的接口请求时，需要在请求头中加入：

```
X-User-Token: <个人 API 密钥>
```

使用 Bearer Token（JWT）的接口请求时，需要在请求头中加入：

```
Authorization: Bearer <Token>
```

::: info 提示
本文档中 `{game}` 代表游戏标识符，值为 `maimai` 或 `chunithm`。
:::

## 目录

- [用户认证](#用户认证)
- [用户信息](#用户信息)
- [通行密钥（Passkey）](#通行密钥passkey)
- [OAuth 授权](#oauth-授权)
- [开发者](#开发者)
- [游戏配置](#游戏配置)
- [玩家](#玩家)
- [成绩](#成绩)
- [Best 成绩](#best-成绩)
- [收藏品](#收藏品)
- [年度总结](#年度总结)
- [曲目别名](#曲目别名)
- [成绩评论](#成绩评论)
- [公共接口](#公共接口)
- [管理员](#管理员)

---

## 用户认证

以下接口无需鉴权。

### POST `/api/v0/user/login`

用户登录。

#### 查询参数

| 参数名 | 类型 | 说明 |
|-|-|-|
| `captcha` | `string` | 人机验证 Token |

#### 请求体

| 字段名 | 类型 | 说明 |
|-|-|-|
| `email` | `string` | 邮箱地址 |
| `password` | `string` | 密码 |

#### 响应体

| 字段名 | 类型 | 说明 |
|-|-|-|
| `token` | `string` | JWT Token |

### POST `/api/v0/user/register`

用户注册。

#### 查询参数

| 参数名 | 类型 | 说明 |
|-|-|-|
| `captcha` | `string` | 人机验证 Token |

#### 请求体

| 字段名 | 类型 | 说明 |
|-|-|-|
| `name` | `string` | 用户名 |
| `email` | `string` | 邮箱地址 |
| `password` | `string` | 密码 |

### POST `/api/v0/user/forgot-password`

发送密码重置邮件。

#### 查询参数

| 参数名 | 类型 | 说明 |
|-|-|-|
| `captcha` | `string` | 人机验证 Token |

#### 请求体

| 字段名 | 类型 | 说明 |
|-|-|-|
| `email` | `string` | 邮箱地址 |

### POST `/api/v0/user/reset-password`

重置密码。

#### 查询参数

| 参数名 | 类型 | 说明 |
|-|-|-|
| `token` | `string` | 密码重置 Token（邮件中获取） |

#### 请求体

| 字段名 | 类型 | 说明 |
|-|-|-|
| `password` | `string` | 新密码 |

### GET `/api/v0/user/refresh`

刷新 JWT Token。

#### 响应体

| 字段名 | 类型 | 说明 |
|-|-|-|
| `token` | `string` | 新的 JWT Token |

---

## 用户信息

以下接口需要 `Authorization: Bearer <Token>` 鉴权。

### GET `/api/v0/user/profile`

获取当前用户信息。

#### 响应体

| 字段名 | 类型 | 说明 |
|-|-|-|
| `id` | `int` | 用户 ID |
| `name` | `string` | 用户名 |
| `email` | `string` | 邮箱地址 |
| `bind_qq` | `int` | 值可空，绑定的 QQ 号 |

### POST `/api/v0/user/profile`

更新当前用户信息。

#### 请求体

| 字段名 | 类型 | 说明 |
|-|-|-|
| `name` | `string` | 值可空，新用户名 |
| `email` | `string` | 值可空，新邮箱地址 |

### POST `/api/v0/user/password`

修改密码。

#### 请求体

| 字段名 | 类型 | 说明 |
|-|-|-|
| `old_password` | `string` | 当前密码 |
| `new_password` | `string` | 新密码 |

### POST `/api/v0/user/bind`

更新用户绑定信息（如绑定 QQ 号）。

#### 请求体

| 字段名 | 类型 | 说明 |
|-|-|-|
| `qq` | `int` | 值可空，QQ 号 |

### POST `/api/v0/user/logout`

注销登录（使当前 Token 失效）。

### POST `/api/v0/user/token`

生成个人 API 密钥。

#### 响应体

| 字段名 | 类型 | 说明 |
|-|-|-|
| `token` | `string` | 新生成的个人 API 密钥 |

### GET `/api/v0/user/crawl/token`

获取爬取 Token（用于游戏数据同步）。

#### 响应体

| 字段名 | 类型 | 说明 |
|-|-|-|
| `token` | `string` | 爬取 Token |

### GET `/api/v0/user/crawl/status`

获取当前用户数据爬取状态。

#### 响应体

| 字段名 | 类型 | 说明 |
|-|-|-|
| `status` | `string` | 爬取状态 |

### DELETE `/api/v0/user`

删除当前用户账号。

---

## 通行密钥（Passkey）

以下接口需要 `Authorization: Bearer <Token>` 鉴权。

### GET `/api/v0/user/passkeys`

获取当前用户绑定的所有通行密钥列表。

#### 响应体

| 字段名 | 类型 | 说明 |
|-|-|-|
| `id` | `int` | 通行密钥 ID |
| `name` | `string` | 通行密钥名称 |
| `created_at` | `string` | 创建时间 |

### POST `/api/v0/user/passkeys`

注册新通行密钥（WebAuthn）。

#### 请求体

WebAuthn 注册响应数据。

### PUT `/api/v0/user/passkeys/{id}`

修改通行密钥名称。

#### URL 参数

| 参数名 | 类型 | 说明 |
|-|-|-|
| `id` | `int` | 通行密钥 ID |

#### 请求体

| 字段名 | 类型 | 说明 |
|-|-|-|
| `name` | `string` | 新名称 |

### DELETE `/api/v0/user/passkeys/{id}`

删除通行密钥。

#### URL 参数

| 参数名 | 类型 | 说明 |
|-|-|-|
| `id` | `int` | 通行密钥 ID |

### GET `/api/v0/user/passkeys/challenge`

获取通行密钥注册 Challenge（WebAuthn）。

#### 响应体

WebAuthn 注册选项数据。

### GET `/api/v0/user/passkey/challenge`

获取通行密钥登录 Challenge（WebAuthn，无需鉴权）。

#### 查询参数

| 参数名 | 类型 | 说明 |
|-|-|-|
| `credential_id` | `string` | 值可空，通行密钥凭据 ID |

#### 响应体

WebAuthn 认证选项数据。

### POST `/api/v0/user/passkey/authenticate`

使用通行密钥登录（无需鉴权）。

#### 请求体

WebAuthn 认证响应数据。

#### 响应体

| 字段名 | 类型 | 说明 |
|-|-|-|
| `token` | `string` | JWT Token |

---

## OAuth 授权

以下接口需要 `Authorization: Bearer <Token>` 鉴权。

### GET `/api/v0/user/oauth/authorize/list`

获取当前用户已授权的 OAuth 应用列表。

#### 响应体

[OAuthApp[]](#oauthapp)

### GET `/api/v0/user/oauth/authorize/info`

获取待授权的 OAuth 应用信息。

#### 查询参数

| 参数名 | 类型 | 说明 |
|-|-|-|
| `client_id` | `string` | OAuth 应用客户端 ID |
| `scope` | `string` | 请求的权限范围 |
| `redirect_uri` | `string` | 值可空，回调地址 |

#### 响应体

[OAuthApp](#oauthapp)

### POST `/api/v0/user/oauth/authorize/confirm`

确认授权 OAuth 应用。

#### 请求体

| 字段名 | 类型 | 说明 |
|-|-|-|
| `client_id` | `string` | OAuth 应用客户端 ID |
| `scope` | `string` | 授权的权限范围 |
| `redirect_uri` | `string` | 值可空，回调地址 |

#### 响应体

| 字段名 | 类型 | 说明 |
|-|-|-|
| `code` | `string` | 授权码 |

### DELETE `/api/v0/user/oauth/authorize/app/{client_id}`

撤销对某 OAuth 应用的授权。

#### URL 参数

| 参数名 | 类型 | 说明 |
|-|-|-|
| `client_id` | `string` | OAuth 应用客户端 ID |

---

## 开发者

以下接口需要 `Authorization: Bearer <Token>` 鉴权。

### GET `/api/v0/user/developer/apply`

获取当前用户的开发者信息。

#### 响应体

[Developer](#developer)

### POST `/api/v0/user/developer/apply`

申请成为开发者。

#### 请求体

| 字段名 | 类型 | 说明 |
|-|-|-|
| `name` | `string` | 开发者名称 |
| `reason` | `string` | 申请原因 |

### POST `/api/v0/user/developer/reset`

重置开发者 API 密钥。

#### 响应体

| 字段名 | 类型 | 说明 |
|-|-|-|
| `api_key` | `string` | 新的开发者 API 密钥 |

### PUT `/api/v0/user/developer`

更新开发者信息。

#### 请求体

| 字段名 | 类型 | 说明 |
|-|-|-|
| `name` | `string` | 值可空，开发者名称 |
| `github` | `string` | 值可空，GitHub 主页 |
| `website` | `string` | 值可空，开发者网站 |

### GET `/api/v0/user/developer/oauth/apps`

获取当前开发者创建的 OAuth 应用列表。

#### 响应体

[OAuthApp[]](#oauthapp)

### POST `/api/v0/user/developer/oauth/app`

创建 OAuth 应用。

#### 请求体

| 字段名 | 类型 | 说明 |
|-|-|-|
| `name` | `string` | 应用名称 |
| `description` | `string` | 值可空，应用描述 |
| `redirect_uris` | `string[]` | 回调地址列表 |
| `scopes` | `string[]` | 申请的权限范围列表 |

#### 响应体

[OAuthApp](#oauthapp)

### POST `/api/v0/user/developer/oauth/app/{client_id}`

编辑 OAuth 应用。

#### URL 参数

| 参数名 | 类型 | 说明 |
|-|-|-|
| `client_id` | `string` | OAuth 应用客户端 ID |

#### 请求体

同[创建 OAuth 应用](#post-apiv0userdeveloperoauthapp)请求体。

### DELETE `/api/v0/user/developer/oauth/app/{client_id}`

删除 OAuth 应用。

#### URL 参数

| 参数名 | 类型 | 说明 |
|-|-|-|
| `client_id` | `string` | OAuth 应用客户端 ID |

### POST `/api/v0/user/developer/oauth/upload-app-logo`

上传 OAuth 应用图标。

#### 请求体

`multipart/form-data` 格式，字段名为 `file`，值为图片文件。

---

## 游戏配置

以下接口需要 `Authorization: Bearer <Token>` 鉴权。

### GET `/api/v0/user/{game}/config`

获取当前用户的游戏配置。

#### URL 参数

| 参数名 | 类型 | 说明 |
|-|-|-|
| `game` | `string` | 游戏标识符，值为 `maimai` 或 `chunithm` |

#### 响应体

| 字段名 | 类型 | 说明 |
|-|-|-|
| `allow_third_party_fetch_player` | `bool` | 是否允许第三方获取玩家信息 |
| `allow_third_party_fetch_scores` | `bool` | 是否允许第三方获取成绩 |
| `allow_third_party_fetch_history` | `bool` | 是否允许第三方获取历史记录 |
| `allow_third_party_write_data` | `bool` | 是否允许第三方写入数据 |

### POST `/api/v0/user/{game}/config`

更新当前用户的游戏配置。

#### URL 参数

| 参数名 | 类型 | 说明 |
|-|-|-|
| `game` | `string` | 游戏标识符，值为 `maimai` 或 `chunithm` |

#### 请求体

同[获取游戏配置](#get-apiv0usergameconfig)响应体。

---

## 玩家

以下接口需要 `Authorization: Bearer <Token>` 鉴权。

### GET `/api/v0/user/{game}/player`

获取当前用户绑定的玩家信息。

#### URL 参数

| 参数名 | 类型 | 说明 |
|-|-|-|
| `game` | `string` | 游戏标识符，值为 `maimai` 或 `chunithm` |

#### 响应体

参考对应游戏文档中的 Player 结构体：
- 舞萌 DX：[Player](/docs/api/maimai#player)
- 中二节奏：[Player](/docs/api/chunithm#player)

### PUT `/api/v0/user/{game}/player`

更新当前用户绑定的玩家信息。

#### URL 参数

| 参数名 | 类型 | 说明 |
|-|-|-|
| `game` | `string` | 游戏标识符，值为 `maimai` 或 `chunithm` |

#### 请求体

参考对应游戏文档中的 Player 结构体。

### DELETE `/api/v0/user/{game}/player`

解除当前用户绑定的玩家。

#### URL 参数

| 参数名 | 类型 | 说明 |
|-|-|-|
| `game` | `string` | 游戏标识符，值为 `maimai` 或 `chunithm` |

### GET `/api/v0/user/{game}/player/heatmap`

获取玩家成绩上传热力图。

#### URL 参数

| 参数名 | 类型 | 说明 |
|-|-|-|
| `game` | `string` | 游戏标识符，值为 `maimai` 或 `chunithm` |

#### 响应体

日期与成绩数量的映射，键为日期（格式为 `YYYY-MM-DD`），值为该日期上传的成绩数量。

### GET `/api/v0/user/{game}/player/trend`

获取玩家 Rating 趋势。

#### URL 参数

| 参数名 | 类型 | 说明 |
|-|-|-|
| `game` | `string` | 游戏标识符，值为 `maimai` 或 `chunithm` |

#### 查询参数

| 参数名 | 类型 | 说明 |
|-|-|-|
| `version` | `int` | 值可空，游戏版本 |

#### 响应体

参考对应游戏文档中的 RatingTrend 结构体：
- 舞萌 DX：[RatingTrend](/docs/api/maimai#ratingtrend)

### GET `/api/v0/user/{game}/player/{collection_type}/{id}`

获取玩家指定收藏品的解锁进度。

#### URL 参数

| 参数名 | 类型 | 说明 |
|-|-|-|
| `game` | `string` | 游戏标识符，值为 `maimai` 或 `chunithm` |
| `collection_type` | `string` | 收藏品类型，值为 `trophy`、`icon`、`plate`、`frame` 或 `character` |
| `id` | `int` | 收藏品 ID |

#### 响应体

参考对应游戏文档中的 Collection 结构体：
- 舞萌 DX：[Collection](/docs/api/maimai#collection)
- 中二节奏：[Collection](/docs/api/chunithm#collection)

### GET `/api/v0/user/{game}/player/{collection_type}`

获取玩家拥有的收藏品列表。

#### URL 参数

| 参数名 | 类型 | 说明 |
|-|-|-|
| `game` | `string` | 游戏标识符，值为 `maimai` 或 `chunithm` |
| `collection_type` | `string` | 收藏品类型，值为 `trophies`、`icons`、`plates`、`frames` 或 `characters` |

#### 响应体

Collection[]，参考对应游戏文档中的 Collection 结构体。

---

## 成绩

以下接口需要 `Authorization: Bearer <Token>` 鉴权。

### GET `/api/v0/user/{game}/player/scores`

获取玩家所有最佳成绩。

#### URL 参数

| 参数名 | 类型 | 说明 |
|-|-|-|
| `game` | `string` | 游戏标识符，值为 `maimai` 或 `chunithm` |

#### 响应体

参考对应游戏文档中的 Score 结构体：
- 舞萌 DX：[Score[]](/docs/api/maimai#score)
- 中二节奏：[Score[]](/docs/api/chunithm#score)

### POST `/api/v0/user/{game}/player/scores`

批量导入玩家成绩（JSON 格式）。

#### URL 参数

| 参数名 | 类型 | 说明 |
|-|-|-|
| `game` | `string` | 游戏标识符，值为 `maimai` 或 `chunithm` |

#### 请求体

| 字段名 | 类型 | 说明 |
|-|-|-|
| `scores` | `Score[]` | 玩家成绩列表 |

### DELETE `/api/v0/user/{game}/player/scores`

删除玩家所有成绩。

#### URL 参数

| 参数名 | 类型 | 说明 |
|-|-|-|
| `game` | `string` | 游戏标识符，值为 `maimai` 或 `chunithm` |

### GET `/api/v0/user/{game}/player/scores/export/{type}`

导出玩家成绩文件。

#### URL 参数

| 参数名 | 类型 | 说明 |
|-|-|-|
| `game` | `string` | 游戏标识符，值为 `maimai` 或 `chunithm` |
| `type` | `string` | 导出格式，目前支持 `csv` |

#### 响应体

指定格式的成绩文件。

### POST `/api/v0/user/{game}/player/scores/import`

从文件导入玩家成绩（会覆盖已有成绩）。

#### URL 参数

| 参数名 | 类型 | 说明 |
|-|-|-|
| `game` | `string` | 游戏标识符，值为 `maimai` 或 `chunithm` |

#### 请求体

`multipart/form-data` 格式，字段名为 `file`，值为成绩文件（如 CSV）。

---

## Best 成绩

以下接口需要 `Authorization: Bearer <Token>` 鉴权。

### GET `/api/v0/user/{game}/player/bests`

获取玩家 Best 成绩。

#### URL 参数

| 参数名 | 类型 | 说明 |
|-|-|-|
| `game` | `string` | 游戏标识符，值为 `maimai` 或 `chunithm` |

#### 响应体

- 舞萌 DX：

| 字段名 | 类型 | 说明 |
|-|-|-|
| `standard_total` | `int` | 旧版本谱面 Best 35 总分 |
| `dx_total` | `int` | 现版本谱面 Best 15 总分 |
| `standard` | [`Score[]`](/docs/api/maimai#score) | 旧版本谱面 Best 35 列表 |
| `dx` | [`Score[]`](/docs/api/maimai#score) | 现版本谱面 Best 15 列表 |

- 中二节奏：

| 字段名 | 类型 | 说明 |
|-|-|-|
| `rating` | `float` | 玩家 Rating |
| `bests` | [`Score[]`](/docs/api/chunithm#score) | Best 30 列表 |
| `recents` | [`Score[]`](/docs/api/chunithm#score) | Recent 10 列表 |

---

## 收藏品

### GET `/api/v0/{game}/{collection_type}/list`

获取游戏收藏品列表（无需鉴权）。

#### URL 参数

| 参数名 | 类型 | 说明 |
|-|-|-|
| `game` | `string` | 游戏标识符，值为 `maimai` 或 `chunithm` |
| `collection_type` | `string` | 收藏品类型，值为 `trophy`、`icon`、`plate`、`frame` 或 `character` |

#### 查询参数

| 参数名 | 类型 | 说明 |
|-|-|-|
| `required` | `bool` | 值可空，是否包含解锁要求，默认值为 `false` |

#### 响应体

参考对应游戏文档：
- 舞萌 DX：[获取收藏品列表](/docs/api/maimai#get-apiv0maimaicollection_typelist)
- 中二节奏：[获取收藏品列表](/docs/api/chunithm#get-apiv0chunithmcollection_typelist)

### GET `/api/v0/{game}/song-collections/{song_id}`

获取指定曲目关联的收藏品列表（无需鉴权）。

#### URL 参数

| 参数名 | 类型 | 说明 |
|-|-|-|
| `game` | `string` | 游戏标识符，值为 `maimai` 或 `chunithm` |
| `song_id` | `int` | 曲目 ID |

#### 响应体

Collection[]，参考对应游戏文档中的 Collection 结构体。

---

## 年度总结

### GET `/api/v0/user/{game}/player/year-in-review/{year}`

获取当前用户的年度总结（需要 `Authorization: Bearer <Token>` 鉴权）。

#### URL 参数

| 参数名 | 类型 | 说明 |
|-|-|-|
| `game` | `string` | 游戏标识符，值为 `maimai` 或 `chunithm` |
| `year` | `int` | 年份 |

#### 查询参数

| 参数名 | 类型 | 说明 |
|-|-|-|
| `agree` | `bool` | 值可空，是否同意生成年度总结，默认值为 `false` |

### POST `/api/v0/user/{game}/player/year-in-review/{year}/share`

创建年度总结分享链接（需要 `Authorization: Bearer <Token>` 鉴权）。

#### URL 参数

| 参数名 | 类型 | 说明 |
|-|-|-|
| `game` | `string` | 游戏标识符，值为 `maimai` 或 `chunithm` |
| `year` | `int` | 年份 |

#### 请求体

| 字段名 | 类型 | 说明 |
|-|-|-|
| `public` | `bool` | 是否公开分享 |

#### 响应体

| 字段名 | 类型 | 说明 |
|-|-|-|
| `token` | `string` | 分享 Token |

### GET `/api/v0/{game}/year-in-review/{year}/share/{token}`

通过分享 Token 获取年度总结（无需鉴权）。

#### URL 参数

| 参数名 | 类型 | 说明 |
|-|-|-|
| `game` | `string` | 游戏标识符，值为 `maimai` 或 `chunithm` |
| `year` | `int` | 年份 |
| `token` | `string` | 分享 Token |

---

## 曲目别名

以下有鉴权要求的接口需要 `Authorization: Bearer <Token>` 鉴权。

### GET `/api/v0/user/{game}/alias/list`

获取曲目别名列表（含审核状态，需鉴权）。

#### URL 参数

| 参数名 | 类型 | 说明 |
|-|-|-|
| `game` | `string` | 游戏标识符，值为 `maimai` 或 `chunithm` |

#### 查询参数

| 参数名 | 类型 | 说明 |
|-|-|-|
| `page` | `int` | 页码，从 `1` 开始 |
| `sort` | `string` | 排序字段与方向，如 `alias_id desc` |
| `approved` | `bool` | 值可空，是否仅返回已审核通过的别名 |
| `song_id` | `int` | 值可空，按曲目 ID 筛选 |

#### 响应体

| 字段名 | 类型 | 说明 |
|-|-|-|
| `aliases` | `Alias[]` | 曲目别名列表 |
| `page_count` | `int` | 总页数 |
| `page_size` | `int` | 每页数量 |

### GET `/api/v0/user/{game}/alias/votes`

获取当前用户的曲目别名投票记录（需鉴权）。

#### URL 参数

| 参数名 | 类型 | 说明 |
|-|-|-|
| `game` | `string` | 游戏标识符，值为 `maimai` 或 `chunithm` |

#### 响应体

| 字段名 | 类型 | 说明 |
|-|-|-|
| `alias_id` | `int` | 别名 ID |
| `vote` | `bool` | 投票方向，`true` 为赞成，`false` 为反对 |

### POST `/api/v0/user/{game}/alias`

提交新的曲目别名（需鉴权）。

#### URL 参数

| 参数名 | 类型 | 说明 |
|-|-|-|
| `game` | `string` | 游戏标识符，值为 `maimai` 或 `chunithm` |

#### 请求体

| 字段名 | 类型 | 说明 |
|-|-|-|
| `song_id` | `int` | 曲目 ID |
| `alias` | `string` | 别名内容 |

### POST `/api/v0/user/{game}/alias/{alias_id}/vote/{direction}`

对曲目别名进行投票（需鉴权）。

#### URL 参数

| 参数名 | 类型 | 说明 |
|-|-|-|
| `game` | `string` | 游戏标识符，值为 `maimai` 或 `chunithm` |
| `alias_id` | `int` | 别名 ID |
| `direction` | `string` | 投票方向，值为 `up` 或 `down` |

### DELETE `/api/v0/user/{game}/alias/{alias_id}`

删除当前用户提交的曲目别名（需鉴权）。

#### URL 参数

| 参数名 | 类型 | 说明 |
|-|-|-|
| `game` | `string` | 游戏标识符，值为 `maimai` 或 `chunithm` |
| `alias_id` | `int` | 别名 ID |

---

## 成绩评论

以下接口需要 `Authorization: Bearer <Token>` 鉴权。

### GET `/api/v0/user/{game}/comment/list`

获取成绩评论列表。

#### URL 参数

| 参数名 | 类型 | 说明 |
|-|-|-|
| `game` | `string` | 游戏标识符，值为 `maimai` 或 `chunithm` |

#### 查询参数

| 参数名 | 类型 | 说明 |
|-|-|-|
| `song_id` | `int` | 曲目 ID |
| `level_index` | `int` | 难度 |
| `song_type` | `string` | 值可空，谱面类型 |

#### 响应体

| 字段名 | 类型 | 说明 |
|-|-|-|
| `comment_id` | `int` | 评论 ID |
| `comment` | `string` | 值可空，评论内容 |
| `rating` | `int` | 值可空，评分 |
| `is_liked` | `bool` | 当前用户是否已点赞 |
| `like_count` | `int` | 点赞数量 |
| `upload_time` | `string` | 发表时间 |
| `uploader` | `object` | 发表用户信息 |
| `uploader.id` | `int` | 用户 ID |
| `uploader.name` | `string` | 用户名 |
| `uploader.avatar_id` | `int` | 头像 ID |

### POST `/api/v0/user/{game}/comment`

发表成绩评论。

#### URL 参数

| 参数名 | 类型 | 说明 |
|-|-|-|
| `game` | `string` | 游戏标识符，值为 `maimai` 或 `chunithm` |

#### 请求体

| 字段名 | 类型 | 说明 |
|-|-|-|
| `song_id` | `int` | 曲目 ID |
| `song_type` | `string` | 谱面类型 |
| `level_index` | `int` | 难度 |
| `comment` | `string` | 值可空，评论内容 |
| `rating` | `int` | 值可空，评分 |

### DELETE `/api/v0/user/{game}/comment/{comment_id}`

删除成绩评论。

#### URL 参数

| 参数名 | 类型 | 说明 |
|-|-|-|
| `game` | `string` | 游戏标识符，值为 `maimai` 或 `chunithm` |
| `comment_id` | `int` | 评论 ID |

### POST `/api/v0/user/{game}/comment/{comment_id}/like`

点赞评论。

#### URL 参数

| 参数名 | 类型 | 说明 |
|-|-|-|
| `game` | `string` | 游戏标识符，值为 `maimai` 或 `chunithm` |
| `comment_id` | `int` | 评论 ID |

### DELETE `/api/v0/user/{game}/comment/{comment_id}/like`

取消点赞评论。

#### URL 参数

| 参数名 | 类型 | 说明 |
|-|-|-|
| `game` | `string` | 游戏标识符，值为 `maimai` 或 `chunithm` |
| `comment_id` | `int` | 评论 ID |

---

## 公共接口

以下接口无需鉴权。

### GET `/api/v0/site/config`

获取查分器站点配置（如资源版本、资源哈希等）。

#### 响应体

| 字段名 | 类型 | 说明 |
|-|-|-|
| `resource_version` | `object` | 各游戏资源版本，键为游戏标识符，值为版本号 |
| `resource_hashes` | `object` | 各游戏各资源文件的哈希，键为游戏标识符，值为资源哈希映射 |

### GET `/api/v0/{game}/crawl/statistic`

获取指定游戏的爬取统计信息。

#### URL 参数

| 参数名 | 类型 | 说明 |
|-|-|-|
| `game` | `string` | 游戏标识符，值为 `maimai` 或 `chunithm` |

---

## 管理员

以下接口需要 `Authorization: Bearer <Token>` 鉴权，且当前用户须具有管理员权限。

### GET `/api/v0/user/admin/users`

获取所有用户列表。

### DELETE `/api/v0/user/admin/users`

批量删除用户。

#### 请求体

| 字段名 | 类型 | 说明 |
|-|-|-|
| `ids` | `int[]` | 要删除的用户 ID 列表 |

### POST `/api/v0/user/admin/email`

向用户发送批量邮件。

#### 请求体

| 字段名 | 类型 | 说明 |
|-|-|-|
| `subject` | `string` | 邮件主题 |
| `content` | `string` | 邮件内容 |
| `user_ids` | `int[]` | 值可空，接收用户 ID 列表，不填则发送给所有用户 |

### POST `/api/v0/user/admin/user/{user_id}`

更新指定用户信息。

#### URL 参数

| 参数名 | 类型 | 说明 |
|-|-|-|
| `user_id` | `int` | 用户 ID |

### DELETE `/api/v0/user/admin/user/{user_id}`

删除指定用户。

#### URL 参数

| 参数名 | 类型 | 说明 |
|-|-|-|
| `user_id` | `int` | 用户 ID |

### GET `/api/v0/user/admin/developers`

获取所有开发者列表。

### DELETE `/api/v0/user/admin/developer`

撤销开发者资质。

#### 请求体

| 字段名 | 类型 | 说明 |
|-|-|-|
| `user_id` | `int` | 要撤销资质的用户 ID |

### DELETE `/api/v0/user/admin/{game}/alias/{alias_id}`

管理员删除曲目别名。

#### URL 参数

| 参数名 | 类型 | 说明 |
|-|-|-|
| `game` | `string` | 游戏标识符，值为 `maimai` 或 `chunithm` |
| `alias_id` | `int` | 别名 ID |

### POST `/api/v0/user/admin/{game}/alias/{alias_id}/approve`

管理员审核通过曲目别名。

#### URL 参数

| 参数名 | 类型 | 说明 |
|-|-|-|
| `game` | `string` | 游戏标识符，值为 `maimai` 或 `chunithm` |
| `alias_id` | `int` | 别名 ID |

---

## 结构体

### OAuthApp

OAuth 应用

| 字段名 | 类型 | 说明 |
|-|-|-|
| `client_id` | `string` | 客户端 ID |
| `name` | `string` | 应用名称 |
| `description` | `string` | 值可空，应用描述 |
| `logo` | `string` | 值可空，应用图标 URL |
| `scopes` | `string[]` | 权限范围列表 |
| `redirect_uris` | `string[]` | 回调地址列表 |

### Developer

开发者信息

| 字段名 | 类型 | 说明 |
|-|-|-|
| `name` | `string` | 开发者名称 |
| `github` | `string` | 值可空，GitHub 主页 |
| `website` | `string` | 值可空，开发者网站 |
| `api_key` | `string` | 开发者 API 密钥 |
