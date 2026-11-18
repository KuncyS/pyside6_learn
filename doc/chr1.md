# 安装 PySide6 的开发环境

在 macOS、Linux 或 Windows 上学习和开发 PySide6 时，不同项目可能依赖不同版本的包。为了避免包版本冲突并保持环境隔离，建议为每个项目创建独立的虚拟环境。本项目统一使用 `uv` 创建环境并管理依赖。

## 一、安装 uv

### 方式一：通过 pip 安装

```shell
pip install uv
```

### 方式二：使用官方脚本安装

macOS / Linux：

```shell
curl -LsSf https://astral.sh/uv/install.sh | sh
```

Windows PowerShell：

```powershell
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
```

升级 uv：

```shell
uv self update
```

## 二、使用 uv 创建 PySide6 项目

### 1. 初始化项目

在项目目录中执行：

```shell
uv init
```

该命令会生成：

- `pyproject.toml`：记录项目和依赖
- `.venv`：项目的虚拟环境
- `uv.lock`：锁定全部依赖版本，便于跨机器复现

指定 Python 版本：

```shell
uv python install 3.12
uv python pin 3.12
```

### 2. 安装 PySide6

```shell
uv add pyside6
```

uv 会自动更新 `pyproject.toml`，创建或更新 `uv.lock`，并同步 `.venv` 中的依赖。

添加开发工具（可选）：

```shell
uv add --dev ruff pytest
```

### 3. 运行项目

不需要手动激活虚拟环境，`uv run` 会自动使用项目环境：

```shell
uv run python main.py
uv run pytest
```

### 4. 同步项目依赖

首次运行或更换机器、拉取代码后，执行：

```shell
uv sync
```

该命令会根据锁文件建立一致的运行环境。

## 三、常用 uv 命令

```shell
uv python list                 # 查看本机可用的 Python 版本
uv python install 3.12         # 下载并安装指定 Python 版本
uv python pin 3.12             # 固定项目使用的 Python 版本
uv lock                        # 重新解析依赖并更新锁文件
uv lock --upgrade-package pandas
uv add requests pandas         # 添加依赖
uv add --dev ruff              # 添加开发依赖
uv remove requests             # 移除依赖
uv sync                        # 同步项目环境
uv run python main.py          # 在项目环境中运行程序
```

## 四、使用传统流程

如果项目使用 `venv` 或 `requirements.txt`，uv 仍可兼容这类工作流。

### 1. 创建虚拟环境

```shell
uv venv
uv venv --python 3.11
```

### 2. 激活虚拟环境

macOS / Linux：

```shell
source .venv/bin/activate
```

Windows 命令提示符：

```shell
.venv\Scripts\activate.bat
```

Windows PowerShell：

```powershell
.venv\Scripts\Activate.ps1
```

激活后终端前缀会显示 `(.venv)`。

### 3. 使用 uv 安装包

```shell
uv pip install pyside6
uv pip install -r requirements.txt
uv pip list
uv pip freeze
uv pip uninstall pyside6
```

精确同步依赖：

```shell
uv pip sync requirements.txt
```

`uv pip sync` 会删除环境中未列在文件里的包，使环境精确匹配依赖列表。

### 4. 退出虚拟环境

```shell
deactivate
```

删除虚拟环境只需删除 `.venv` 文件夹。

## 五、Python 自带的 venv

`pip` 本身不能创建虚拟环境；创建虚拟环境需要使用 Python 自带的 `venv` 模块。

```shell
python -m venv .venv
```

激活后使用 pip：

```shell
pip install pyside6
pip freeze > requirements.txt
pip install -r requirements.txt
pip list
pip uninstall pyside6
```

退出环境：

```shell
deactivate
```

## 六、venv + pip 与 uv 的对比

| 方案 | 优点 | 适用场景 |
|---|---|---|
| `venv + pip` | Python 内置，无需额外安装 | 简单脚本、老项目、学习环境 |
| `uv` | 速度快，兼容 pip、venv，并提供锁文件和 Python 版本管理 | 新项目和需要稳定复现的项目 |

对于 PySide6 项目，建议优先使用 `uv init`、`uv add` 和 `uv sync` 的现代项目流程。

