# NovelAI 分镜漫画技能测试

日期：2026-11-01。技能：`novelai-panel-manga`。

## 已验证

- 技能 frontmatter、命名与 scaffold 检查通过 `quick_validate.py`。
- 所有技能文件为 UTF-8；相对引用存在；UI 元数据可解析。
- 请求 dry-run：V5 Full、4 个槽、A/A/B/A 人物映射、坐标开关、seed 和原台词保留，负向没有压制漫画或文字。
- 两次实际生成的 `generation.json.request` 分别与对应 dry-run payload 一致。
- 两张图片均经目视检查。准确四格，顺序为上方通栏 → 中左小格 → 中右竖格 → 下方通栏。浅发旅人在 1/2/4 格出现，深发伤者在第 3 格出现。

## 实际生图

共同参数：`novelai-cli 0.1.4`，`nai-diffusion-5-full`，832×1216，28 steps，scale 5，seed 20261101，1 sample，坐标启用，qualityToggle=false，ucPreset=2。

| 轮次 | 请求与 payload | 图像 | 结果 |
|---|---|---|---|
| 初版 | [request.json](request.json) / [payload.json](payload.json) | [图片](outputs/novelai-1793521755652827400-47484/novelai-0.png) | 分镜和人物对应通过；末格长对白目视漏掉“得好重！” |
| 一次修订 | [request-revision-1.json](request-revision-1.json) / [payload-revision-1.json](payload-revision-1.json) | [图片](outputs-revision-1/novelai-1793521890999238700-13616/novelai-0.png) | 保留四格与人物对应；末格对白目视完整 |

修订只改变末格对白在主提示词中的内部换行，以及末格槽内的气泡区域/横排说明。移除新增换行后，台词与初版逐字一致。seed、其他槽和采样设置相同。第二张其他区域也随提示词改变，整页文字转为横排。

两次生成目录均保存有 CLI 返回的 `generation.json`，包括实际请求、图像路径和设置；图像条目的 seed 为 null，但实际请求的 parameters.seed 为 20261101。

## GUI 可用而 CLI 报未配置凭证

同一 CLI 的 `token status`：沙箱内 environmentToken=false、savedToken=false；经工具授权在沙箱外执行时 environmentToken=false、savedToken=true。由此确认沙箱阻止了已保存凭证读取，无需重新配置 token。之后在沙箱外完成上述两次生图。未读取或输出 token。

## 未验证

没有 OCR 逐字对照，未验证字体与所有标点完全还原、角色原设外貌还原、跨页一致性或跨 seed 的稳定成功率。两张均支持当前版式与人物对应，但不构成严格网格保证。CLI 参数通过与实际图像效果分开评估。

## 复现修订版

```powershell
novelai-cli generate --request 'G:\git\novelManga\artifacts\bamboo-four-panel\request-revision-1.json' --dry-run
novelai-cli generate --request 'G:\git\novelManga\artifacts\bamboo-four-panel\request-revision-1.json' --output-dir 'G:\git\novelManga\artifacts\bamboo-four-panel\outputs-revision-1'
```
