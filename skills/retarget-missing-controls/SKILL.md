---
name: retarget-missing-controls
description: 当用户要修 Sekiro/FromSoftware 动作 retarget 的“看起来像了但仍有腿、附件、音效、判定问题”时使用。核心目标不是继续试 slot、HKB、菜单触发，而是先对比 working mod、当前失败产物、源动作，找出缺失控制点（如 RootPos、Foot_Target、reference frame、TAE 音效/判定事件），再把缺项收进主工具链。
---

# Retarget Missing Controls

## 何时使用

满足任一条件就用：

- 用户说“动作基本对了，但腿/脚/附件/音效/判定不对”
- 用户怀疑“不是参数问题，是算法缺项”
- 你发现自己开始反复改 `slot / HKB / 菜单触发 / 普通连段路由`

## 目标

先回答这句：

`working mod 比当前失败产物，多做了哪一步数据传递？`

不要先回答：

- “再换一个 combo slot”
- “再改一个 custom state”
- “再试一下菜单热键”

## 强制输入

开始前必须明确 3 个对比对象：

1. `working mod` 成品文件
2. `current failed artifact` 当前失败产物
3. `source anim` 源动作

没有这 3 个对象，不要进入游戏内 probe。

## 强制检查的控制点

按这个顺序查：

1. `RootPos`
   - 本地平移范围
   - 是否错误地被塞进 `Master`

2. `*_Foot_Target* / *_Effector* / *_IK`
   - 本地平移是否被错误归一化
   - 是否真的进了输出 track

3. `hkaDefaultAnimatedReferenceFrame`
   - X / Z / Y / facing
   - 是否与 working mod 同量级

4. 附件骨
   - `Sheath / LargeSheath / Bow / Weapon / String` 等
   - 平移和旋转是否真的在动
   - 是否只是“位置跟着走”，但缺少父链旋转/挂点行为

5. `TAE`
   - 时长是否匹配新动作
   - 音效事件是否还是旧 slot 残留
   - hitbox / 判定事件是否还是旧 slot 残留

## 禁止项

在离线 diff 出控制点前，不要做这些：

- 改 `combo4/combo5` 路由
- 改 `custom state` 入口
- 改菜单热键来猜问题
- 连续做 in-game 视觉实验

这些只能说明“表现错了”，不能说明“哪一层缺了数据”。

## 输出要求

先产出一张差异表，至少包含：

- 对象：working / failed / source
- 文件：`a05x hkx` / `c0000.anibnd a50.tae` / 需要时 `behbnd`
- 控制点：`RootPos / Foot_Target / accessory bones / reference frame / TAE events`
- 现象：谁有，谁没有，差异量级多少
- 结论：下一步该补算法、补 TAE、还是补 HKB

只有差异表成立后，才能继续改代码。

## 修复优先级

1. **算法缺项**
   - `RootPos`
   - `Foot_Target`
   - accessory bones
   - root-motion / reground

2. **播放链缺项**
   - TAE 时长
   - 音效
   - FFX
   - hitbox / 判定

3. **最后才是 HKB / state path**
   - 只在证明资产层已经完整后再碰

## 推荐话术模板

用户如果只说“继续修腿/音效/判定”，先 internally 改写成：

```text
先不要继续做游戏内 probe，也不要改 slot / HKB / 连段入口。

请把：
1. working mod 成品
2. 当前失败产物
3. 源动作

做一次离线控制点对比，必须逐项检查：
- Master
- RootPos
- *_Foot_Target* / *_Effector* / *_IK
- reference frame samples
- 附件骨
- TAE 时长 / 音效 / 判定 / 特效

先给差异表，再改框架。
```

## 落地到本工程

在本仓优先使用：

- `skel_diff/retarget.py`
- `skel_diff/compare_anim.py`
- `skel_diff/build_tae_groundseven.py`
- `skel_diff/build_vfx_groundseven.py`
- `docs/re_offsets.md`

不要再从零散 `sekiro_boss_control/research/retarget_*probe.py` 开始。
