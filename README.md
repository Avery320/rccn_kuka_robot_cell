# RCCN KUKA 機器人工作單元

## 1. 系統概述

本系統包含兩個 KUKA KR300 R2500 機器人，分別位於工作單元的東側和西側。每個機器人都配備了線性軌道系統，可以沿著 X 軸移動。

### 1.1 座標系統
- 世界座標系（World）：整個工作單元的參考座標系
- 東側機器人：位於 y = -2.4m 處
- 西側機器人：位於 y = 2.4033m 處

## 2. 機器人配置

### 2.1 關節結構
每個機器人包含以下關節：

| 關節 | 類型 | 運動範圍 | 速度限制 | 力矩限制 |
|------|------|----------|-----------|-----------|
| E1 (線性軌道) | 平移關節 | 0.0 至 4.266 米 | 0.25 米/秒 | 1000.0 N·m |
| A1 (基座旋轉) | 旋轉關節 | -185° 至 185° | 156°/秒 | - |
| A2 (肩部關節) | 旋轉關節 | -155° 至 35° | 156°/秒 | - |
| A3 (肘部關節) | 旋轉關節 | -130° 至 154° | 156°/秒 | - |
| A4 (腕部旋轉) | 旋轉關節 | -350° 至 350° | 330°/秒 | - |
| A5 (腕部彎曲) | 旋轉關節 | -130° 至 130° | 330°/秒 | - |
| A6 (工具端旋轉) | 旋轉關節 | -350° 至 350° | 615°/秒 | - |

### 2.2 初始位置配置
機器人的初始位置可以在 `config/joint_limits.yaml` 中配置：

```yaml
west_robot:
  initial_positions:
    west_joint_e1: 1.0     # 軌道位置（米）
    west_joint_a1: 90.0    # 基座旋轉
    west_joint_a2: 0.0     # 肩部關節
    west_joint_a3: 90.0    # 肘部關節
    west_joint_a4: 0.0     # 腕部旋轉
    west_joint_a5: 0.0     # 腕部彎曲
    west_joint_a6: 0.0     # 工具端旋轉
```

## 3. 使用說明

### 3.1 啟動系統
使用以下命令啟動機器人工作單元：
```bash
roslaunch rccn_kuka_robot_cell display_with_joints.launch
```

### 3.2 參數配置
- 關節限制和初始位置：`config/joint_limits.yaml`
- 機器人模型：`urdf/rccn_kuka_robot_cell.urdf`
- RViz 配置：`rviz/urdf.rviz`

### 3.3 視覺化
系統啟動後，您可以在 RViz 中：
1. 查看機器人的 3D 模型
2. 使用 GUI 控制器調整關節位置
3. 觀察關節狀態和座標系

## 4. 文件結構

```
rccn_kuka_robot_cell/
├── config/
│   └── joint_limits.yaml    # 關節參數配置
├── launch/
│   └── display_with_joints.launch  # 啟動文件
├── meshes/
│   ├── rail/               # 軌道系統模型
│   └── world/              # 工作單元模型
├── rviz/
│   └── urdf.rviz          # RViz 配置文件
└── urdf/
    ├── rccn_kuka_robot_cell.xacro    # 主 XACRO 文件
    ├── rccn_west_robot_macro.xacro   # 西側機器人宏
    └── rccn_east_robot_macro.xacro   # 東側機器人宏
```

## 5. 注意事項

1. 在修改關節參數時，請確保：
   - 角度值使用度數（degrees）
   - 線性移動使用米（meters）
   - 速度限制使用適當的單位（度/秒或米/秒）

2. 啟動系統前，請確保：
   - 所有必要的 ROS 包都已安裝
   - 機器人模型文件路徑正確
   - 配置文件格式正確

3. 使用 GUI 控制器時：
   - 注意關節限制
   - 避免突然的大幅度運動
   - 觀察 RViz 中的實時反饋 