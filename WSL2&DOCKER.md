# WSL2 安装Ollama

## WSL2 安装独立的docker

```bash
$ curl -fsSL https://get.docker.com -o get-docker.sh
$ sudo sh get-docker.sh
$ sudo systemctl start docker
```

执行一次```dockerd-rootless-setuptool.sh install```后如果提示这个
执行这段脚本，然后再执行一次
作用是可以在当前用户使用docker
```
########## BEGIN ##########
sudo sh -eux <<EOF
# Install newuidmap & newgidmap binaries
apt-get install -y uidmap
EOF
########## END ##########
```

## 准备Ollama用的环境变量
```bash
export OLLAMA_HOST=0.0.0.0:11434
export OLLAMA_MODELS=/home/ai/ollama-models
```

OLLAMA_HOST：修改绑定地址
OLLAMA_ORIGINS:跨域列表，局域网内部访问可设置成*
OLLAMA_MODELS ：修改模型存放地址
OLLAMA_MAX_LOADED_MODELS： 可以并发加载的最大模型数，前提是它们适合可用内存。
OLLAMA_NUM_PARALLEL： 每个模型同时处理的最大并行请求数。默认值将根据可用内存自动选择 4 或 1。

# 安装Ollama

打开WSL执行
```bash
curl -fsSL https://ollama.com/install.sh | bash
```

查看状态
```bash
sudo systemctl status ollama
```

查看进程
```bash
sudo ps -ef | grep -v color | grep ollama
```

### 查看端口
先安装：
```bash
sudo apt install net-tools
```

查看：
```bash
sudo netstat -anp | grep 11434
```

测试目的，先搞一个小模型跑起来
```bash
ollama pull qwen3:4b
```

查看ip
```bash
ip addr | grep inet |grep eth
```
