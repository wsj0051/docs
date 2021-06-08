# 个人笔记
## Jenkins
1. 插件下载地址
 - [hpi插件地址](http://updates.jenkins-ci.org/latest/)
 - [Theme插件](http://wiki.jenkins-ci.org/display/JENKINS/Simple+Theme+Plugin)

2. 页面自定义
修改jar包`jenkins/WEB-INF/lib/jenkins-core-1.651.3.jar`文件中的`lib/layout/layout.jelly`

## github加速
1. 油猴脚本工具
- 加速访问脚本地址：https://greasyfork.org/zh-CN/scripts/397419
- 加速下载脚本脚本地址：https://greasyfork.org/scripts/412245
- Github 增强 - 高速下载: https://greasyfork.org/zh-CN/scripts/412245-github-%E5%A2%9E%E5%BC%BA-%E9%AB%98%E9%80%9F%E4%B8%8B%E8%BD%BD

2. 一键获取Github文件加速下载地址（浏览器扩展）
- 扩展官网：https://fhefh2015.github.io/Fast-GitHub/
- Chrome安装地址：https://chrome.google.com/webstore/detail/mfnkflidjnladnkldfonnaicljppahpg
- 国内可访问Chrome安装地址1：https://chrome.zzzmh.cn/info?token=mfnkflidjnladnkldfonnaicljppahpg
- 国内可访问Chrome安装地址2：https://www.extfans.com/productivity/mfnkflidjnladnkldfonnaicljppahpg/
- Edge安装地址：https://microsoftedge.microsoft.com/addons/detail/ljceflkaahacpphaioldeledefadpmdp
## npm
1. 设置代理，用户名密码
```
npm config set proxy http://username:password@server:port
npm confit set https-proxy http://username:password@server:port
```
2. 取消代理
```
npm config delete proxy
npm config delete https-proxy
```
3. cnpm
```
npm install -g cnpm --registry=https://registry.npm.taobao.org
```
## nvm
```
cd ~/
git clone https://github.com/nvm-sh/nvm.git .nvm
cd ~/.nvm
. ./nvm.sh
```
### 修改环境变量  
修改文件 `~/.bashrc`, `~/.profile`, or `~/.zshrc`末尾配置环境变量
```
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion
```
