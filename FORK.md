# dsh-workspace-explorer-fork

> 这是**两个独立插件之二**。另一个是开发进程胶囊：[dsh-devproc](https://github.com/2426834555-ui/dsh-devproc)。
> 两者互不依赖，各自克隆、各自安装；一起装才和作者的界面一致（胶囊在输入框上方，文件树在右侧面板）。

基于上游 [Jiyr0119/dsh-workspace-explorer](https://github.com/Jiyr0119/dsh-workspace-explorer) 0.10.0 的 fork。

## 安装（换电脑照抄）

```powershell
# 1) 克隆
git clone https://github.com/2426834555-ui/dsh-workspace-explorer-fork.git "$HOME\.dsh\plugins\dsh-workspace-explorer-fork"

# 2) 因为是 TS 源码仓库，要先装依赖并构建出 lib/
cd "$HOME\.dsh\plugins\dsh-workspace-explorer-fork"
npm install --legacy-peer-deps     # 或 pnpm install
npm run build                      # 生成 lib/index.js + lib/client.js

# 3) 装进 profile（包名仍是 dsh-workspace-explorer）
cd "$HOME\.dsh\profiles\desktop"
pnpm remove @jiyr0119/dsh-workspace-explorer   # 若装过原版
pnpm add "dsh-workspace-explorer@link:../../plugins/dsh-workspace-explorer-fork"
```

然后在 `$HOME\.dsh\profiles\desktop\package.json` 的 `dsh.profile.bundles` 里把
`"@jiyr0119/dsh-workspace-explorer"` 换成 `"dsh-workspace-explorer"`，重启 DSH。

> 注意：`cordis.patch.yml` 里原本可能写着 `name: '@jiyr0119/dsh-workspace-explorer'`，
> 需要同步改成 `dsh-workspace-explorer`，否则 profile 会去 npm 装回原版。

## 相对上游的改动

| 改动 | 位置 | 说明 |
| --- | --- | --- |
| 打开文件直接进编辑态 | `openPreview` | 打开文件后自动 `enterEditMode()`，不用再点「编辑」。可用 `localStorage.setItem('dshwe.prefs', JSON.stringify({autoEdit:false}))` 关掉 |
| 编辑区 `Ctrl/Cmd+S` 保存 | 编辑 textarea 的 `onKeyDown` | 脏且不在保存中时调用 `saveFile()` |

这两处都是**源码级**改动（不是往 `lib/` 产物里打补丁），因此 `npm run build` 后 `lib/` 会自然带上。

## 安装（换电脑用这个）

```powershell
git clone <你的 fork 地址> "$HOME\.dsh\plugins\dsh-workspace-explorer-fork"
cd "$HOME\.dsh\plugins\dsh-workspace-explorer-fork"
npm install --legacy-peer-deps
npm run build

# 让当前 profile 用这份构建产物
cd "$HOME\.dsh\profiles\desktop"
pnpm remove @jiyr0119/dsh-workspace-explorer
pnpm add "dsh-workspace-explorer@link:../../plugins/dsh-workspace-explorer-fork"
# 再把 package.json 里 dsh.profile.bundles 的 @jiyr0119/dsh-workspace-explorer 换成 dsh-workspace-explorer
```

## 说明

- 上游为 MIT，本 fork 保持 MIT，`LICENSE` 原样保留。
- 只用于本地；要分发给别人建议直接给上游提 PR（这两个功能本身很小）。
