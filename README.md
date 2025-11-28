# 基于双重水印的人脸深伪篡改有效检测与身份高质恢复的主动取证平台
# 系统使用

1. 打开浏览器访问`iscp.roseliquor.xyz`即可进入系统的首页界面。

2. 可根据需求自行注册账号来登录系统或者使用内置管理员账号：

   邮箱：XXXXXXXXX

   密码：XXXXXXXXX

3. 后续就可以根据系统面板提示使用各种功能（水印嵌入，篡改人脸，定位恢复等）

   **ps: **

   ​	**1、我们提供了测试图片test_image.png，当然可以选择自行上传图片，但需要尽量含有清晰的人脸面部信息，图片尽量高宽等维否则容易影响后续效果!**

   ​	**2、由于模型处理需要一定时间，请在上传图片点击功能按钮后耐心等待结果返回。**

4. **使用说明：**

   ① 首先需要使用水印嵌入系统来嵌入水印；

   ② 嵌入篡改定位水印后，再进行伪造模拟（局部编辑）后，可以对局部编辑的区域进行篡改定位（对应篡改定位系统）

   ③ 嵌入身份恢复水印后，再进行伪造模拟（身份替换）后，可以对身份替换的图片进行身份恢复（对应身份恢复系统）

   ④ 嵌入双重水印后，对于进行伪造模拟(局部编辑、身份替换)后的图像，可以同时实现篡改定位和身份恢复

5. 系统仅供学习交流使用，请勿用作其他侵权行为！如使用中遇到一些问题（eg：网站不稳定，模型服务调用失败等）

   请联系我们：

   188****7326（微信同号）

6. 感谢您的体验，如有相关可改进的地方也欢迎提出建议！

# 本地部署

1. **基础环境配置：** 

   - 操作系统   Microsoft Windows10及以上 

   - 内存  16GB及以上
   -  存储空间  32GB及以上 
   - 图像处理器 CUDA12.6 NVIDIAGeForceRTX3050 LaptopGPU以上 
   - 其他软件配置：
     - 前端开发工具VisualStudio Code 
     - 后端开发工具Pycharm专业版 
     - 数据库管理工具Navicat15.0.25 
     - 数据库 MySQL8.0.37 Redis2.0.14 
     - 前端开发框架 Node20.14.0 Vue3.5.17 
     - 后端开发框架 Python3.11.9 FastAPI0.115.12 
     - 深度学习开发框架PyTorch2.7.1(CUDA12.6版本)

2. **后端启动** 

   - **下载源码**

     下载源代码文件并解压，进入到code/safe-system/backend，使用Pycharm或者VS Code打开。

   - **下载模型并导入** 

     本系统涉及到大量深度学习模型，占用空间较大，因此已经将预训练的权重文件上传至百度网盘，请从网盘中下载所有文件方面后续操作，百度网盘链接的如下：https://pan.baidu.com/s/1JEDhbb_Q0AqcxMa7on9cCQ?pwd=**** 根据下面的表格，从网盘内下载各个模型权重文件并导入到后端相应的文件夹下。

     |     网盘文件夹     |                模型功能说明                 |                          后端文件夹                          |     来源      |
     | :----------------: | :-----------------------------------------: | :----------------------------------------------------------: | :-----------: |
     |      IdFMark       |            嵌入提取身份特征水印             |              backend\ISC_Net\IdFMark\checkpoint              |    自训练     |
     |         IR         |        根据身份特征水印执行身份恢复         |                backend\ISC_Net\IR\checkpoints                |    自训练     |
     |         TL         |            嵌入提取篡改定位水印             |                backend\ISC_Net\TL\checkpoints                | 开源+自主微调 |
     |   arcface_model    |            提取人脸身份特征向量             |                backend\ISC_Net\arcface_model                 |     开源      |
     |  insightface_func  |                人脸区域识别                 |       backend\ISC_Net\insightface_func\models\antelope       |     开源      |
     | noise_faceshif ter |       伪造模拟——faceshifter 所需文件        |        backend\ISC_Net\Noise\faceshifter\saved_models        |     开源      |
     |   noise_simswap    |         伪造模拟——simswap 所需文件          |       backend\ISC_Net\Noise\simswap\checkpoints\people       |     开源      |
     |   noise_infoswap   |         伪造模拟——infoswap 所需文件         | backend\ISC_Net\Noise\infoswa p\checkpoints_512\wo_kernel_smooth |     开源      |
     |         dm         | 伪造模拟——stable diffusion-inpaint 所需文件 |                          backend\dm                          |     开源      |

