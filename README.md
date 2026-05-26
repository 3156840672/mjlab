在官方mjlab中添加了自己的四足  
source ~/anaconda3/bin/activate  
conda activate env_isaaclab  
# train
```bash
uv run train Mjlab-Velocity-Rough-Dog1 --env.scene.num-envs 1024
```

# play
```bash
uv run play Mjlab-Velocity-Rough-Dog1   --checkpoint-file logs/rsl_rl/dog1_velocity/2026-05-06_13-21-47/model_7500.pt --viewer viser
```

```bash
uv run play Mjlab-Velocity-Rough-Dog1   --checkpoint-file logs/rsl_rl/dog1_velocity/2026-05-06_13-21-47/model_7500.pt
```

继续训练   
```bash
uv run train Mjlab-Velocity-Flat-Dog1 \
    --agent.resume True \
    --env.scene.num-envs 512
    --agent.load_run 2026-05-08_12-30-11 \
    --agent.load_checkpoint model_2100.pt
    ```


