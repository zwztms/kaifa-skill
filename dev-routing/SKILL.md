---
name: "dev-routing"
description: "判断当前开发任务应走哪条流程并声明状态。Invoke when a new development request starts or when scope changes significantly."
---

# Dev Routing

## 目标

作为总路由器，先判断当前任务类型、项目状态、上下文完整度，再决定进入哪个下游 skill。

## 全局规则

- 当前状态固定输出为：`路由判断中`
- 除正式报告正文外，必须显式写出当前状态、当前目标、推荐流程、关键分歧、下一步动作
- 如果认为可以跳过前置阶段直接编码，必须先获得用户明确许可
- 如信息不足，不能伪装成高置信度；必须说明缺口
- 所有流程分歧、阶段跳过、是否直接编码，都必须以选择题形式交给用户

## 默认主流程

### 新项目

`business-design -> code-detailed-design -> implement-verify -> review-hard -> completion-workflow`

### 已有代码/已有系统

`repo-investigation -> business-design -> code-detailed-design -> implement-verify -> review-hard -> completion-workflow`

## repo-investigation 触发规则

以下情况必须进入 `repo-investigation`：

- 已有代码或已有运行系统
- 业务目标更新后，原先理解可能失效
- 代码更新后，原先设计依赖的现实约束可能变化
- 设计或实现过程中发现当前理解与仓库现实冲突

## 输出契约

必须输出：

1. 当前状态
2. 当前目标
3. 已知事实
4. 流程选择题
5. 推荐主流程
6. 可选替代流程
7. 是否允许直接编码
8. 若允许直接编码，需要用户确认的内容
9. 下一步应进入的 skill

## 推荐输出格式

```markdown
当前状态：路由判断中
当前目标：判断本次任务的执行流程
已知事实：
- ...

我的推荐：流程 A
推荐理由：...

请你做选择：
1. 流程 A（推荐） - 适用于...，风险...
2. 流程 B - 适用于...，风险...
3. 直接编码 - 仅在...时可选，风险...

是否允许直接编码：是/否
如果允许直接编码，需用户确认：...
下一步动作：进入 ...
```

## 禁止事项

- 禁止在本阶段直接产出详细设计或代码
- 禁止因为“看起来简单”就绕过用户确认直接跳步
- 禁止只给一个结论，不解释为什么走这条流程
