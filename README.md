# mjlab 四足机器人训练

基于 [mjlab](https://github.com/xxx/mjlab) 框架，添加了自己的 Dog1 和 Dog2 四足机器人，用于强化学习运动控制训练（velocity tracking）。

## 环境

```bash
source .venv/bin/activate
uv run ty check       # 类型检查
make check            # 格式化 + 类型检查
```

## 机器人

| 机器人 | 电机 | 驱动方式 | 路径 |
|---|---|---|---|
| **Dog1** | Unitree Go1 同款 | 直驱 | `src/mjlab/asset_zoo/robots/dog1/` |
| **Dog2** | 达妙 8009 | 直驱 | `src/mjlab/asset_zoo/robots/dog2/` |
| **ArmV5** | — | — | `src/mjlab/asset_zoo/robots/armv5/` |

Dog2 详细信息见 `src/mjlab/asset_zoo/robots/dog2/README.md`。

## 训练

```bash
# Dog2 粗糙地形
uv run train Mjlab-Velocity-Rough-Dog2 --env.scene.num-envs 512

# Dog2 平坦地形（大量环境并行）
uv run train Mjlab-Velocity-Flat-Dog2 --env.scene.num-envs 4096

# Dog1 粗糙地形
uv run train Mjlab-Velocity-Rough-Dog1 --env.scene.num-envs 1024

# ArmV5 机械臂 reach 任务
uv run train Mjlab-Reach-ArmV5
```

### 续训

```bash
uv run train Mjlab-Velocity-Rough-Dog2 \
  --env.scene.num-envs 256 \
  --agent.resume True \
  --agent.load_run 2026-05-25_22-24-30 \
  --agent.load_checkpoint model_9999.pt
```

## 可视化

```bash
uv run play Mjlab-Velocity-Rough-Dog2 \
  --checkpoint-file logs/rsl_rl/dog2_velocity/<run>/<model>.pt \
  --viewer viser --num-envs 1
```

## 已注册任务

```
Mjlab-Velocity-Rough-Dog1    Mjlab-Velocity-Flat-Dog1
Mjlab-Velocity-Rough-Dog2    Mjlab-Velocity-Flat-Dog2
Mjlab-Reach-ArmV5
```

## 项目结构

```
src/mjlab/
├── asset_zoo/robots/
│   ├── dog1/   dog1_constants.py  +  xmls/
│   └── dog2/   dog2_constants.py  +  xmls/  +  README.md
└── tasks/velocity/config/
    ├── dog1/   env_cfgs.py  rl_cfg.py
    └── dog2/   env_cfgs.py  rl_cfg.py

logs/rsl_rl/
├── dog1_velocity/
└── dog2_velocity/
```

## 常见问题

| 问题 | 解决 |
|---|---|
| 训练不收敛 | 检查 reward 权重、action scale、PD 增益 |
| 脚不离地 / 拖着走 | 打开 `air_time`、加大 `foot_clearance` |
| 小腿碰地 | 保留 `shank_collision` 传感器和惩罚 |
| nconmax overflow | `cfg.sim.nconmax = None` |
| NATIVECCD margin 错误 | XML 中 `geom margin` 去掉 |
| 显存不足 | 减少 `--env.scene.num-envs`


