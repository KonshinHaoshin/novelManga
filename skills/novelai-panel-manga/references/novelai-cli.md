# novelai-cli 实测工作流

以下接口依据本机 `novelai-cli 0.1.4`。使用前读 `novelai-cli generate --help`；新版本字段不同则以 dry-run 和实际接口为准。此参考只指导已获用户授权的图片测试。

## 请求

`--request` 接收 **JSON 文件路径**，不是内联 JSON。使用 UTF-8 无 BOM 文件，写入实际换行；不要把 `\n` 当作要显示的字样。图片请求与 API 底层 payload 是两种结构，不能混用。

```json
{
  "model": "nai-diffusion-5-full",
  "prompt": "2girls, manga, monochrome. Exactly two panels, top then bottom. Text: 你好！\n\n再见！",
  "negativePrompt": "lowres, bad quality, watermark",
  "characters": [
    {"prompt": "In the top panel: girl, pale hair. She says \"你好！\".", "negativePrompt": "", "x": 0.5, "y": 0.25},
    {"prompt": "In the bottom panel: girl, dark hair. She says \"再见！\".", "negativePrompt": "", "x": 0.5, "y": 0.75}
  ],
  "useCharacterCoords": true,
  "qualityToggle": false,
  "ucPreset": 2,
  "width": 832,
  "height": 1216,
  "steps": 28,
  "scale": 5,
  "seed": 123456,
  "nSamples": 1
}
```

在此 CLI 版本中，位置启用字段是 `useCharacterCoords`，不是 `useCoords`；字符槽字段是 `characters`，不是 `characterPrompts`。槽内位置是顶层 `x`/`y`，不是 `center`。槽内负向是 `negativePrompt`，`uc` 会被忽略。使用坐标时必须确认 payload 的 `use_coords` 为真。`ucPreset: 2` 用于不自动加入默认 UC；检查实际版本和请求产物，避免预设压制漫画。

## 先 dry-run

```powershell
novelai-cli generate --request 'C:\absolute\request.json' --dry-run
```

确认输出：

- `model` 是选定的 V5 模型，实际 `input` 与 `parameters.v4_prompt.caption.base_caption` 保留所有文字。
- 即使 V5，结构仍叫 `v4_prompt`；`caption.char_captions` 数量和顺序等于计划槽位，`char_caption` 内容没有丢失。
- 对灵活版式，另外保留画格到槽位的映射。检查的是槽位数，不要求它等于总格数；插入格可用物体槽，同格多人也可拆成多个槽。内部记录的 `panels`、`bbox`、`parent` 不能直接加进 CLI 请求，它们不是此接口的布局参数。
- 如果启用位置，`parameters.use_coords` 与 `v4_prompt.use_coords` 都为真，`centers` 与计划一致。
- `qualityToggle` 为假、`ucPreset` 为 2，手写负向不压制对白、分格或网点。
- seed、尺寸、采样数符合测试计划。先 1 张，记录参数；不要通过套用 GUI 预设引入不明风格、文字负向或额外采样。

`--dry-run` 不需要凭证，不联网，不生图。把它通过只检查标准输出的脚本保存为审计用 JSON，可与原请求对照。它通过只证明参数结构成立。

## 生图与检查

```powershell
novelai-cli generate --request 'C:\absolute\request.json' --output-dir 'C:\absolute\outputs'
```

使用 CLI 已配置凭证；不执行 `token` 的显示/导出、不打印环境或凭证文件。若 GUI 可用而 CLI 报 `API Token is not configured`，先用 `novelai-cli token status` 查看仅含布尔值的状态；必要时通过正常工具授权在沙箱外重复该命令。相同 CLI 的 `savedToken` 若在沙箱内为假、外部为真，说明沙箱阻止了凭证读取，不应要求用户重新设置 token。此时可在用户已授权的生图范围内，经工具授权执行原请求，不需要查看 token。只有排除此类差异后再调查版本或配置来源。

连接失败时报告具体失败，不关 TLS 验证来掩盖问题。需要沙箱网络授权时走正常工具授权。

看生成图并检查：准确格数（分别数主格与插入格）、大小格与跨行关系、插入格是否嵌在指定宿主、阅读顺序；每格人物/物体对应；同一人物跨格外貌/服装；动作是否串格；气泡归属和类型；逐句中文有无漏字、错字或重复。气泡尾与人物视线应符合对话。仅仅把六个等大格排出来，不能算通过了非均分版式检查。

把视觉检查逐项标为通过/部分通过/失败/无法确认；没有 OCR 或看不清时不能声称逐字正确。保留实际图像路径、请求、dry-run payload、seed 和结果说明。参数验证与图像效果分别报告。

初次样图失败时，只针对一个已观察到的问题做一轮修订，尽量固定 seed 和其他设置。布局复杂或中文长句仍失真，交付可复现请求和具体限制，建议缩短文字/调整格子/后期排字；不要无限重试或说“已稳定解决”。新一轮修改超出用户当前测试范围时再确认。
