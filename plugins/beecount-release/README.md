# beecount-release

BeeCount(蜜蜂记账)全家桶的发版流水线技能。

一次发版横跨四个仓:App tag → CI 构建多端产物;Cloud tag → Docker 镜像;changelog 双格式(App Store + Google Play);官网文档同步;可选的宣传视频与社媒文案。本技能把 3.4.0 / 1.4.0 那次完整发版的实操沉淀为可重复流程。

## 技能

- **release** — 完整流水线:预检 → 用户确认门(范围 + 版本号)→ Cloud/App 打 tag → changelog → 官网 → 视频文案 → 收尾核验。

## 设计要点

- **确认在前、tag 在后**:版本号与发布范围必须用户拍板,打 tag 前命令原样过目。
- **Cloud 先于 App**:版本偏斜规则,生产环境先有新镜像可升。
- **商店文案红线**:Google Play 段不含 iOS 条目;商店不写自部署/运维项。
