# flutter-riverpod-drift

脚手架 Claude Code 插件:基于 [BeeCount](https://github.com/TNT-Likely/BeeCount) 的生产实践,一键生成 **Flutter + Riverpod + Drift** 的成熟 app 骨架,带 Design Token 系统、Repository 多端切换、严格 Drift 迁移纪律、PrimaryHeader/SectionCard 统一页面骨架等模式。

## 这个插件能做什么

跟 Claude 说一句"按 flutter-riverpod-drift 起一个新 Flutter 项目叫 Foo",它会:

1. 问清几个关键参数(项目名、bundle id、是否要云同步、是否要 i18n 等)
2. 执行 `flutter create` 起空壳,然后清掉默认 `lib/` 重建成 BeeCount 风格的目录树
3. 生成 Design Token 系统(`BeeTokens` / `BeeDimens` / `BeeShadows` / `BeeTextTokens`)、`PrimaryHeader` 替代 AppBar、`SectionCard` 卡片容器、`showToast` 替代 SnackBar、`LoggerService` 替代 print、`.scaled()` 响应式扩展
4. 搭好 Drift 数据库骨架(单文件 schema + 严格 `MigrationStrategy.onUpgrade`)+ Repository 抽象层(base interface → local 实现 → 可选 cloud 实现)
5. 配 Riverpod providers 按 domain 拆分的目录约定
6. 跑通 `dart run build_runner build -d` → `flutter pub get` → `flutter run` 一遍自检
7. 给出"还需要你手动做的 5 件事"清单(主题色定制、`.arb` 翻译、签名配置、CI workflows 模板等)

## 安装

```bash
/plugin marketplace add TNT-Likely/honeycomb
/plugin install flutter-riverpod-drift@honeycomb
```

## 适用场景

适合:个人 / 小团队的 Flutter app(SaaS、工具、内容、社交)、有云同步需求、需要本地 DB 缓存、看重设计一致性。BeeCount 本身已经在 iOS / Android 双端跑了 v0.x → v1.x 多次发版,这套架构是验证过的。

不适合:纯 Flutter web 网站(SQLite-on-web 还要适配)、需要 BLoC 的团队、用 GetX/Provider 的项目、非常简单的 demo(脚手架略重)。

## 技术栈

| 层 | 选型 | 为什么 |
|---|------|--------|
| State Mgmt | flutter_riverpod | 编译期类型安全,无 BuildContext 耦合,代码生成可选 |
| Local DB | Drift + sqlite3_flutter_libs | type-safe SQL DSL,迁移工具链完善,Stream 监听原生支持 |
| 设计系统 | 自建 Design Token(`BeeTokens` + `BeeDimens` + `BeeShadows`)| 比 Material `ThemeData` 更细粒度,亮/暗模式自动适配 |
| 页面骨架 | `PrimaryHeader` + `SectionCard` 模式 | 替代 AppBar+ListTile 千篇一律的 Material 默认 |
| 路由 | Navigator 1.0 + MaterialPageRoute | 不上 go_router,绝大多数 app 用不到深链 |
| Cloud(可选) | supabase_flutter / 自建 REST | Repository 抽象层在中间,具体后端可换 |
| i18n | flutter_localizations + `.arb` | 官方方案,`flutter gen-l10n` 跑一次出代码 |
| 日志 | 自建 `LoggerService` 全局实例 | 持久化 + 应用内查看 + 原生桥接,优于 print |
| 响应式 | `.scaled(context, ref)` 扩展 | 用户字体设置 + 屏幕尺寸双因子缩放 |

## 设计原则

- **Repository 是数据访问唯一入口**:UI 永远走 Provider,Provider 内部用 Repository,Repository 才是真正访问 DB / Cloud。业务代码绝不该出现 `if (appMode == cloud)` 的判断
- **Drift 迁移纪律**:所有 schema 变更只走 `db.dart` 的 `MigrationStrategy.onUpgrade`,禁止启动代码里 raw SQL 建表,禁止 Drift 外的 `schema_migrations` 表
- **Token 优先**:UI 颜色/间距/圆角/阴影/排印全部走 Token,不直接用 `Colors.grey` / 字面量
- **缓存 + Stream 双阶段**:列表页用 splash 预加载缓存秒开,然后 `switchToStreamMode()` 切到实时 Stream
- **统一日志**:`logger.info('TAG', '...')`,禁用 `print` / `debugPrint`

## 反模式提示

skill 文档里专门列了 11 条反模式(❌ `AppBar` / ❌ `SnackBar` / ❌ `print` / ❌ `Colors.grey` / ❌ UI 直接调 Repository / ❌ 业务代码判 cloud / ❌ Drift 外的迁移 ... )和"BeeCount 有但新项目不要默认抄"的部分(账本/交易模型、共享账本、同步引擎、AI Kit、home_widget 等)。

## 详情

完整 skill 内容见 [`skills/scaffold/SKILL.md`](./skills/scaffold/SKILL.md)。
