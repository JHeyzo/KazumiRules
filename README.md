# KazumiRules（优化版）

基于上游 [Predidit/KazumiRules](https://github.com/Predidit/KazumiRules)（MIT License，署名保留，见 LICENSE）的优化复刻，只做有把握的修复和删减，不重写未实测的 XPath / baseURL。

## 在 Kazumi 里添加

设置 → 规则仓库 → 添加仓库地址，填：

```
https://raw.githubusercontent.com/JHeyzo/KazumiRules/main/
```

`index.json` 就在仓库根目录。添加后在规则管理里安装需要的规则即可。

注意：HLS 广告过滤（`adBlocker`）需要 Kazumi 1.9.3 及以上（API Level 5+），旧版 Kazumi 请先升级，否则该开关可能不生效。

## 本次优化清单（2026-11-08 核查）

- `adBlocker` 开成 `true`（原来是 false / 缺失，API 等级 ≥5，支持 HLS 广告过滤）：LMM、moonci、ezdmw、giriGiriLove、xfdmnext。
- `7sefun`、`akianime`：`api` 由 `"4"` 升到 `"5"`，同时开 `adBlocker: true`。这两条原来是 API Level 4，开广告过滤需要 Level 5。
- 其余 XPath、baseURL、searchURL、版本号一律不动，没有实测页面 HTML 就不重写选择器。

## 删掉的规则及原因

| 规则 | 原因 |
| --- | --- |
| aafun | 与 moonci 同站（base 都是 `www.moonci.com`），且 `chapterRoads` 是 `"//"`，属重复 + 损坏规则，留 moonci 即可。 |
| xfdmneo | 同一 Xifan 站，已有更新的 API 版 xfdmnext；xfdmneo 是 XPath + `useLegacyParser`、2026-03-08 旧版，属被取代的重复规则。 |
| TvTFun | 核查失败：首页 `www.tvtfun.net` 域名无法解析，搜索 API `www.tvtfun.net/api/client/anime` 同样 DNS / 连接失败，两次独立核查均失败，按“只推能用的”删掉。 |
| dalvdm | 核查失败：首页 `www.sbdl.cc` 和搜索 API `index.php/ajax/suggest` 两次均返回 403（服务器有响应、域名可解析，但拒绝了核查方的访问）。这可能是反爬，不代表站已死，但无法核实可用，按“只推能用的”标准不推。 |
| sorani | 2026-11-08 实机核查：搜得到，但选集 API 里 5 集的 `playUrl` 全空，播放页显示“暂无数据/请先登录”，站公告称部分地区无法播放，Kazumi 解析不到 m3u8，播不了。 |
| mgnacg | 2026-11-08 实机核查：搜得到，但详情页地址打开是首页列表、无简介无选集，`sourceTemplate` 拼不出真正的详情页，Kazumi 抓不到播放列表，播不了。 |
| modu（魔都资源） | 2026-11-08 实机核查：资源不全，删掉。 |
| wujin（无尽资源） | 2026-11-08 实机核查：太卡，播不动，删掉。 |

## DM84 备用域名

DM84 的 `baseURL` 未改，仍是 `https://dmbus.cc/`——该站实测能打开。站内公告称永久网址是 `dm84.vip`、发布页是 `dm84.pro`，只写在这里备用，未验证过 `dm84.vip`，dmbus.cc 打不开时可再试。

## 保留的 14 条规则及核查结果

核查方法：2026-11-08 对每条规则的 baseURL（XPath 规则）和搜索 API 地址（API 规则）逐个用 browser.open 核查。标注说明：“搜索 API 已验证”指用 `wd=test` 等关键词实际调了接口并返回了正常 JSON；“仅首页可达，搜索未实测”指只打开了首页，未直接验证搜索接口（例如 POST 接口无法用 GET 核查）。

| 规则 | 核查结果 |
| --- | --- |
| 7sefun | 首页可达，番剧列表正常；站内提示“域名已更换，部分用户可能需要挂梯才能播放”。仅首页可达，搜索未实测。 |
| AGE | 首页可达，最近更新到 2026-11-07。仅首页可达，搜索未实测（返回的是 20 小时前缓存）。 |
| akianime | 首页可达，番剧列表正常。仅首页可达，搜索未实测。 |
| baimao | 首页可达，番剧列表正常。仅首页可达，搜索未实测。 |
| DM84 | 首页 `dmbus.cc` 可达（实机浏览器核实），番剧列表正常。仅首页可达，搜索未实测。 |
| ezdmw | 首页 `m.ezdmw.org` 可达，番剧列表正常。仅首页可达，搜索未实测。 |
| giriGiriLove | 搜索 API 已验证：`m3u8.girigirilove.com/api.php/Vod/get_list` 返回 `code:1`，共 4 条。首页 `ani.girigirilove.com` 在核查方地区被挡（“暂不支持该地区访问”），但搜索 API 本身可用。 |
| LMM | 首页可达；搜索 API 已验证：`index.php/ajax/suggest` 返回 `code:1`（`test` 无匹配，`list:[]`，属正常空结果）。 |
| moonci | 首页可达（实机浏览器核实），番剧列表正常。搜索 API 用爬虫核查时域名无法解析，属核查方问题、非站死证据，仅首页可达，搜索未实测。 |
| mutefun | 首页可达（返回的是 13 天前缓存），番剧列表正常；该规则自带反爬 + 验证码，解析天然慢，慎当主力。仅首页可达，搜索未实测。 |
| MXdm | 首页 `www.dcc3.com` 可达，番剧列表正常。仅首页可达，搜索未实测。 |
| xfdmnext | 首页 `next.xifanacg.com` 可达，番剧列表正常。搜索接口是 POST（`api.xifanacg.com`），无法用 GET 直接核查，仅首页可达，搜索未实测。 |
| guangsu（光速资源） | MacCMS 采集站 API 规则：搜索 + 选集都是 API，`api.guangsuapi.com/api.php/provide/vod/`，已验证：搜“葬送的芙莉莲” total:2，选集 `ac=detail` 直接给 m3u8。两条线，选 `gsm3u8` 线（直链），别选 `gsyun` 云播线。字幕未证实（API 返回无字幕字段），待手机实测。 |
| mdzy（魔都新域） | MacCMS 采集站 API 规则：魔都资源新域名 `www.mdzyapi.com/api.php/provide/vod/`（老域名 caiji.moduapi.cc 的 modu 规则已删），单条 `modum3u8` 线。已验证：搜“葬送的芙莉莲” total:2，S1 全 28 集；浏览器真放第 1 集能播，1080p，有烧进画面的中文字幕（对白纯中文，带“(沪)剧审字(2023)第0458号”），片头有烧进画面的赌博广告（582316.com）。 |

## 速度表（诚实标注局限）

browser.open 不返回每次请求的毫秒耗时，所以下面没有编出来的毫秒数，只有按核查结果分的档。所有一次成功的打开都是在单次调用内返回的，没有出现超过 10 秒才返回或超时的情况（失败的那两条是立即报错，不是超时）。

| 档 | 规则 | 测的是哪个地址 |
| --- | --- | --- |
| 快 | giriGiriLove、LMM、guangsu、mdzy | 搜索 API 直接返回了正常 JSON |
| 中 | 7sefun、AGE、akianime、baimao、DM84、ezdmw、moonci、MXdm、xfdmnext | 首页一次打开，列表正常 |
| 慢，慎用 | mutefun | 首页是 13 天前缓存 + 自带反爬/验证码；7sefun 站内自述部分用户需挂梯 |
| 失败，已删 | TvTFun、dalvdm、sorani、mgnacg、modu、wujin | 见上表删掉的原因 |

重要：这个速度是在服务器侧测的，只能做相对排序，不能代表用户手机上走日本节点时的真实速度；最终以手机实测为准。

## 来源与许可

- 上游规则：Predidit/KazumiRules，MIT License。
- 本仓库只做上述修复和删减，规则本身的版权 / 责任归原站和上游所有。
