# research-skill

这个仓库用来维护可复用的 Codex / Agent skill。

目标不是堆资料，而是沉淀一套可触发、可执行、可复盘的工作范式。  
每个 skill 都应该回答三件事：

1. 什么时候触发
2. 触发后先做什么
3. 哪些路径已经被证伪，不该再打转

## 目录结构

```text
research-skill/
├─ skills/
│  ├─ top-down-debugging/
│  │  └─ SKILL.md
│  └─ retarget-missing-controls/
│     └─ SKILL.md
└─ README.md
```

## 当前 skill

### `top-down-debugging`

通用范式 skill。  
适用于代码、数据、动画、UI、构建、网络、自动化、工具链和方案研究等任何“开始打转”的任务。

核心要求：

- 先接管可信层级，不把 handoff / working 样本只当背景材料
- 强制建立 `working / failing / reference / layer model`
- 先找层，再找控制点，再找值
- 同层连续两次负信号后，必须升层
- 已知 dead path 直接禁用，不再在错误家族里微调

这个 skill 应该尽量保持通用，只沉淀跨问题类型都成立的方法论。

### `retarget-missing-controls`

专项 skill。  
适用于动画 retarget、动作 sidecar、骨骼附件、RootPos、Foot Target、TAE 控制点缺失这类问题。

这个 skill 可以更专，允许写具体检查项、具体骨骼、具体 sidecar 经验。

## 维护原则

### 1. 通用和专项分开

- 通用方法论放进 `top-down-debugging`
- 某个领域的特殊经验放进专项 skill

不要把一次性的项目细节塞进通用 skill。

### 2. 只写高价值约束

skill 不是 README 扩写版，也不是百科。  
只保留会直接改变代理行为的内容，例如：

- 触发条件
- 强制顺序
- 禁止路径
- 最小差异表模板
- 复盘规则

### 3. 优先沉淀“停止打转”的规则

如果某次调试最后发现：

- 可信层级没有被及时接管
- sidecar 被误判成主资产
- 上下文问题被误判成文件问题
- 在 dead path 里反复微调

就优先把这类教训写回 skill。

### 4. 文档保持中文

这个仓库默认用中文维护。  
除非某个术语必须保留英文，否则正文、说明和范式都用中文写。

## 同步路径

当前建议把这里当作维护源：

- `D:\research-skill\skills\...`

Codex 运行时安装副本：

- `C:\Users\61000\.codex\skills\...`

如果 skill 在这里改了，运行时副本也要同步，不然触发到的还是旧版本。

## 推荐工作流

1. 在真实任务里发现打转或重复试错
2. 先判断这是通用问题还是专项问题
3. 修改对应的 `SKILL.md`
4. 同步到安装副本
5. 下次任务里强制按 skill 执行
6. 会后把这次是否真的减少打转写进复盘

## 这套仓库当前的定位

它不是项目资料库。  
它更像一组“代理工作规范”：

- 遇到问题时，先怎么建模
- 什么时候该停下局部试错
- 什么时候该升层
- 哪些经验应该沉淀成长期约束

如果某条内容不能稳定改善下一次任务，就不该长期留在这里。
