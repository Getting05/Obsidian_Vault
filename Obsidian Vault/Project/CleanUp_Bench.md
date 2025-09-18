## 🎯 项目概述

**CleanUp_Bench** 是一个基于 Isaac Sim 的智能清洁机器人演示项目，它模拟了一个 Create-3 移动机器人配备 Panda 7DOF 机械臂在室内环境中执行垃圾收集任务。

## 📂 项目架构分析

### 1. **核心启动脚本 (run.sh)**
这是项目的主入口脚本，具有以下关键功能：

- **🔍 智能环境检测**：自动检测用户名、Python版本、CUDA环境
- **📁 配置驱动的路径管理**：通过 config.py 自动配置所有必要路径
- **✅ 系统验证**：检查 Isaac Sim 安装、机器人模型文件、资产库等
- **🚀 一键启动**：自动设置环境变量并启动主程序

关键特性：
```bash
# 支持多用户环境
CURRENT_USER=${USER:-${USERNAME:-$(whoami)}}

# 路径验证和错误处理
if [ $MISSING_PATHS -gt 0 ]; then
    print_error "发现 $MISSING_PATHS 个路径问题"
    # 提供详细的修复建议
fi
```

### 2. **配置管理系统 (config.py)**
这是项目的配置中心，采用面向对象设计：

- **🔧 用户路径自动检测**：智能匹配不同用户的安装路径
- **⚙️ 参数集中管理**：物理引擎、机器人控制、场景设置等
- **🎛️ 快速配置预设**：提供多种预设配置（性能优化、调试模式等）

核心配置类：
```python
class CleanupSystemConfig:
    def __init__(self, username=None):
        self.USERNAME = username
        self.USER_PATHS = {
            "isaac_assets_base": f"/home/{username}/isaacsim_assets/...",
            "isaac_sim_install": f"/home/{username}/isaacsim"
        }
```

### 3. **主程序 (ultra_stable_create3.py)**
这是核心仿真程序，实现完整的机器人清洁系统：

- **🤖 机器人系统**：Create-3 移动平台 + Panda 机械臂
- **🏠 场景构建**：室内家具环境 + 垃圾物品生成
- **🧠 智能算法**：A*路径规划 + 精确抓取控制
- **⚡ 性能优化**：CUDA加速 + 多线程处理

## 🔄 工作流程

### 启动阶段：
1. **环境检测** → 验证Python、CUDA、Isaac Sim
2. **配置加载** → 读取用户特定的路径和参数
3. **路径验证** → 确保所有必要文件存在
4. **仿真初始化** → 启动Isaac Sim仿真环境

### 运行阶段：
1. **🏠 场景创建**：生成室内家具和垃圾物品
2. **🤖 机器人初始化**：配置移动平台和机械臂
3. **🎯 任务执行**：
   - 小垃圾智能收集（2-3分钟）
   - 大垃圾精确抓取（3-4分钟）
   - 智能返回起点


## 🛠️ 使用方式

### 简单启动：
```bash
./run.sh
```

### 自定义配置：
```python
# 在 ultra_stable_create3.py 中选择配置
config = QuickConfigs.debug_mode(username)        # 调试模式
config = QuickConfigs.performance_optimized()     # 性能优化
```
