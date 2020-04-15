# 目的
本文档描述了如何构建一个longan的编译环境。

# 步骤
```shell
# 1. 拷贝ssh秘钥，用于ews的目录库的获取
## TODO: 使用映射
cp ~/.ssh/id* .
# 2. 构建docker
docker build --build-arg USER_ID=$(id -u ${USER}) GROUP_ID=$(id -g ${USER}) -t longan:v1 .
# 3. 运行docker
docker run -it --name BUILD -v ~/Workspace/Projects/longan/:/data longan:v1
```