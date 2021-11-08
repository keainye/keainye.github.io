---
title: 自用短链接服务
date: 2021/11/08 13:34
---

**7nm.co**

现在 5 个字母以内又好听不贵的域名已经不好找了，找了一晚上才找到这一个 qwq

见过了许多短域名（b23.tv, 3.cn, t.cn）后，觉得短链接服务确实挺帅的，于是决定自己搭建一个短链接服务。恰好最近在看 VTB，七海 nanami，就买了个 `7nm.co`，不仅是七海的缩写，还有 7 纳米的含义。

## 设计概览

这个短链接服务完全部署在 Cloudflare Workers 中，我相信 Cloud Flare 对于并发的处理肯定比我自己的服务器强 ~~才不是懒得备案~~ ，并且 Cloudflare Workers KV 这个设计我还是挺满意的。

整个短链接服务分为两个程序（[ShortLink](https://github.com/Kininaru/short-link), [ShortLinkApi](https://github.com/Kininaru/short-link-api)），使用了两个 KV 命名空间。其中，`ShortLink` 是提供跳转的主程序；`ShortLinkApi` 是控制程序，负责生成、修改、删除、覆盖短链接。

## 安装

出于法律问题，这个短链接服务我仅打算自用。但是代码是完全公开的，并且与 `7nm.co` 没有强耦合。如果你在使用过程中有任何问题，或者发现了任何 bug，请发邮件至 [me@mail.kininaru.dev](mailto:me@mail.kininaru.dev)。如需部署，请按照以下步骤配置。

假设你需要为 `example.com` 配置短链接服务，并在 `api.example.com` 控制这个服务，那么你需要：

1. fork [ShortLink](https://github.com/Kininaru/short-link) 和 [ShortLinkApi](https://github.com/Kininaru/short-link-api)
2. 在 [https://dash.cloudflare.com/profile/api-tokens](https://dash.cloudflare.com/profile/api-tokens) 使用 `编辑 Cloudflare Workers` 为模板创建一个令牌。
3. 在 fork 的两个仓库中创建名为 `CF_API_TOKEN` 的 Actions Secrets，并且将上一步创建的令牌作为值填入。
4. 在 Cloudflare -> Workers -> KV 中添加两个命名空间，`ShortLink` 和 `ShortLinkApi`，并且记下它们的 id。
5. 修改 fork 之后的两个仓库根目录下的 `wrangler.toml` 文件，将 `kv_namespaces` 中 `ShortLink` 和 `ShortLinkApi` 对应的 id 改成上一步中记下的 id。
6. 在 `ShortLinkApi` 这个 KV 命名空间中，加入一个键 `code`，值设置为只有你自己知道字符串，我们称之为 **短链接控制令牌**。这个令牌在安装的时候用不到，但是在使用时会用到。
7. `commit` 这次更改至两个仓库。如果没出现问题，此时 GitHub Actions 应该被启动，然后将这两个 Workers 添加至你的 Cloudflare 账户。
8. 配置你的域名，添加 `example.com` 和 `api.example.com` 两个解析，启用 Cloudfalre 代理。
9. 添加 `example.com/*` 的 Workers 路由至 `short-link`，添加 `api.example.com/*` 的 Workers 路由至 `short-link-api`（如果没有出现这两个 Workers，请刷新页面，或者检查 GitHub Actions 输出结果）。

## 接口

在成功搭建后，Cloudflare Workers 就会自动维护 `example.com/*` 和 `api.example.com/*` 的所有请求。前者会自动 302 跳转所有有记录的链接，例如 [7nm.co/ZwVk9a](https://7nm.co/ZwVk9a) 会自动跳转到本文；后者用于这些记录的增删改。

我们在 **api.example.com/manage** 提供了一个添加短链接的快捷方法。目前界面十分简陋（[api.7nm.co/manage](https://api.7nm.co/manage)），并且只有生成短链接的功能，因为我暂时不需要其他功能 ~~才不是因为我懒~~。

打开这个链接后会出现一个页面，此时你需要打开浏览器终端，然后输入 `localStorage.setItem("code", "请填入你的短链接控制令牌");`，执行，然后就可以使用了。需要注意的是，你需要保证这个链接是一个以 `http` 或者 `https` 开头的合法链接，否则后端会拒绝生成链接。

虽然前端界面很简陋，但后端 API 较为完善，可以提供以下功能。

>  如果没有特殊说明，以下所有 API 请求都要添加 `Authorization` 请求头，值为上文你自定的**短链接控制令牌**。

**api.example.com/add**

- 从一个普通链接生成一个短链接
- `POST`

- request body 应该是一个 JSON，形如

  ```json
  { "link": "https://another.example.com/urlpath" }
  ```

- 返回值是一个 JSON

  ```json
  { "code": 0, "msg": "/FL44zE"}
  ```

  `code` 为 0 时则添加成功，`msg` 为短链接的 `urlpath`。在 `urlpath` 前加上 `example.com` 就能 302 跳转。

  `code` 为 1 时添加失败，失败原因会存在 `msg` 中，目前有 `"link error"` 和 `"no extra space"`，前者意味着你提交的链接格式非法，后者说明在生成短链接的时候，发生了两次 MD5 碰撞，需要增加链接长度。

  如果你遇到了后者这种情况，请给我发邮件，我会第一时间处理。同时，如果你有更好的短链接算法，也欢迎交流或者 PR。

- 如果你提交了一个已生成过的普通链接，则会返回之前生成的短链接链接。

**api.example.com/delete**

- 删除一个短链接记录

- `POST`

  其实这里应该使用 `DELETE`，更符合 HTTP 协议的语义。在后文设计思想中我会对这一点进行讨论。

- request body 应该是一个 JSON，形如

  ```json
  { "shortLink": "/FL44zE" }
  ```

- 返回值是一个 JSON

  ```json
  { "code": 0 }
  ```

  一般来说，我们保证这次删除操作一定能够成功，所以理论上不存在例外情况。

**api.example.com/set**

- 自主定一个短链接记录，如果记录已存在，则会返回失败

- `POST`

- request body 应该是一个 JSON，形如

  ```json
  {
      "shortLink": "/special",
      "link": "https:/special.example.com/links"
  }
  ```

  语义很明显了，这里不做解释。

- 返回值是一个 JSON

  ```json
  { "code": 0 }
  ```

  0 代表设定成功，1 代表短链接已存在。

- 通过这个 API，你可以设置一些极短的特殊链接，比如 [7nm.co/h](https://7nm.co/h) 就可以极快地访问这个博客。当然，直接修改名为 `ShortLink` 的 Cloudflare KV 里面的映射也可以实现同样的效果。

**api.example.com/alive**

- 检测服务器是否在线

- 任意 HTTP method

- 返回值是一个 JSON，当它的值是

  ```json
  {
      "code": 0,
      "msg": "ShortApi ctrl server."
  }
  ```

  时，我们认为服务器运作正常。

## 设计

**ShortLink**

这个转发程序十分简单，核心仅包含这几行代码

```javascript
let path = "/" + request.url.split("/")[3];
let forwardLink = await ShortLink.get(path);
if (forwardLink === null) return new Response("Link not found.", {status: 404});
console.log(`${path},${forwardLink},${Date.now()}`);
return Response.redirect(forwardLink, 302);
```

简单来说，这个转发端会获取每个请求的 `urlpath`，然后在 `ShortLink` 这个 KV 空间中寻找一个用于跳转的链接。如果找到了，就 302 跳转过去；如果没找到，就返回 404 错误。

为什么用 302 跳转？因为这样就可以精确地统计每次访问。未来可以通过分析 Cloudflare 的日志来获取访问信息。并且这样设计还有针对搜索引擎策略的考虑。

**ShortLinkApi**

这个程序负责增删改转发映射规则，还提供了一个前端管理界面。

所有控制信息均存在 **ShortLinkApi** 这个 KV 命名空间中（目前只有一个 `code` 负责控制身份），可以自行修改。程序在收到请求之后，首先判断 path 是不是 `/manage`，如果是，就直接返回静态页面，如果不是，就判断请求头中 `Authorization` 的值是否正确，如果不正确就返回 403，如果正确就允许此次操作。

为了简化设计，这个程序并没有过多地分析收到的请求。对于所有的请求，程序会先将它们的请求体变成一个 JavaScript 对象：

```javascript
let body = await request.json();
```

然后接下来的所有逻辑都依赖这个 `body` 对象。这也是我在 `delete` API 中使用 POST 请求的原因，因为 DELETE 请求并没有请求体。

关于 `ShortLinkApi` 剩余的部分没啥可以说的，就是一个 `switch` 负责调用请求对应的函数，然后执行并返回。

**生成短链接的算法**

1. 收到了一个链接，检查这个链接是否合法
2. 对链接进行 MD5 计算，得到一个长度为 16 的 `Uint8Array`
3. 取上一步中数组的前 6 位，对 62（10 + 26 + 26）取余之后变成 `0 - 9, A - Z, a - z` 中对应的字符，然后在这个字符串前面加上 `\`
4. 取上一步得到的字符串，查看是否存在。如果：
   - 不存在：返回这个字符串
   - 存在，但是对应的长链接相同：返回这个字符串
   - 存在，但是对应的长链接不同：取第二步中数组的后 6 位，进行第三步，取得一个字符串，再进行本校验步骤

第 4 步的重复校验只能容忍一次重复。如果两次都碰撞了，则会要求管理员更改代码增加一位短链接。

我对这个短链接算法并不是很满意，因为对于碰撞的处理太草率了，应该设计容错率更高的碰撞算法。但是 Cloudflare Worker 只能运行 10ms，查询 KV 相对来说非常耗时，所以我这么设计了算法。不过这个算法也有一个好处，那就是当重复提交一个长链接时，这个算法会返回已有的短链接，不会占用额外的空间。

如果你有好的算法，欢迎 PR，或者与通过邮件我讨论。当然你也可以在我的留言板里面写下你的意见，留言板的地址见博客底部。

## 最终

我是脆鲨，我永远喜欢 nanami！

![nana7mi](https://images.chromium.link/c8d01e4cee21632345fbce222c63b21b489eab7a.png@518w.webp)

