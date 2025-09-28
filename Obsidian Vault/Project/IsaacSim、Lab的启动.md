newgrp docker ``` bash
source activate isaaclab_4_5_0
```


```

注意启动前，要在该终端

cd ~/isaacsim

source setup_conda_env.sh

否则执行下面代码会报错`ModuleNotFoundError: No module named 'isaacsim'`


cd ~/IsaacLab 
./isaaclab.sh -p scripts/demos/arms.py

```

启动
```bash
cd ~/isaacsim 
source setup_conda_env.sh  

./isaac-sim.sh --/persistent/isaac/asset_root/default="${HOME}/isaacsim_assets/Assets/Isaac/4.5"

```