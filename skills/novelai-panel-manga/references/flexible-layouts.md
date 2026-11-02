# 灵活漫画整页

## 先安排叙事，再安排格子

把场景中的建立环境、动作、反应、细节揭示、重点台词分开。重要的表演或揭示分配较大面积；短促反应和物件细节可以较小。对白少不代表必须小格，停顿也可用大格表达。保留原对白与先后，允许把同一句的两个语义段放到相邻格；没有必要时不扩写剧情。

| 形式 | 适用作用 | 英文编排措辞 |
|---|---|---|
| 通栏横格 | 开场环境、长动作、结尾停顿 | `a wide panel spanning the full page width` |
| 跨行竖格 | 站姿、纵向动作、持续反应 | `a tall panel on the left spanning the combined height of the two stacked panels on its right` |
| 大小格混排 | 区分主次、改变节奏 | `a large reaction panel beside two smaller detail panels` |
| 插入格 | 手机、手部、眼睛等局部 | `a small bordered inset embedded in the upper-left corner of the large bottom panel` |
| 出血格 | 需要延伸到纸边的画面 | `the final panel extends to the bottom and right edges of the page` |
| 倾斜格线 | 强动作、冲击瞬间 | `two adjacent panels separated by a slanted gutter` |
| 破框 | 局部动作或人物超出原边界 | `her hand extends beyond the border of its panel into the gutter` |

这些措辞是软性引导，不是图形布局命令。选与剧情相关的形式组合，不把所有形式塞进一页。插入格与破框的计数不同：手机另有封闭小框时是额外一格；手伸出原画框仍属于原格。

## 编排记录与位置

复杂版式可先记录：`id / order / bbox=[left,top,right,bottom] / parent / role / content / slots`。坐标范围 0–1，供自己核对空间。它不是 CLI schema，也不会强制模型按边界裁切。

- 普通格彼此不重叠；插入格显式归属宿主并位于其内部。出血表示接触页面边缘，不表示无框画面可以盖住邻格。
- 倾斜格用外接边界框辅助规划，但不能把边界框校验当作真实斜边的验证。
- 主体中心必须落在自己的格内；宿主格的主脸、关键手势和气泡应避开插入格。主体落点不一定是格子几何中心。
- 阅读路线单独写清。跨行格未必跟着从上到下逐行排序；插入格要说明读在宿主对白之前还是之后。主提示词和 `Text:` 顺序遵循同一路线。
- 同格多人需要两个槽时，两槽指向同一画格，不能增加主提示词中的格数；同一人物重复出场也不能增加故事人物数。

无需每次输出完整坐标 JSON。用户看到的简洁草图/分镜表及各槽位置通常足够。用户要求严格复刻版式时，先说明提示词的边界；若纯生图未达标，可建议版式参考或逐格生成后排版，不声称坐标能锁定四边。

## 例：把《练习成果》改为六格整页

人物与原对白沿用爱音/素世。把“自拍数量”和“意识到说漏嘴”拆成相邻两格，再加一个无字手机插入格；没有新增对白。总计 **五个主格＋一个插入格＝六格**。

```text
┌────────────────────────────────┐
│ P1：通栏，爱音炫耀手酸          │
├───────────┬────────────────────┤
│           │ P3：横小格，脱口而出│
│ P2：素世  ├────────────────────┤
│ 跨行竖格  │ P4：横小格，突然心虚│
├───────────┴────────────────────┤
│ ┌──────────┐                   │
│ │ P5：手机 │  P6：素世的大特写  │
│ │ 无字插格 │  ＋最后一句吐槽    │
│ └──────────┘                   │
└────────────────────────────────┘
```

阅读顺序 P1→P2→P3→P4→P5→P6；P5 嵌在 P6 左上角，先揭示手机自拍，再读素世的吐槽。编号只用于说明，不印在页面上。

