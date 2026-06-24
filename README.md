# WS-Maestro

WoSense App 自动化测试项目。

## 项目简介

本项目基于 Maestro 实现 WoSense Android App 的自动化测试。

WoSense 是一款用于控制和管理手持 SLAM 设备的软件，支持设备连接、扫描控制、项目管理、点云采集、数据导出等功能。

本仓库用于：

- 功能回归测试
- 冒烟测试
- Bug 复现验证
- 稳定性测试
- Jenkins CI 集成

------

## 技术栈

- Maestro
- Android Debug Bridge (ADB)
- Jenkins
- Git

------

## 目录结构

```text
WS-Maestro
│
├─ flows/             # 正式测试用例
│
├─ experiments/       # Bug复现、临时验证脚本
│
├─ data/              # 公共测试数据
│
├─ docs/              # 项目文档
│
├─ Jenkinsfile        # Jenkins流水线
│
├─ README.md
│
└─ .gitignore
```

------

## Flows 目录规范

flows 用于存放正式测试用例。

原则：

- 一个 yaml 对应一个测试场景
- 用例名称应能体现测试目标
- 避免使用 test1、demo、new 等无意义命名

推荐：

```text
language_switch.yaml
device_connect.yaml
rtk_collection.yaml
project_create.yaml
```

不推荐：

```text
demo.yaml
test.yaml
new.yaml
aaa.yaml
```

------

## Experiments 目录规范

experiments 用于存放：

- Bug复现脚本
- 临时验证脚本
- 压力测试脚本
- 调试脚本

此目录中的脚本默认不参与回归测试。

示例：

```text
scan_stress.yaml
language_bug_1023.yaml
connection_retry.yaml
```

------

## 测试数据规范

仅当数据被多个用例复用时，才放入 data 目录。

例如：

```text
data/
├─ accounts.yaml
├─ devices.yaml
└─ coordinate_systems.yaml
```

如果数据仅在单个用例中使用，可以直接写在脚本内部。

避免为了数据分离而分离。

------

## 脚本开发原则

### 保持可读性

优先保证脚本可读。

推荐：

```yaml
- tapOn:
    id: iv_setting

- tapOn:
    id: rb_language
```

不要过度拆分简单操作。

------

### 避免过度封装

只有当一段业务流程被多个用例重复使用时，才考虑抽取公共流程。

例如：

- 连接设备
- 创建项目
- 打开设置
- 开始扫描

否则直接写在用例中。

------

### 用例独立

每个用例应尽量独立执行。

避免：

- 依赖其他用例执行结果
- 依赖人工预操作
- 依赖未知环境状态

------

## Jenkins 集成

后续支持：

- 参数化执行
- 指定设备执行
- 指定脚本执行
- 定时回归测试
- Git Push 自动触发
- 测试结果归档

------

## 当前规划

第一阶段：

- 完成核心功能自动化覆盖

第二阶段：

- 接入 Git 管理

第三阶段：

- Jenkins 自动执行

第四阶段：

- 夜间回归测试

第五阶段：

- 自动巡检与稳定性测试

------

## 维护说明

新增测试脚本前，请确认：

1. 是否属于正式测试用例
2. 是否属于 Bug 复现脚本
3. 是否需要公共测试数据
4. 是否需要加入 Jenkins 回归任务

优先保证脚本简单、清晰、易维护。
