# funmodel-dev

funmodel 系列的联合开发仓库，用 Git 子模块把公共基础库和各个模型推理扩展包固定在一起，
一次 clone 就能拉齐整条产品线，用于跨包联调。

## 项目

| 目录 | 项目 | 类别 | 说明 |
| --- | --- | --- | --- |
| `apps/funmodel` | [funmodel](https://github.com/farfarfun/funmodel) | package | 公共基础库：`PredictModel` 推理基类、模型缓存目录约定，以及图像变换、URL 取图、摄像头帧队列等共用工具 |
| `apps/funmodel-dwpose` | [funmodel-dwpose](https://github.com/farfarfun/funmodel-dwpose) | package | 基于 DWPose ONNX 模型的全身姿态估计包，提供 `DWposePredict` |
| `apps/funmodel-mivolo` | [funmodel-mivolo](https://github.com/farfarfun/funmodel-mivolo) | package | 基于 MiVOLO 的人脸年龄与性别识别包，提供 `MivoloPredictor` |
| `apps/funmodel2` | [funmodel2](https://github.com/farfarfun/funmodel2) | package（已归档） | 基于已废弃 funkeras 的权重缓存实验，从未发布 PyPI，仅作历史留存；模型推理请用 `funmodel` |

各扩展包通过 PEP 420 命名空间包机制挂载到 `funmodel` 下，因此扩展包的改动往往要和基础库
一起验证——这也是把它们放进同一个工作区的原因。四个子项目都是纯 package，没有常驻服务进程。

具体的安装、配置和使用方式见各子项目自己的 README。

## 获取代码

首次克隆时同时拉取子模块：

```bash
git clone --recurse-submodules https://github.com/farfarfun/funmodel-dev.git
cd funmodel-dev
```

已经克隆过、但 `apps/` 下是空目录的，补拉子模块：

```bash
git submodule update --init --recursive
```

## 更新子模块

```bash
git submodule update --remote
git add apps/funmodel apps/funmodel-dwpose apps/funmodel-mivolo apps/funmodel2
```

子模块指针的更新由本仓库记录，需要随父仓库一起提交。

## 快速开始

只想用某个推理包，不必克隆本仓库，直接从 PyPI 安装：

```bash
pip install funmodel-dwpose
pip install funmodel-mivolo
```

要在工作区里联调（改基础库、用扩展包验证），把子模块装成可编辑模式：

```bash
pip install -e apps/funmodel
pip install -e apps/funmodel-dwpose
```

本仓库自身不发布任何包，也还没有共享的构建脚本（没有 `scripts/funbuild.toml`），
在这里执行 `funbuild build` 会因为识别不到版本清单而回退到空构建，发版请到各子项目仓库操作。

## 关于组织

[farfarfun](https://github.com/farfarfun) 是一个专注于实用工具库的开源组织，涵盖云存储、
数据处理、AI、多媒体与开发工具链等方向。本仓库属于其中的 AI 模型推理方向，只负责把
funmodel 系列的几个仓库编排在一个工作区里，不承载业务代码。

- 🏠 组织主页：<https://github.com/farfarfun>
- 📦 PyPI：<https://pypi.org/user/niuliangtao/>
- 📧 联系：farfarfun@qq.com
