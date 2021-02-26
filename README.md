# Multipass 集群文档  
## 设置 ssh 免密登录  
- 创建 ssh 密钥对  

``` sh
sh-keygen -b 2048 -f ~/.ssh/multipass -t rsa -q -N ""
```

*~/.ssh* 目录下会生成 *multipass* 和 *multipass.pub* 文件  

- 新建 cloud-init 配置文件  
``` sh
touch ./cloud-init.yaml
```

- 控制台输出 *multipass.pub* 公钥的内容  

``` sh
cat ~/.ssh/multipass.pub
```

- 粘贴内容到 cloud-init.yaml 文件中  

``` yaml
#cloud-config
ssh_authorized_keys:
  - ssh-rsa AAAAB3.......hh32R ruan@mbp
```

## 配置虚拟机代理  
- 通过 cloud-init 配置文件配置代理环境变量  

``` yaml
write_files:
  - path: /etc/environment
    content: |
      http_proxy=http://192.168.64.1:7890
      https_proxy=http://192.168.64.1:7890
    append: true
```

## 启动虚拟机并测试  
- 启动虚拟机  

``` sh
multipass launch --name vm1 --cpus 1 --mem 2048M --disk 20G --cloud-init ./cloud-init.yaml
```

- 查看虚拟机 IP 地址  

``` sh
multipass info vm1 | grep 'IPv4' | awk '{print $2}'
```

- 通过 ssh 链接虚拟机  

``` sh
ssh -i ~/.ssh/multipass ubuntu@192.168.64.14
```
