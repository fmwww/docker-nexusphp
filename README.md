# docker-nexusphp

1. 复制`.env.example`为`.env`，可根据需要修改`.env`中的内容，启动
```shell
cp .env.example .env
docker-composer up -d
```
2. 进入到workspace容器
```shell
docker-compose exec php8 /bin/zsh
```
3. 在www目录下，拉取[nexusphp](https://github.com/xiaomlove/nexusphp)代码
```shell
git clone https://github.com/xiaomlove/nexusphp.git
```
4. 进入nexusphp目录，安装composer依赖
```shell
cd nexusphp
composer install
```
5. 复制安装脚本
```shell
cp -R nexus/Install/install public/
```
6. 访问nexusphp.test
