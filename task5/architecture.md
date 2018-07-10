# web应用架构
## 架构模式  
&nbsp;&nbsp;&nbsp;&nbsp;项目采用MVC设计模式，实现视图，控制器和模型地分离。  
具体实施步骤：  
1. 浏览器发出http请求  
2. nginx服务器接收请求，向php进程管理器发出请求  
3. php-fpm发出php shell命令  
4. php shell访问mysql数据库，得到结果，将结果返回  
5. nginx得到结果，定位即将转向地html模板  
6. php shell利用得到地结果填充模板  
7. 填充后的模板返回给nginx服务器，服务器返回给浏览器  

![](architecture.png)  

## 详细导向过程
1. 浏览器发出http请求  
2. 路由找到对应地控制器  
3. 控制器进行功能调用从模型中得到结果  
4. 控制器利用得到地结果填充html模板  
5. 填充后的html界面返回给浏览器  
![](architecture2.png)  