3. **数据集下载** 

   本平台为了防止深度伪造模型被恶意利用，因此所有深度伪造模拟行为均为 随机，系统需要为深度伪造模型提供源人脸数据集。由于数据集较大，已经上传 到百度网盘，请从百度网盘中下载deepfake_dataset.zip文件，百度网盘的链接为：https://pan.baidu.com/s/1JEDhbb_Q0AqcxMa7on9cCQ?pwd=****，从该链接中下载 deepfake_dataset.zip 文件后，解压到后端文件夹deepfake_dataset 文件夹即可。

4. **环境安装** 

   - pytorch 等前述基础环境配置请提前自行从官网下载，步骤简单，不再赘述； 
   - 运行命令：pip install -r requirements.txt，完成后端环境配置。 

5. **数据库配置** 

   - 自行修改backend/app/core/config.py 文件夹下的用户名、密码等数据库配置信息，然后运行python -m app.scripts.init_db 命令，初始化数据库表。 
   - 后端项目启动 运行命令：uvicorn main: app--reload，即可启动后端项目。

6. **前端启动** 

   - 下载源代码文件并解压，code/safe-system/frontend，使用VSCode打开； 
   - 安装相关包：npm install； 
   -  启动前端：npm run dev。 
   - 访问127.0.0.1:5173，即可体验项目。



# 云端部署

基于AI的面部篡改检测与取证系统 - 云端Web应用

## 🏗️ 项目架构

```
┌─────────────────┐    HTTP请求     ┌──────────────────┐
│   腾讯云服务器    │ ──────────────►│   本地GPU服务器    │
│                 │                │                  │
│  ┌─────────────┐│                │ ┌─────────────── │
│  │   前端Vue    ││                │ │  AI模型推理   │ │
│  │   应用       ││                │ │  服务        │ │
│  └─────────────┘│                │ └─────────────── │
│  ┌─────────────┐│                │                  │
│  │  后端API     ││                │                  │
│  │  服务        ││                │                  │
│  └─────────────┘│                │                  │
└─────────────────┘                └──────────────────┘
```

## 📦 项目结构

```
safety-system-web/
├── frontend/                 # Vue.js前端应用
│   ├── src/
│   ├── public/
│   └── package.json
├── backend/                  # FastAPI后端服务
│   ├── app/
│   │   ├── api/             # API路由
│   │   ├── core/            # 核心配置
│   │   ├── models/          # 数据模型
│   │   └── utils/           # 工具函数
│   ├── main.py              # 应用入口
│   ├── requirements.txt     # Python依赖
│   └── .env                 # 环境配置
└── deploy/                  # 部署配置
    ├── nginx.conf           # Nginx配置
    └── deploy.sh            # 部署脚本
```

## 🚀 快速部署

### 1. 准备工作

确保你的腾讯云服务器满足以下要求：
- Ubuntu 20.04+ 或 CentOS 7+
- 4GB+ 内存
- 20GB+ 磁盘空间
- 已开放80端口

### 2. 上传项目文件

```bash
# 将项目文件上传到服务器
scp -r safety-system-web root@your-server-ip:/opt/
```

### 3. 运行部署脚本

```bash
# 登录服务器
ssh root@your-server-ip

# 进入项目目录
cd /opt/safety-system-web

# 给部署脚本执行权限
chmod +x deploy/deploy.sh

# 运行部署脚本
./deploy/deploy.sh
```

### 4. 配置模型服务器地址

```bash
# 编辑环境配置文件
nano backend/.env

# 修改MODEL_SERVER_URL为你的本地服务器地址
MODEL_SERVER_URL=http://your-home-ip:5001
```

### 5. 重启服务

```bash
systemctl restart safety-system-web
```

## ⚙️ 手动部署步骤

如果自动部署脚本失败，可以按以下步骤手动部署：

### 1. 安装依赖

```bash
# 更新系统
apt update && apt upgrade -y

# 安装必要软件
apt install -y nginx python3 python3-pip python3-venv nodejs npm
```

### 2. 配置Python环境

```bash
# 创建虚拟环境
cd /opt/safety-system-web
python3 -m venv venv
source venv/bin/activate

# 安装Python依赖
pip install -r backend/requirements.txt
```

### 3. 构建前端

```bash
cd frontend
npm install
npm run build
cd ..
```

### 4. 配置Nginx

