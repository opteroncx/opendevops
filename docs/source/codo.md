### 项目前端

> 项目前端代码，提供两种方式，可自行编译，也可在下载release资源包，二选一
>
> 我们提供的有release包，建议直接下载release包更为方便！

**一、 直接下载资源包(建议)**

- 建议使用最新版本[Release](<https://github.com/opendevops-cn/codo/releases/>)

```
echo -e "\033[32m [INFO]: codo(项目前端) Start install. \033[0m"
CODO_VER="codo-beta-0.2.1"
if ! which wget &>/dev/null; then yum install -y wget >/dev/null 2>&1;fi
[ ! -d /var/www ] && mkdir -p /var/www
cd /var/www && wget https://github.com/opendevops-cn/codo/releases/download/${CODO_VER}/${CODO_VER}.tar.gz
tar zxf ${CODO_VER}.tar.gz
if [ $? == 0 ];then
    echo -e "\033[32m [INFO]: codo(项目前端) install success. \033[0m"
else
    echo -e "\033[31m [ERROR]: codo(项目前端) install faild \033[0m"
    exit -8
fi
```

**二、手动编译方式**

> 编译需要Node，不建议使用此方式。shell脚本内容，复制内容到.sh文件执行即可

```shell
[ -f /usr/local/bin/node ] && echo "Node already exists" && exit -1
cd /usr/local/src && rm -rf node-v10.14.2-linux-x64.tar.xz
wget https://nodejs.org/dist/v10.14.2/node-v10.14.2-linux-x64.tar.xz
tar xf node-v10.14.2-linux-x64.tar.xz -C  /usr/local/ >/dev/null 2>&1
rm -rf /usr/local/bin/node
rm -rf /usr/local/bin/npm
rm -rf /usr/bin/pm2
ln -s /usr/local/node-v10.14.2-linux-x64/bin/node /usr/local/bin/node
ln -s /usr/local/node-v10.14.2-linux-x64/bin/node /usr/bin/node
ln -s /usr/local/node-v10.14.2-linux-x64/bin/npm  /usr/local/bin/npm
ln -s /usr/local/node-v10.14.2-linux-x64/bin/npm  /usr/bin/npm
/usr/local/bin/node -v
/usr/local/bin/npm -v
sudo npm i -g pm2 >/dev/null 2>&1
ln -s /usr/local/node-v10.14.2-linux-x64/bin/pm2 /usr/bin/

```

**获取代码，修改配置**

```shell
cd /opt/codo/ && git clone https://github.com/opendevops-cn/codo.git && cd codo


vim src/config/index.js
### 修改配置 baseUrl pro 为你的后端地址, 默认已修改好，例如
baseUrl: {
    dev: '',
    pro: '/api/'
  },
npm install --ignore-script
npm run build
mkdir -p /var/www/codo && rm -rf /var/www/codo/*
\cp -arp dist/* /var/www/codo/

### 后续访问使用API网关中的vhosts，节省资源，这里不单独安装配置nginx
```


