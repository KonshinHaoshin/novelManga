# 官方依据与经验边界

核对日期：2026-11-01。以下为摘要，不是独立 API 规范；版本变化时核对对应页面。

- [Multi-Character Prompting](https://docs.novelai.net/en/image/multiplecharacters/)：共享主提示词与独立角色槽、顺序与坐标、主提示词的人数标签、`|` 替代语法。槽内使用无数字的 girl/boy。V4/V4.5 自定义位置为 5×5 网格；V5 更自由。
- [Text Rendering](https://docs.novelai.net/en/image/textrendering/)：`Text:` 置于主提示词末尾，文字条目以空行分隔；本格自然语言可以重复原文协助归属。V5 支持中文；V4/V4.5 官方仅支持英文。含空白/换行的文字长度上限分别为 118、374（V5 Curated）、750（V5 Full）。
- [V5 官方发布说明](https://journal.novelai.net/image-generation-novelai-diffusion-v5-is-here-c2df7c6b8d2d/)：自然语言版式描述和角色位置可引导整页分格，支持更复杂漫画；`high complexity` 为 V5 复杂度标签。网页对引号文字的自动处理属于前端便利，CLI 不应假定有同样预处理。
- [Quality Tags](https://docs.novelai.net/en/image/qualitytags/)：自动质量词可能包含 `no text`；短字/音效受抑制时可关闭，并手动加质量词。
- [Undesired Content](https://docs.novelai.net/en/image/undesiredcontent/)：部分预设包含 `multiple views`、`screentone`、`comic` 等；漫画需要审查其冲突。

**经验性编排**：一格一槽、同一人物跨槽重复身份锚点、两个人物四个槽仍保留 `2girls`，是面向本任务的提示词约定；官方没有提供“第 N 槽必定生成第 N 个矩形格”的保证。坐标定位的是提示词主体，不是画格边界。样图检查必须独立于 schema/dry-run 校验。
