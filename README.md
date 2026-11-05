# TVBox 自建源

把多个公开配置里**真正能用的直连站点**合并成一个自己的配置地址，省去在几十个失效站点里反复翻找。

## 这个源是怎么来的

只收 `type 0/1` 且 `api` 是 http 地址的**直连 CMS 站点**。`type 3` 的爬虫站一律不要——它们的代码在播放器壳里，换个 JSON 就失效，搬不走。

每条站点都要过三关：

1. `?ac=videolist&pg=1` 是否真返回片库（不是空列表）
2. 取详情能否拿到 `m3u8` 播放地址
3. 连续拉若干分片能否下载成功

## 怎么用（电视端）

在 TVBox / 影视仓的「设置 → 配置地址」里填：

```
https://gh-proxy.com/https://raw.githubusercontent.com/<你的用户名>/<仓库名>/main/tvbox.json
```

`gh-proxy.com` 是国内可用的免费镜像前缀，实测 1.6 秒返回；不加前缀的 `raw.githubusercontent.com` 直连会超时。

备用前缀（主前缀失效时换这个）：

```
https://cdn.jsdelivr.net/gh/<你的用户名>/<仓库名>@main/tvbox.json
```

## 维护

`.github/workflows/rebuild.yml` 每天 UTC 03:30（北京 11:30）跑一次 `scripts/build.py`，自动重测、重写 `tvbox.json` 并提交。公开仓库的 Actions 是免费的，不需要服务器。

想立刻重跑：仓库页面 → Actions 标签 → 左侧 `rebuild-tvbox-source` → 右侧 `Run workflow`。

## 说明

- 源会持续死（域名过期、限流、换 CDN），所以自动重建比手工整理一次更重要。
- 站点全部是第三方采集接口，没有版权授权，仅供个人自用，不要公开传播或商用。
- 本仓库只做"可用性筛选"，不托管任何影视内容。
