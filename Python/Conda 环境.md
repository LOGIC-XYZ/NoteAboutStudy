创建新环境
```sh
conda create -n <环境名> python=<版本>
```

查看所有环境
```sh
conda env list
conda info --envs
```

进入环境
```sh
conda activate <环境名>
```

退出环境
```sh
conda deactivate
```

删除某个环境
```sh
conda remove -n <环境名> --all
```

查看环境所安装的包
```sh
conda list    # 当前环境
conda list -n <环境名>    # 指定环境
```

导出当前环境安装的包
```sh
conda list --export > requirements.txt
```