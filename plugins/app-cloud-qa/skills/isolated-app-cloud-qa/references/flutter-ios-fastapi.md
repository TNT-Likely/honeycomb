# Flutter iOS 与 FastAPI 适配要点

## 产物与配置

- 在源码副本修改主包、测试包、widget extension、App Group 和硬编码组名；正式 checkout 的 iOS 配置不改写。移除正式 iCloud/ubiquity entitlement 与 Info 配置。
- 同时读取构建产物的 Info.plist 与实际签名 entitlement。模拟器上也要核验；必要时用 QA entitlement 重新 ad-hoc 签名后再检查。
- `flutter drive --use-application-binary` 可以安装并验证已检查的同一产物；显式指定新 UDID。不要用 `-d booted` 或全局 `simctl shutdown all`。
- `get_app_container` 查不到容器时，区分“首次安装不存在”与“已有应用无法备份”。已有应用需同时备份 data 和 App Group。

## Cloud 初始化

在无 `.env` 的独立 cwd 中执行迁移与启动；对子进程使用环境变量白名单，源码通过固定提交导出，Python 运行时可只读复用。不要先 import 服务再检查数据库路径，模块 import 可能已创建数据库或启动后台任务。

除数据库外检查附件、备份、staging、restore、RAG cache、rclone、静态目录等实际写入位置。禁用与用例无关的备份调度、外部代理和远程 AI；测试凭证随机生成，原始日志私有保存，避免启动日志输出管理员密码。

服务返回本次 run ID 与实际源码身份，App 测试核对 origin、运行时 provider 和 QA 服务标记。临时端口与 PID 都可被复用，停止前核对启动时间与命令。路径检查要拒绝逃逸、直接符号链接及嵌套链接父目录。

## 已遇到的验收陷阱

| 现象 | 核查与处理 |
|---|---|
| 截图里有行，finder 找不到 | 某些虚拟列表未实现 onstage debug traversal。限定目标 widget，使用 `skipOffstage: false` 遍历实际元素，再滚到可见位置并发送触摸 |
| 取消后第二次长按无响应 | Flutter 3.27 live binding 可能丢弃 Navigator 延迟发出的 PointerCancel。独立 QA 设备可启用 `shouldPropagateDevicePointerEvents`，保证原 recognizer 复位；用重复长按证明处理有效 |
| Widget 测试通过、实服保存时 Riverpod 断言 | 嵌套 ProviderScope 覆盖当前账本会影响未声明 scoped dependencies 的 provider；优先沿用生产上下文或显式传参，避免为了测试改全局状态关系 |
| Push 成功但全量快照字段丢失 | 分别核对增量 payload、projection、full snapshot 与 web 部分更新；不要只检查服务响应码 |
| 报告体积异常大 | integration screenshot 回调可能把原始 PNG bytes 留在 reportData，写报告前移除 screenshot 数组，图片另存 |
| 测试结束截图显示桌面 | 备份流程可能已 terminate QA App。正常入口重新启动、检查进程并截图；不能据此直接判断业务崩溃 |

这些处理有版本和适用条件。先检查当前框架/项目实现与日志，不把历史故障当成其他项目的默认行为。
