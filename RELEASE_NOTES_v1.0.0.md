# PaperMirror v1.0.0

PaperMirror 首个正式版。写英文论文、检查修改时，可以用中文对照阅读，并查看修改前后的公式。

- 修改、新增和删除的内容会留下标记，连续保存也不会清掉前面的改动。点 `Show before` 可以对照之前的中文或公式。
- 点击英文源码，中文预览会跳到对应位置。用 `Prev` / `Next` 可以逐处检查改动，检查完点 `Clear marks` 开始下一轮。
- 修复段落错配、部分改动漏标、外部工具修改文件后预览不刷新，以及清除后旧标记重新出现的问题。
- 改善公式显示、逐段重译、停止翻译和接口失败后的重试。
- 首次设置集中到中文页面：必填项有提示，模型可以直接修改，密钥可从服务商官方入口获取。高级参数默认收起，保存并测试成功后即可打开预览。
- 商店名称和简介加入中文，插件 ID 和原商店链接保持不变。
- README 加入 8 秒演示，按日常使用、首次设置、常见问题的顺序整理说明。

改动标记按段落、公式等内容块记录，重载编辑器后会重新开始比较。大幅重写的段落可能显示为删除和新增。

## 安装

VS Code 用户可在扩展面板更新 PaperMirror。Cursor 用户可下载本页的 `papermirror-vscode-1.0.0.vsix`，运行 `Extensions: Install from VSIX...` 安装。

## English

The first stable release adds persistent revision markers and earlier-text/formula comparisons, fixes change matching and preview refresh issues, and improves translation retry and stop behavior. The README includes an eight-second demo and shorter usage and setup instructions.

The Chinese setup page includes required-field hints, editable models, official API-key links, secure key storage and connection testing. Advanced options are collapsed. Store metadata now includes Chinese wording while keeping the existing extension ID.

Changes are compared at block level within the current preview session. Reloading the editor resets the comparison; substantial rewrites may appear as deletion and addition.

## Verification

- Build and TypeScript checks passed.
- 99 core tests and 100 extension tests passed.
- The Chinese setup page was checked in an isolated headless browser, including editable models, official key-page actions and required-field errors. Provider results and browser opens were simulated; no real credentials were used.
- The change-mark fixes were checked in an isolated VS Code window, including external saves, earlier Chinese and equations, Clear, and re-enabling markers.
- The packaged README and runtime files were checked against the release sources.
