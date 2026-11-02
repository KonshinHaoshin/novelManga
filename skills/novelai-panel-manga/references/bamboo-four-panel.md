# 竹林相遇：两个人物，四个槽

这是用户提供的示例，演示不等大小分格与重复角色。人物 A 为 `chihaya_anon`、浅色头发、采药旅人；人物 B 为 `togawa_sakiko`、深色散发、血染白衣。角色 tag 用作外貌起点，不继承现代校服。

| 格 | 槽 | 人物 | 位置 | 叙事 / 文字类型 | 可选主体中心 |
|---|---|---|---|---|---|
| 1 | char1 | A | 上方通栏 | 悠闲采药 / 内心话 | (0.50, 0.16) |
| 2 | char2 | A | 中左小格 | 发现血泊 / 滴答音效 | (0.25, 0.49) |
| 3 | char3 | B | 中右竖格 | 负伤戒备 / 锯齿对白 | (0.75, 0.50) |
| 4 | char4 | A | 下方通栏 | 俯身搭话 / 普通对白 | (0.50, 0.84) |

阅读顺序：上 → 中左 → 中右 → 下。不是四列纵排，也不是四个独立人物。

## 主提示词

```text
2girls, manga, monochrome, high complexity, text, chinese text, very aesthetic, amazing quality. A single Chinese xianxia manga page set in a misty bamboo forest, with exactly four clearly separated panels and white gutters. One wide horizontal panel spans the entire top; beneath it are one small panel on the middle-left and one tall vertical panel on the middle-right; one wide horizontal panel spans the entire bottom. Read the top panel first, then middle-left, then middle-right, then the bottom panel. The recurring pale-haired traveler is the same person in panels 1, 2 and 4; the dark-haired wounded woman appears in panel 3. The cast consists of only these two people. All dialogue and thought bubbles contain Chinese text.
Text: 采完这株七星草，换了下品灵石就能去山下喝仙酿、摸鱼躺平啦～

滴答……

……滚开。别碰我。

哇！
这位仙子姐姐伤得好重！
别逞强啦，
遇到我算你走运哦！
```

## Character 1 — 上方通栏 / A

```text
In the wide top panel, wide shot, eye-level view: girl, chihaya_anon, pale hair, simple travel robes, a worn bamboo basket on her back, a cheap sword sheathed at her waist. She walks leisurely through the misty bamboo forest, biting a blade of grass; bamboo sways in the strong wind. A thought bubble belonging to the pale-haired traveler reads "采完这株七星草，换了下品灵石就能去山下喝仙酿、摸鱼躺平啦～".
```

## Character 2 — 中左小格 / A

```text
In the small middle-left panel, medium shot from a downward angle, with her face and stepping foot both in view: girl, chihaya_anon, pale hair, simple travel robes, a worn bamboo basket on her back, a cheap sword sheathed at her waist. The pale-haired traveler looks down in shock, a sweat drop on her cheek, one foot stepping into a dark puddle of blood on the forest ground. Small sound-effect lettering near the puddle reads "滴答……".
```

原始描述的 medium close-up 难以同时容纳脸和脚，此处只把该格调整为中景俯拍以保留动作。用户要求保留特写时，可建议改为脚部特写或同格局部插入，不私自增加第五格。

## Character 3 — 中右竖格 / B

```text
In the tall vertical middle-right panel, medium shot: girl, togawa_sakiko, dark hair loose and disheveled, pale frail face, bloodstained white robes. The wounded woman slumps against gnarled tree roots in the bamboo forest, weak but guarded, eyes half-open with a sharp cold glare. A jagged speech bubble belonging to the wounded dark-haired woman reads "……滚开。别碰我。".
```

## Character 4 — 下方通栏 / A

```text
In the wide bottom panel, tight facial close-up: girl, chihaya_anon, pale hair, the collar of her simple travel robes visible. The pale-haired traveler crouches down close to the wounded woman's eye level, with wide sparkling curious eyes and a warm energetic smile. Her face fills the panel. The composition reserves a large white speech bubble in the right third of this panel for the pale-haired traveler. Her entire utterance is written legibly in four short horizontal lines, preserving every word including the phrase about the severe injury. The speech bubble reads exactly "哇！
这位仙子姐姐伤得好重！
别逞强啦，
遇到我算你走运哦！".
```

## 负向

```text
lowres, worst quality, bad quality, jpeg artifacts, bad anatomy, bad hands, watermark, signature
```

以上示例保持原台词，气泡分别为内心话、音效、戒备对白、热情对白。人物衣服增加了最低限度的仙侠旅装锚点，避免角色 tag 拉回校服；若用户已指定服装应替换。坐标仅供辅助定位，版式仍靠主提示词和各槽的区域说明。

## 实测边界

2026-11-01 使用 `novelai-cli 0.1.4`、`nai-diffusion-5-full`、832×1216、28 steps、scale 5、seed 20261101，启用表中坐标，关闭自动质量与默认 UC，先后生成两张。两张均生成四格，上/中左/中右/下的布局和 A/A/B/A 出场对应成立。

初版末格未预留气泡区域、对白没有内部换行，目视发现漏掉“得好重！”。以上版本改为右侧留白与四行排字，第二张中该段对白目视完整。修改也影响了整页排字方向；相同 seed 不代表其他像素或排字保持不变。未做 OCR 逐字校验，也未验证跨 seed 或多次生成的成功率。
