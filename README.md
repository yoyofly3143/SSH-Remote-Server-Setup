# [SSH-Remote-Server-Setup](https://roadmap.sh/projects/ssh-remote-server-setup)

## 项目名称  
基于 SSH 的远程登录配置与验证  

## 项目目标  
- 生成两对 SSH 密钥  
- 将公钥导入阿里云 ECS 实例  
- 使用私钥在本地实现免密远程登录  

---

## 操作步骤  

### 1. 本地生成密钥对  
在本地 Linux 执行：  
```bash
ssh-keygen -t rsa -b 2048 -f ~/.ssh/key1
ssh-keygen -t rsa -b 2048 -f ~/.ssh/key2
```
说明：  
- `-t rsa`：指定密钥类型为 RSA  
- `-b 2048`：密钥长度  
- `-f`：指定文件名  

生成的文件：  
- 私钥：`~/.ssh/key1`、`~/.ssh/key2`  
- 公钥：`~/.ssh/key1.pub`、`~/.ssh/key2.pub`  

---

### 2. 导入公钥到 ECS 实例  
远程实例执行将本地生成的公钥内容添加到实例。  
```bash
echo "这里粘贴key1.pub的内容" >> ~/.ssh/authorized_keys
echo "这里粘贴key2.pub的内容" >> ~/.ssh/authorized_keys
```
> 注意：一行一个公钥，文件权限需正确：  
```bash
chmod 600 ~/.ssh/authorized_keys
chmod 700 ~/.ssh
```

---

### 3. 本地连接 ECS  
使用公网 IP 登录：  
```bash
ssh -i ~/.ssh/key1 root@<实例公网IP>
```
如果成功进入远程 shell，则说明免密登录配置成功。  

---

### 4. 可选：配置 SSH 简化登录  
编辑 `~/.ssh/config`，添加：  
```text
Host my-ecs
    HostName <实例公网IP>
    User root
    IdentityFile ~/.ssh/key1
```
以后只需执行：  
```bash
ssh my-ecs
```

---

## 总结  
- 生成了两对密钥 ✅  
- 配置公钥到 ECS ✅  
- 验证 SSH 免密登录 ✅  
