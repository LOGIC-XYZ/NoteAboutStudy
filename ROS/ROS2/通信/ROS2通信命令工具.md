---
tags:
  - ROS
---
## ros2 node
```
info  输出节点信息
list  输出运行中的节点的列表
```
## ros2 interface
```
list      输出所有可用的接口消息
package   输出指定功能包下的
packages  输出包含接口消息的功能包
proto     输出接口消息原型
show      输出接口消息定义格式
```
## ros2 topic
```
bw       输出话题消息传输占用的带宽
delay    输出带有 header 的话题延迟
echo     输出某个话题下的消息
find     根据类型查找话题
hz       输出消息发布频率
info     输出话题相关信息
list     输出运行中的话题列表
pub      向指定话题发布消息
type     输出话题使用的接口类型
```
## ros2 service
```
call  向某个服务发送请求
find  根据类型查找服务
list  输出运行中的服务列表
type  输出服务使用的接口类型
```
## ros2 action
```
info       输出指定动作的相关信息
list       输出运行中的动作的列表
send_goal  向指定动作发送请求
```
## ros2 param
```
delete    删除参数
describe  输出参数的描述信息
dump      将节点参数写入到磁盘文件
get       获取某个参数
list      输出可用的参数的列表
load      从磁盘文件加载参数到节点
set       设置参数
```