| 画格 | 边界框 | 内容 / 槽位 | 主体落点 |
|---|---|---|---|
| P1 | [0.04,0.04,0.96,0.25] | 爱音 / char1 | (0.35,0.15) |
| P2 | [0.04,0.28,0.32,0.60] | 素世 / char2 | (0.18,0.44) |
| P3 | [0.35,0.28,0.96,0.43] | 爱音 / char3 | (0.58,0.35) |
| P4 | [0.35,0.46,0.96,0.60] | 爱音 / char4 | (0.60,0.53) |
| P5 | [0.06,0.65,0.28,0.79]，parent=P6 | 手机 / char5 | (0.17,0.72) |
| P6 | [0.04,0.63,0.96,0.96] | 素世 / char6 | (0.65,0.80) |

主提示词的版式段可写：

```text
A single portrait manga page with five main panels and one bordered inset, six panels in total. A shallow wide establishing panel spans the top. In the middle band, a narrow tall panel on the left spans the combined height of two smaller horizontal panels stacked on the right. One large wide closing panel spans the bottom. A small phone-detail inset is embedded in the upper-left corner of this closing panel, leaving the closing speaker's face and speech bubble unobstructed on the right. Read the top panel first, then the tall middle-left panel, the upper middle-right panel, the lower middle-right panel, the phone inset, and finally the large closing panel. Use clearly visible gutters between main panels; the inset is the only intentional overlap. Panel identifiers are planning labels and are not printed in the artwork.
```

逐槽起句对应如下，再补人物锚点、表演和气泡：

| 槽 | 区域起句 | 对白 |
|---|---|---|
| char1 | `In the shallow full-width top panel, medium shot:` | 素世，我练到手都酸啦！ |
| char2 | `In the narrow middle-left panel spanning both middle rows, medium close-up:` | 哦？今天练了几首？ |
| char3 | `In the small upper middle-right panel, facial close-up:` | 四十五张…… |
| char4 | `In the small lower middle-right panel, facial close-up:` | 啊，你问练琴啊。 |
| char5 | `Inside the small bordered phone inset at the upper-left of the large bottom panel, extreme close-up:` | 无字，手机自拍缩略图；无需 girl 标签 |
| char6 | `In the large full-width bottom panel surrounding the phone inset, close-up:` | 难怪。酸的是举手机那只手吧。 |

主 `Text:` 中按表列出五段实际文字，插入格不制造一个空的文字条目。若 char5 将手机生成到格外，先检查它是否明确写了“嵌在底部宿主内”、是否启用坐标以及是否挤占主脸或气泡。若只有悬浮手机而没有独立小画框，补明插格自己的矩形黑边、白底和格沟，内部只放裁切的手机屏幕特写；不能把手机外壳当作已实现插入格边框。物体位置正确与画框实现是两个检查项。

本例于 2026-11-01 用 V5 Full、832×1216、28 steps、scale 5、seed 20261103 生成两次。两次均实现通栏开场、左侧跨行竖格、右侧上下小格和通栏结尾。第一次只有悬浮手机；同 seed 下补明独立矩形画框、白底与格沟后，第二次出现了单独框起的屏幕细节，形成五主格加一插格。屏幕里缩略图数量与要求仍有差异，画框边界也不能按数值保证。该测试没有验证斜格、破框、出血或跨 seed 稳定性；原有四格请求的 dry-run 仍通过。

## 其他选择

- 对白密集：一个环境通栏，随后两行不等宽对话格，结尾留较宽反应格；不要为了自由版式强加插入格。
- 动作段落：一个跨行主动作格，旁边排列小反应格；倾斜格线只用于相邻动作瞬间，保持明确阅读方向。
- 安静情绪页：少量大格与宽留白、局部小特写；需要时用底部出血收尾。留白本身是节奏，不自动用额外台词填满。

无论选哪种，生成后分别核对主格、插格、格子比例、跨行关系和内容落点；“有六格”与“实现了这张版式”是不同检查结果。
