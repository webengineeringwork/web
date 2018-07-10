# Web engineering
## 成员名单
| 学号 | 姓名 |
| - | :-: |
| 15130110071 | 范中豪 |
| 15130110067 | 高 杰 |
| 15130110076 | 吴家明 |
| 15130110090 | 周 璇 |
| 15130110036 | 吴多贤 |
| 15130110089 | 张 政 |
| 15130110105 | 陈含艺 |
| 15130110053 | 韩宝鑫 |

# 项目运行说明

## 配置开发环境

- 系统环境 : Ubuntu 16.04
- 依赖管理 : composer
- PHP版本 : PHP 7.0.30
- 数据库 : MySQL 5.7.22
- PHP扩展 :
	- OpenSSL PHP Extension
	- PDO PHP Extension
	- Mbstring PHP Extension
	- Tokenizer PHP Extension
	- XML PHP Extension
	- Ctype PHP Extension
	- JSON PHP Extension
	- MySQL PHP Extension

## 配置运行环境

- 系统环境 : CentOS 7
- Web服务器 : Nginx
- PHP进程管理 : php-fpm
- 依赖管理 : composer
- PHP版本 : PHP 7.0.30
- 数据库 : MySQL 5.7.22
- PHP扩展 :
	- OpenSSL PHP Extension
	- PDO PHP Extension
	- Mbstring PHP Extension
	- Tokenizer PHP Extension
	- XML PHP Extension
	- Ctype PHP Extension
	- JSON PHP Extension
	- MySQL PHP Extension

## 运行项目
1. clone项目源码 : git clone https://github.com/webengineeringwork/bookshop
2. 安装依赖 : composer install
3. 拷贝.env.example文件为.env，配置数据库
4. 在MySQL中创建一个.env中配置的数据库
5. 生成laravel key : php artisan key:generate
6. 自动化创建项目中的数据库表 : php artisan migrate
7. 测试运行项目 : php artisan serve
8. 浏览器打开 localhost:8000查看运行结果
9. 在nginx中的server中进行相关配置，并将root指向项目的public目录

## 具体操作说明

### 安装依赖管理
- Ubuntu : `sudo apt-get install composer`
- CentOS : `yum install composer`

### 安装php及相关扩展
将最新的PHP安装源加入到系统的软件源中，Ubuntu使用ppa方式添加软件源，CentOS在`/etc/yum.repos.d/`中添加PHP对应的源

Ubuntu安装扩展`sudo apt-get install php php-fpm php-openssl php-pdo php-mbstring php-tokenizer php-xml php-ctype php-json php-mysql`

CentOS安装扩展`yum install -y php php-fpm php-openssl php-pdo php-mbstring php-tokenizer php-xml php-ctype php-json php-mysql`

检测相关扩展是否安装成功 : `php -m`查看PHP当前支持的扩展

### 开发环境运行
`php artisan serve`，运行后会提示访问localhost:8000查看项目的运行结果

### 运行环境操作
在nginx中为项目添加一个server，将root指向项目的public，其他具体配置查看nginx官网文档
nginx配置完成后浏览器访问server中配置的server_name

