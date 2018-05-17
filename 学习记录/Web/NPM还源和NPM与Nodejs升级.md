# NPM换源
## 临时使用
```
npm --registry https://registry.npm.taobao.org install express
```
## 使用nrm工具换源
```bash
#安装
npm install -g nrm

#列出所有源
nrm ls

#切换淘宝源
nrm use taobao

#测试速度
nrm test
```

# 升级NPM
```
npm -g install npm@next
```
# 升级Nodejs
使用版本管理工具`n`

## 安装
```
npm install -g n
```

## 切换版本
```bash
n stable  #最新稳定版

n latest  #最新版

n 版本号  #指定版
```