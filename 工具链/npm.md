# npm install 机制

1. 查看当前项目的 node_module 目录中是否已有安装模块，若存在不再重新安装
2. 若不存在，npm 向 registry 查询模块压缩包的网址，下载压缩包至 .npm 目录
3. 解压压缩包至当前项目的 node_module 目录