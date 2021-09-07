# 第三方依赖库安全管理 


一套完整的系统，需要用到大量的第三方资源，包括操作系统、链接库、中间件、数据库等等，如果第三方资源存在严重的安全漏洞，那么将导致系统带来巨大的安全问题。可以参考[第三方漏洞介绍](https://cdn2.hubspot.net/hub/203759/file-1100864196-pdf/docs/Contrast_-_Insecure_Libraries_2014.pdf)

因此,知道系统使用了那些第三方资源，并且跟踪这些第三方资源的最新漏洞，将会是非常必要的 

## Dependency Check 
[官方文档](https://wiki.owasp.org/index.php/OWASP_Dependency_Check)  
[github dependency-check](https://github.com/jeremylong/DependencyCheck)  

### 介绍 
对项目下的第三方库进行扫描，结合[NVD](https://nvd.nist.gov/products/cpe)数据进行比对，确认第三方库是否存在安全漏洞，主要针对Java语言和.NET  

### 插件支持 
[Jenkins Plugin](https://wiki.jenkins-ci.org/display/JENKINS/OWASP+Dependency-Check+Plugin)  
[Command Line](https://github.com/jeremylong/DependencyCheck/releases)  
[Maven Plugin](http://jeremylong.github.io/DependencyCheck/dependency-check-maven)  
[Ant Task](https://jeremylong.github.io/DependencyCheck/dependency-check-ant/)



### 测试
测试命令行工具  
```
#下载 安装 
wget https://github.com/jeremylong/DependencyCheck/releases/download/v6.3.1/dependency-check-6.3.1-release.zip
unzip dependency-check-6.3.1-release.zip
cd dependency-check 
# 查看帮助 
./bin/dependency-check.sh -h
# 扫描单个文件 测试 
./bin/dependency-check.sh  --out . --scan lib/xz-1.8.jar

# 会在当前目录下产生一个html报告 
```


## 相关资源 

OWASP dependency-check  
http://jeremylong.github.io/DependencyCheck/
OWASP dependency-track  
https://github.com/stevespringett/dependency-track  
OWASP dependency-check-sonar-plugin  
https://github.com/stevespringett/dependency-check-sonar-plugin



## Dependency Track

## www.contrastsecurity.com

## Sonatype.com 

## Safety 

主要检测python的安全依赖安全性，并且有在线网站 
https://pyup.io/docs/usage-examples/

https://github.com/pyupio 


## 参考 
[pci 3.0 第三方漏洞管理](https://www.pcisecuritystandards.org/documents/PCI_DSS_v3.pdf)
[cwe已知漏洞描述](http://cwe.mitre.org/data/definitions/937.html)