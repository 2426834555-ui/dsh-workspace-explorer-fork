# 本地 fork 说明（相对上游 @jiyr0119/dsh-workspace-explorer 0.10.0）

本仓库是 [Jiyr0119/dsh-workspace-explorer](https://github.com/Jiyr0119/dsh-workspace-explorer) 的个人 fork，
只保留一个提交，改动集中在 `src/client/index.tsx`：

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
