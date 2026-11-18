# 已从版本控制移除的产物/数据

> 这些路径在 `plus` 上已 `git rm --cached`，**文件仍保留在磁盘**；`main`（上游快照）仍保留跟踪。
> 目的：仓库只保留代码、文档、工具与必要配置，避免二进制/缓存/历史数据膨胀。
> 上游同步按 `AGENTS.md` 0.0 的“手工处理”执行，不做 `git merge upstream/main` 整体合并。

## 已移除

| 路径 | 大小量级 | 原因 |
|---|---:|---|
| `DFO-115US单机一键启动器.exe` | ~63 MB | 发布用二进制，随启动器自更新变化 |
| `gm-tool/configs/`（保留 `tech_debt.md`） | ~130 MB | 旧 GM 工具的历史导出数据，可由 PVF/索引重建 |
| `analysis/dumps/`（保留 `README.md`、`CLIENT-MECHANICS.md`） | ~32 MB | 逆向取证中间产物，脚本可复现 |
| `pvf_cache/unwrap.json` | 小 | 本机绝对路径缓存清单，不可移植 |
| `server/work/dfo_probe_tools/probe.exe` | ~0.6 MB | 构建产物；`probe.cpp` 源码保留 |
| `tools/tools-server-configs.zip` | ~0.7 MB | 压缩包产物；内容可重建 |

## 例外（保留跟踪）

- `server/work/dfo-lan/runtime/*_ok.bin`：runtime 状态标记。门禁 `§0.3.1` 对 `runtime` 路径一律阻断（删除也会命中），因此保留跟踪；它们极小，不构成体积问题。

## 保留

- 源码、测试、脚本、文档；
- `server/work/dfo-lan/configs/`（服务端运行所需配置/策略）；
- `server/work/dfo-lan/internal/testfixture/testdata/`（测试夹具）；
- `server/work/dfo-lan/cmd/wireprobe/testdata/`、协议模板 `.bin`（测试/协议数据）；
- `analysis/AGENTS.md`、`analysis/dumps/README.md`、`analysis/dumps/CLIENT-MECHANICS.md`；
- `gm-tool/README.md` 等文档与脚本。

## 恢复跟踪

需要把某个路径重新纳入版本控制时：

```powershell
git add -f <path>
git commit -m "chore: 恢复跟踪 <path>"
```

`.gitignore` 已对这些路径加了忽略规则；`-f` 可强制加入。

## 上游处理

上游若修改了这些路径：

1. `git fetch upstream`
2. `git log --oneline main..upstream/main`
3. `git diff main..upstream/main -- <path>`
4. 按实际 diff 手工应用（`git show` / `git apply` / 手改），不要整体 merge。
