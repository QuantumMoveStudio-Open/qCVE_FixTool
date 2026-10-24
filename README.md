# qCVE_FixTool

> 开源漏洞修复工具集 · 按 `qCVE` 编号归档
> 量方易动工作室 · 开源部（QuantumMoveStudio-Open）

![License](https://img.shields.io/badge/license-MIT-green)
![Python](https://img.shields.io/badge/python-3.8%2B-blue)
![Platform](https://img.shields.io/badge/platform-Termux%20%7C%20Linux%20%7C%20Windows-lightgrey)

---

## 项目简介

本仓库收录针对各类 **`qCVE-XXXXXX` 编号漏洞**的开源**修复工具**。仓库只提供检测与修复能力，不包含漏洞利用代码（PoC / exploit）。

组织方式为**一个漏洞编号一个目录**，同一漏洞可提供多套实现（Model 1 / Model 2 …），以便在不同环境、不同权限条件下选择合适的修复路径。

工具的整体设计取向一致，但具体实现与命令行参数各不相同：

- **单文件、纯标准库**：无需安装第三方依赖，可在 Termux、主流 Linux 发行版与 Windows 直接运行
- **先检测、后修复**：先扫描环境与组件版本，再确定修复范围
- **先备份、可回滚**：写入操作前将目标文件备份至工作目录
- **尽量可预演**：部分工具提供预演模式（如 `--dry-run`）与独立体检（如 `--verify`），是否支持以各自说明为准

---

## 工具索引

> 本表由项目维护者维护。新增工具的收录流程见「参与贡献」一节。

| qCVE 编号 | 型号 | 修复对象 | 文件 |
|:--|:--|:--|:--|
| `qCVE-20260146` | Model 1 | 运行时与依赖组件版本基线 | `tools/qCVE-20260146/model1.py` |
| `qCVE-20260146` | Model 2 | 浏览器内核版本（WebView / Chrome） | `tools/qCVE-20260146/model2.py` |

同一编号下的多个型号为**互补关系而非替代关系**，覆盖层面不同。以 `qCVE-20260146` 为例：Model 1 处理运行时与依赖包，Model 2 处理浏览器内核，两者配合方能完整覆盖。

各工具支持的系统与组件范围，见对应目录下的 `README.md`。

---

## 目录结构

```text
qCVE_FixTool/
├── README.md                      # 仓库总览与工具索引
├── LICENSE
├── .gitignore
├── tools/
│   ├── qCVE-20260146/
│   │   ├── model1.py
│   │   ├── model2.py
│   │   └── README.md              # 该编号的专用说明
│   └── <新编号>/
│       ├── model1.py
│       └── README.md
└── docs/
    └── TEMPLATE.md                # 单编号 README 模板
```

**命名约定**

- 目录：`tools/qCVE-<编号>/`
- 脚本：`model<N>.py`，N 从 1 起递增
- 兼容历史命名：`qCVE-<编号>_FixTool(Model<N>).py` 仍然有效；新增工具推荐使用上述结构，可避免括号在 Shell 中需转义的麻烦

漏洞的具体信息（影响版本区间、修复项、验证方式、回滚方法）记录在各编号目录的 `README.md` 中，仓库首页仅保留总览与索引。

---

## 快速开始

### 环境要求

- Python 3.8 及以上
- Termux：`pkg update && pkg install python git`
- Debian / Ubuntu：`sudo apt install python3 git`
- Windows：安装 [Python 3](https://www.python.org/downloads/)，勾选 *Add python.exe to PATH*

### 获取与运行

```bash
git clone https://github.com/QuantumMoveStudio-Open/qCVE_FixTool.git
cd qCVE_FixTool
```

以下以 `qCVE-20260146` 的 Model 1 为例，该工具支持 `--dry-run` 与 `--verify`；其余工具支持的参数可能不同，请以各自 `--help` 为准。

```bash
# 查看该工具支持的参数
python3 tools/qCVE-20260146/model1.py --help

# 预演：仅输出执行计划
python3 tools/qCVE-20260146/model1.py --dry-run

# 体检：不做任何修改
python3 tools/qCVE-20260146/model1.py --verify

# 执行修复
python3 tools/qCVE-20260146/model1.py
```

---

## 使用说明

各工具面向的漏洞不同、实现路径不同，**命令行参数并不统一**，此处不作统一约定。使用前请查阅对应目录的 `README.md`，或直接运行：

```bash
python3 tools/<编号>/model<N>.py --help
```

各编号目录下的 `README.md` 记录该工具支持的参数、退出码、检测范围、工作目录与回滚方式。

---

## 参与贡献

欢迎通过 Issue 反馈问题、通过 Pull Request 提交改进。由于本仓库由维护者统一管理，外部贡献请采用 **Fork + PR** 流程：

1. Fork 本仓库并克隆到本地
2. 新建分支，例如 `feat/qCVE-XXXXXX-model1`
3. 按「新增工具规范」实现并自测
4. 向 `main` 分支提交 Pull Request，说明修复目标、验证方式与测试环境

**新增工具规范**

1. 建立目录 `tools/qCVE-<编号>/`，脚本命名 `model1.py`、`model2.py` …
2. 以 `docs/TEMPLATE.md` 为基础编写该目录的 `README.md`，写明检测范围、支持的参数、验证方法与回滚方式
3. 不引入第三方依赖，保持单文件可运行
4. 若工具会写入系统，建议提供预演模式（如 `--dry-run`）与写入前的备份步骤
5. 提交前确认 `--help` 可正常输出，并按本工具实际支持的参数完成一次完整验证

工具索引表由维护者在合并 PR 后统一登记，提交者无需修改首页。

---

## 免责声明

- 本仓库仅提供检测与修复工具，不提供漏洞利用代码。
- 工具仅限用于已获合法授权的设备与自有环境，不得用于未授权系统。
- 版本检测基于本地命令输出与文件特征，存在误判可能，`--verify` 结果仅供参考。
- 工具在写入前会自动备份，但仍建议您在操作前自行备份重要数据。因使用本工具产生的任何直接或间接后果，由使用者自行承担。

---

## 相关链接

- 官网：<https://quantummovestudio.dpdns.org>
- 开源组织：<https://github.com/QuantumMoveStudio-Open>
- 合作方镜像站：<https://sunworld-fuchuan-technology-organization.dnsglobalnodes.dpdns.org/Resources>
- 联系邮箱：quantummovestudio@qq.com

---

## License

[MIT](LICENSE) © 2026 量方易动工作室（QuantumMoveStudio）
