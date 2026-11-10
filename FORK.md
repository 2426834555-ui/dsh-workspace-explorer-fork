# dsh-workspace-explorer-fork

右侧「工作区文件」面板：目录树、点击/拖拽插入文件引用、文件名与内容搜索、预览、编辑保存。

> 另一个独立插件：[dsh-devproc](https://github.com/2426834555-ui/dsh-devproc)（输入框上方的开发进程胶囊）。
> 两者互不依赖，各自克隆、各自安装。两个都装，界面才和作者本机一致：胶囊在上，文件树在右。

基于上游 [Jiyr0119/dsh-workspace-explorer](https://github.com/Jiyr0119/dsh-workspace-explorer) 0.10.0 的 fork，上游 MIT。

## 相对上游的改动

| 改动 | 位置 | 说明 |
| --- | --- | --- |
| 打开文件直接进编辑态 | `src/client/index.tsx` 的 `openPreview` | 打开文件后自动进入编辑，不用再点「编辑」。想恢复只读：控制台执行 `localStorage.setItem('dshwe.prefs', JSON.stringify({autoEdit:false}))` 再刷新 |
| 编辑区 `Ctrl/Cmd+S` 保存 | 编辑 textarea 的 `onKeyDown` | 有未保存改动且不在保存中时调用 `saveFile()` |
| 包名去作用域 | `package.json` / `tsdown.config.ts` / `cordis.patch.yml` | `@jiyr0119/dsh-workspace-explorer` → `dsh-workspace-explorer`，避免与 npm 上的原版冲突 |

改动都在 **TS 源码**里（不是往 `lib/` 产物里打补丁），所以 `npm run build` 后 `lib/` 自然带上。

> 编辑区是原生 `textarea`，没有语法高亮 / 自动补全 / 跳转定义——那是上游的设计，本 fork 没动。

## 依赖

- Node.js 20+
- DSH 桌面版（profile 目录 `~/.dsh/profiles/<profile>`，本机是 `desktop`）
- 构建工具来自 devDependencies（tsdown + typescript + lightningcss），`npm install` 会装

## 安装

```powershell
# 1) 克隆
git clone https://github.com/2426834555-ui/dsh-workspace-explorer-fork.git `
  "$HOME\.dsh\plugins\dsh-workspace-explorer-fork"

# 2) 必须构建：这是 TS 源码仓库，没有 lib/ 装了也加载不出来
cd "$HOME\.dsh\plugins\dsh-workspace-explorer-fork"
npm install --legacy-peer-deps     # peer 依赖版本冲突，必须带这个参数
npm run build                      # 产出 lib/index.js + lib/client.js

# 3) 链接进 profile（装过 npm 原版的话先摘掉）
cd "$HOME\.dsh\profiles\desktop"
pnpm remove @jiyr0119/dsh-workspace-explorer
pnpm add "dsh-workspace-explorer@link:../../plugins/dsh-workspace-explorer-fork"
```

4. 打开 `$HOME\.dsh\profiles\desktop\package.json`，在 `dsh.profile.bundles` 里把
   `"@jiyr0119/dsh-workspace-explorer"` 换成 `"dsh-workspace-explorer"`（没有就加一行）。

5. 检查 `$HOME\.dsh\profiles\desktop\cordis.patch.yml`，若有

   ```yaml
   - insert:
       - id: workspace-explorer
         name: '@jiyr0119/dsh-workspace-explorer'
   ```

   把 `name` 改成 `'dsh-workspace-explorer'`，否则 profile 会去 npm 装回原版。

6. 重启 DSH，右侧出现「工作区文件」面板。

## 两个插件一起装

```powershell
git clone https://github.com/2426834555-ui/dsh-devproc.git "$HOME\.dsh\plugins\dsh-devproc"
git clone https://github.com/2426834555-ui/dsh-workspace-explorer-fork.git `
  "$HOME\.dsh\plugins\dsh-workspace-explorer-fork"

cd "$HOME\.dsh\plugins\dsh-workspace-explorer-fork"
npm install --legacy-peer-deps
npm run build

cd "$HOME\.dsh\profiles\desktop"
pnpm add "dsh-devproc@link:../../plugins/dsh-devproc"
pnpm add "dsh-workspace-explorer@link:../../plugins/dsh-workspace-explorer-fork"
```

`dsh.profile.bundles` 里加两行：

```json
"dsh-devproc",
"dsh-workspace-explorer"
```

## 开发

```powershell
npm run dev        # 监听构建 + 自动部署
npm run typecheck  # 类型检查
npm run build      # 正式构建
```

改了源码要重新 `npm run build` 再刷新页面；DSH 加载的是 `lib/`，不是 `src/`。

## 上游原版

不想用 fork 的话，npm 上的原版是 `@jiyr0119/dsh-workspace-explorer`，功能等同，只是少了上表三处改动。

## License

MIT，`LICENSE` 原样保留自上游。发现上游的 bug 或想分享这些改动，建议直接给上游提 PR。