```bash
# 复制配置文件
cp deploy/nginx.conf /etc/nginx/sites-available/safety-system

# 修改配置文件中的路径
sed -i 's|/path/to/safety-system-web|/opt/safety-system-web|g' /etc/nginx/sites-available/safety-system
sed -i 's|/var/www/safety-system|/opt/safety-system-web/frontend/dist|g' /etc/nginx/sites-available/safety-system

# 启用站点
ln -s /etc/nginx/sites-available/safety-system /etc/nginx/sites-enabled/
rm -f /etc/nginx/sites-enabled/default

# 测试配置
nginx -t

# 重启Nginx
systemctl restart nginx
```

### 5. 配置系统服务

```bash
# 创建systemd服务文件
cat > /etc/systemd/system/safety-system-web.service << EOF
[Unit]
Description=Safety System Web API
After=network.target

[Service]
Type=exec
User=www-data
Group=www-data
WorkingDirectory=/opt/safety-system-web/backend
Environment=PATH=/opt/safety-system-web/venv/bin
ExecStart=/opt/safety-system-web/venv/bin/uvicorn main:app --host 0.0.0.0 --port 8000
Restart=always
RestartSec=3

[Install]
WantedBy=multi-user.target
EOF

# 设置权限
chown -R www-data:www-data /opt/safety-system-web

# 启动服务
systemctl daemon-reload
systemctl enable safety-system-web
systemctl start safety-system-web
```

## 🔧 配置说明

### 环境变量配置

编辑 `backend/.env` 文件：

```env
# 模型服务器地址 - 重要！
MODEL_SERVER_URL=http://your-home-ip:5001

# JWT密钥 - 生产环境必须修改
SECRET_KEY=your-super-secret-key

# 数据库配置
DATABASE_URL=sqlite:///./safety_system.db

# 其他配置...
```

### Nginx配置

主要配置项在 `deploy/nginx.conf`：
- 静态文件服务
- API代理转发
- 文件上传大小限制
- 安全头设置

## 🔗 内网穿透配置

### 使用FRP

1. **配置FRP客户端**

   编辑 `frp/frpc.toml`:

```ini
[common]
server_addr = your-frp-server.com
server_port = 7000
token = your-frp-token

[model_api]
type = http
local_ip = 127.0.0.1
local_port = 5001
custom_domains = model-api.your-domain.com
```

2. **启动FRP客户端**

```bash
# 下载FRP客户端
wget https://github.com/fatedier/frp/releases/download/v0.52.3/frp_0.52.3_linux_amd64.tar.gz
tar -xzf frp_0.52.3_linux_amd64.tar.gz

# 启动客户端
./frpc -c frp/frpc.toml
```

## 📋 服务管理

### 查看服务状态

```bash
# 查看API服务状态
systemctl status safety-system-web

# 查看Nginx状态
systemctl status nginx

# 查看API日志
journalctl -u safety-system-web -f

# 查看Nginx日志
tail -f /var/log/nginx/access.log
tail -f /var/log/nginx/error.log
```

### 重启服务

```bash
# 重启API服务
systemctl restart safety-system-web

# 重启Nginx
systemctl restart nginx

# 重新加载Nginx配置
nginx -s reload
```

## 🔍 故障排除

### 常见问题

1. **API服务启动失败**
   
   ```bash
   # 检查Python依赖
   source venv/bin/activate
   pip install -r backend/requirements.txt
   
   # 检查配置文件
   cat backend/.env
   ```
   
2. **前端页面无法访问**
   ```bash
   # 检查Nginx配置
   nginx -t
   
   # 检查文件权限
   ls -la frontend/dist/
   ```

3. **模型服务连接失败**
   ```bash
   # 测试模型服务连接
   curl http://your-home-ip:5001/health
   
   # 检查防火墙设置
   ufw status
   ```

### 日志分析

```bash
# API服务日志
journalctl -u safety-system-web --since "1 hour ago"

# Nginx错误日志
tail -100 /var/log/nginx/error.log

# 系统日志
dmesg | tail -20
```

## 🔒 安全建议

1. **修改默认密钥**
   - 更改 `.env` 中的 `SECRET_KEY`
   - 使用强密码策略

2. **配置防火墙**
   ```bash
   ufw enable
   ufw allow 22    # SSH
   ufw allow 80    # HTTP
   ufw allow 443   # HTTPS (如果使用)
   ```

3. **启用HTTPS**
   - 申请SSL证书
   - 配置Nginx HTTPS

4. **定期更新**
   ```bash
   apt update && apt upgrade -y
   ```

## 📞 技术支持

如果遇到部署问题，请检查：
1. 服务器系统要求
2. 网络连接状态
3. 模型服务器配置
4. 日志文件内容

---

**注意**: 确保本地GPU模型服务正常运行，否则云端应用无法正常工作。
