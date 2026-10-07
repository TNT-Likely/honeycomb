# BeeCount 项目适配

App 仓维护 `scripts/qa/isolated_app_cloud.py`、`scripts/qa/test_isolation.py`、`integration_test/transaction_copy_live_test.dart` 和 `docs/contributing/ISOLATED_APP_CLOUD_QA_ZH.md`。执行前读项目 guide 与当前 runner，以下命令中的路径和 ref 按本次 checkout 替换。

```sh
python3 -m unittest discover -s scripts/qa -p 'test_*.py' -v
python3 scripts/qa/isolated_app_cloud.py prepare \
  --cloud-repo ../BeeCount-Cloud --cloud-ref <已提交的分支或SHA> \
  --skill-repo ../honeycomb --flutter <Flutter可执行文件>
```

`prepare` 打印本次私有 run 路径、新 UDID 和 loopback Cloud origin。使用实际打印路径，不复制以前运行的值。

```sh
qa_run_dir='<本次打印的run目录>'
python3 scripts/qa/isolated_app_cloud.py build --run "$qa_run_dir" --flutter <Flutter可执行文件>
python3 scripts/qa/isolated_app_cloud.py preflight --run "$qa_run_dir"
python3 scripts/qa/isolated_app_cloud.py run --run "$qa_run_dir" --flutter <Flutter可执行文件>
python3 scripts/qa/isolated_app_cloud.py restart-check --run "$qa_run_dir" --flutter <Flutter可执行文件>
python3 scripts/qa/isolated_app_cloud.py stop --run "$qa_run_dir"
```

默认 Cloud ref 为 `origin/main`，本地 checkout 分支不切换。联调修复时显式指定 ref，manifest 记录实际 SHA。Cloud checkout 的 `.venv/bin/python` 只读复用；数据库迁移在新目录执行。runtime 参数取本机已安装版本，默认值不代表本机一定可用。

## 本适配的身份和证据

QA 主包 `com.tntlikely.beecount.qa`，扩展 `.qa.BeeCountWidgetExtension`，App Group `group.com.tntlikely.beecount.qa`；所有已有模拟器受保护。产物身份不符时不可安装。

`evidence/` 下：逐项 `acceptance.json`、真实 Cloud `cloud-projection.json`、`environment.json`、正常入口 `restart-persistence.json` 和合成数据截图。原始日志、credentials/env、构建产物及 `private-backups/` 留在私有 run，不复制到 PR。

首页复制用例覆盖：实际长按菜单与预填、取消后再次复制、独立新身份、编辑备注与重复保存、当前日期、标签/账户/标记/币种、收入/转账余额/外币、原附件及周期关系不继承、首页切换账本、服务 503 后恢复、真实 owner/editor 邀请与共享资源引用、Cloud web 写 API 修改后拉回、多次同步及正常入口持久化。

权限阻断用例修改的是 QA 本地已知角色与资源镜像；不能宣称已测服务端撤销成员。服务 503 不能宣称已测飞行模式。Android、真机和 web 前端需要独立适配，当前 iOS 用例不能替代。
