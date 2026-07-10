# unirtos_uart_demos

本仓库推荐通过 unirtos-cli 的 demo 工作流使用，以保证创建、环境拉取和编译流程一致。

## 功能描述

本 Demo 展示串口通信的基础开发流程，适合作为外设通信与调试的入门样例。

- 演示 UART 初始化与基础收发路径
- 提供串口日志与数据交互验证示例
- 便于扩展协议封装、缓存处理与异常恢复逻辑

## 快速上手

### 1. 安装 UniRTOS 工具链

- [开发准备](https://www.quectel.com.cn/unirtos/docs?docs_page=快速上手/开发准备/开发准备.html)
- [安装交叉编译工具链](https://www.quectel.com.cn/unirtos/docs?docs_page=快速上手/环境搭建/环境搭建.html)
- [安装 Python3](https://www.python.org/downloads/)
- [安装 git](https://git-scm.com)
- 安装 `unirtos-cli`：`pip install unirtos-cli`

以上工具安装完成后，确认以下命令可用：

```bash
python --version # Python3
git --version
unirtos --version # 1.0.5 及以上版本
unirtos-cli version # 1.0.11 及以上版本
```

### 2. 使用 unirtos-cli 拉取 demo

先查看可用 demo 与版本：

```bash
unirtos-cli ls-demos
```

创建本 demo 工程：

```bash
unirtos-cli new -r unirtos_uart_demos
```

如需指定版本：

```bash
unirtos-cli new -r unirtos_uart_demos -v 1.0.0
```

### 3. 进入工程并编译

```bash
cd unirtos_uart_demos-1.0.0
unirtos-cli env-setup
unirtos-cli build
```

## 常用命令

```bash
# 打开 SDK 菜单配置
unirtos-cli menuconfig

# 清理构建产物
unirtos-cli clean
```

## 代码概览

#### 项目结构

```
uart_demos/
├── CMakeLists.txt           # CMake 构建配置
├── demo.manifest.json       # 应用清单文件
├── README.md                # 本文件
├── uart_demo.c              # UART 示例源代码
└── uart_demo.h              # UART 示例头文件和配置项
```

#### 示例工作流程

```
程序启动
	↓
调用 unir_uart_demo_init()
	↓
创建名为 "uart_demo" 的任务
	↓
进入任务主函数 unir_uart_demo_process()
	↓
注册 UART 事件回调 unir_uart_ind()
	↓
配置 UART 参数和引脚复用
	↓
打开 UART 端口
	↓
根据测试用例执行 UART 功能测试：
  ├─ 周期性发送数据
  ├─ 接收数据并通过回调处理
  ├─ 主动读取并回写数据
  ├─ 切换不同波特率
  └─ 切换 UART/AT 命令模式
```

#### 主要 API 接口

##### unir_uart_demo_init
任务初始化函数
- 输出 UART Demo 启动日志
- 检查任务是否已创建
- 创建 UART 示例任务，设置堆栈大小和优先级
- 设置任务名称和入口函数

##### unir_uart_demo_process
任务处理函数
- 注册 UART 事件回调
- 配置 UART 波特率、数据位、停止位、校验位和流控参数
- 配置 UART TX/RX 及流控引脚
- 打开 UART 端口
- 根据当前测试用例执行发送、接收、波特率切换或模式切换测试

##### unir_uart_ind
UART 事件回调函数
- 处理串口接收事件
- 处理发送完成事件
- 处理发送缓冲区不足，超出阈值事件
- 将事件信息通过 UART 输出

##### unir_demo_uart_case_switch
测试用例切换函数
- 切换当前 UART 测试场景
- 支持输出测试、回调接收、主动读取、波特率切换和 UART/AT 模式切换

#### 日志展示

初始化阶段可在日志中看到以下输出：

```
[I/UART_API] enter UniRTOS UART DEMO !!!
```

成功运行后，示例会初始化 UART1，并默认每隔约 1 秒通过串口输出以下内容：

```
hello UniRTOS
hello UniRTOS
hello UniRTOS
...
```

## 配置说明

默认 UART 配置定义在 `uart_demo.h` 中：

- `UNIR_TEST_UART_PORT`：默认测试端口为 `QOSA_UART_PORT_1`
- `UNIR_TEST_UART_TX_PIN`：默认 TX 引脚为 `UNIR_UART1_TX_PIN`
- `UNIR_TEST_UART_RX_PIN`：默认 RX 引脚为 `UNIR_UART1_RX_PIN`
- `CONFIG_UNIRTOS_UART_DEMO_TASK_STACK_SIZE`：任务栈大小为 4096
- `UNIR_UART_DEMO_TASK_PRIO`：任务优先级为 `QOSA_PRIORITY_NORMAL`

不同平台的 PINMUX 定义可能存在差异，请根据实际平台的 PINMUX 表调整 UART 引脚和复用功能配置。


## 技术社区

技术社区：https://forumschinese.quectel.com/c/66-category/66

## 贡献指南

欢迎参与共建，建议按以下方式提交：
- 提交前先执行一次基础验证：env-setup、build、clean。
- 使用清晰的提交说明，描述改动目的、影响范围和验证结果。
- 新增功能或行为变化时，同步更新 README 与相关文档。
- 通过 Issue 或 Pull Request 提交问题修复与功能改进。
