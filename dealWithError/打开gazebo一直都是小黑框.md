时间              2025.08.01
起因：无法正常打开gazebo，一直都是小黑框

### 原因分析：
由于缺少模型导致
### 解决路径：
需要提前把模型下载好并放在指定目录下
```bash
cd ~/.gazebo/
mkdir -p models
cd ~/.gazebo/models/
wget http://file.ncnynl.com/ros/gazebo_models.txt
wget -i gazebo_models.txt
ls model.tar.g* | xargs -n1 tar xzvf
```

#error #ROS 