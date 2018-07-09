# 为Web添加安全防护策略

<p>安全在web开发中十分重要，虽然属于非功能需求，但必须在前期就开始考虑。否则,严重的安全漏洞会导致用户隐私泄露，而且基本架构确定后很难再进行较大调整。那么就需要进行PHP安全防护措施</p>  

<h2>PHP登录用户基本信息</h2>  

<p>对于Web应用最基本的就是进行用户的信息的验证，包括进行数据库表单一致性校验和非法字符校验。另外由于当前简单的密码序列安全性较低，还需要进行密码加密操作。如下是对用户名和密码的验证，以及将当前密码进行简单加密。  
＜?php  
$enteredPassword.  
$salt = substr($enteredPassword, 0, 2);  
$userPswd = crypt($enteredPassword, $salt);  
// $userPswd然后就和用户名一起存储在MySQL 中  
crypt()和Apache的口令-应答验证系统的应用  
$username = "HBX"; //用户名  
$password = "123456798"; //密码  
$db = "users"; //数据库名  
// 设置是否通过验证标志，默认为否  
$authorization = 0;  
// 提示用户输入帐号和密码  
if (isset($PHP_AUTH_USER) &amp;&amp; isset($PHP_AUTH_PW)){  
　　mysql_pconnect($host, $username, $passwd) or die("不能连接到MySQL服务器!");  
　　mysql_select_db($db) or die("不能选择数据库！");  
　　// 进行加密  
　　$salt = substr($PHP_AUTH_PW, 0, 2);  
　　$encrypted_pswd = crypt($PHP_AUTH_PW, $salt);  
　　//SQL查询语句  
　　$query = "SELECT username FROM members WHERE username = \'$PHP_AUTH_USER\' AND password = \'$encrypted_pswd\'";  
　　// 执行查询  
　　if (mysql_numrows(mysql_query($query)) == 1) {  
$authorization = 1;  
　　}  
}  
if (! $authorization){  
　　header(\'WWW-Authenticate: Basic realm="用户验证"\');  
　　header(\'HTTP/1.0 401 Unauthorized\');  
　　print "无法通过验证";  
　　exit;  
}else {  
　print "已经加密";}  
?＞  
MD5加密是当前比较安全的加密方式，而且由于PHP内置md5()混编函数，所以只需要调用该类即可使用，简单例子如下  
＜php  
$input = "Hello,PHP world!";  
$output = md5($input);  
print "输出: $output ";  
?＞  
当然为了防止暴力破解，还需要进行验证码验证。由于验证码代码庞大，而且完全可以加载挂件来实现就不多做解释。当然类似于验证码的还有手机验证、第三方多人验证等等。</p>  

<h2>数据库安全问题</h2>  

<p>虽然PHP 本身并不能保护数据库的安全，但是我们用可以PHP对数据库进行基本操作。而且有一条原则是：深入防御。对于数据库这种类型的采取较多的措施更能提高其安全性。</p>  

<h3>1数据库权限</h3>  

<p>要禁止用户直接对数据库进行操作，因为用户的不当操作及恶意攻击都可能导致数据库异常或者信息泄露。并且在对数据库权限分配时，要尽可能的低且不影响用户体验，还要尽可能不要对其他用户数据产生直接影响。</p>  

### 2.数据库连接
把连接建立在 SSL 加密技术上可以增加客户端和服务器端通信的安全性，或者 SSH也可以用于加密客户端和数据库之间的连接。如果使用了这些技术的话，攻击者要监视服务器的通信或者得到数据库的信息就会变得困难。  
### 3.数据库数据的加密
SSL/SSH 能保护客户端和服务器端交换的数据，但 SSL/SSH 并不能保护数据库中已有的数据。SSL 只是一个加密网络数据流的协议。如果攻击者取得了直接访问数据库的许可（绕过 web 服务器），敏感数据就可能暴露或者被滥用，除非数据库自己保护了这些信息。对数据库内的数据加密是减少这类风险的有效途径，但是只有很少的数据库提供这些加密功能。对于这个问题，有一个简单的解决办法，就是创建自己的加密机制。  
<?php <br>
$query = sprintf("INSERT INTO users(name,pwd) VALUES('%s','%s');", <br>
addslashes($username), md5($password)); <br>
$result = pg_query($connection, $query);  </p>

<p>$query = sprintf("SELECT 1 FROM users WHERE name='%s' AND pwd='%s';", <br>
addslashes($username), md5($password)); <br>
$result = pg_query($connection, $query);  </p>

<p>if (pg_num_rows($result) > 0) { <br>
echo 'Welcome, $username!'; <br>
} else { <br>
echo 'Authentication failed for $username.'; <br>
} <br>
?> </p>

<h2>防SQL注入攻击方面</h2>

<p>一般的php默认配置文件是在conf/php.ini通过配置php.ini防止php脚本和SQL攻击</p>

<h3>(1) php的安全模式</h3>

<p>php的安全模式是个非常重要的内嵌的安全机制，能够控制一些php中的函数，比如system()，
同时把很多文件操作函数进行了权限控制，但是默认的php.ini是没有打开安全模式的，将其打开：safe_mode = on</p>

<h3>(2) 用户组安全</h3>

<p>当safe_mode打开时，safe_mode_gid被关闭，那么php脚本能够对文件进行访问，而且相同组的用户也能够对文件进行访问。  
建议设置为：safe_mode_gid = off</p>

<h3>(3) 安全模式下执行程序主目录和文件</h3>

<p>如果安全模式打开了，但是却是要执行某些程序的时候，可以指定要执行程序的主目录和文件：  
safe_mode_exec_dir = D:/usr/bin  
safe_mode_include_dir = D:/usr/www/include/</p>

<h3>(4) 控制php脚本能访问的目录</h3>

<p>使用open_basedir选项能够控制PHP脚本只能访问指定的目录，这样能够避免PHP脚本访问，我们一般可以设置为只能访问网站目录：open_basedir = D:/usr/www</p>

<h3>(5) 关闭PHP版本信息在http头中的泄漏</h3>

<p>我们为了防止黑客获取服务器中php版本的信息，可以关闭该信息斜路在http头中：  
expose_php = Off</p>

<h3>(6) 关闭注册全局变量</h3>

<p>在PHP中提交的变量，包括使用POST或者GET提交的变量，都将自动注册为全局变量，能够直接访问，这是对服务器非常不安全的，所以把注册全局变量选项关闭：register_globals = Off</p>

<h3>(7) 打开magic_quotes_gpc来防止SQL注入</h3>

<p>SQL注入是非常危险的问题，小则网站后台被入侵，重则整个服务器沦陷。php.ini中有一个设置：magic_quotes_gpc =  On。这个默认是关闭的，打开后将自动把用户提交对sql的查询进行转换，</p>

<h3>(8) 错误信息控制</h3>

<p>一般php在没有连接到数据库或者其他情况下会有提示错误，一般错误信息中会包含php脚本当前的路径信息或者查询的SQL语句等信息，很不安全，所以一般服务器禁止错误提示：display_errors = Off</p>

<h3>(9) 错误日志</h3>

<p>关闭display_errors后能够把错误信息记录下来，便于查找服务器出错原因：
log_errors = Off。同时也要设置错误日志存放的目录，和apache的日志存在一起：
error_log = D:/usr/local/apache2/logs/php_error.log</p>
