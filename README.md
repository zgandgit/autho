# autho
+ index.html网站首页
+ login.html授权页面
+ autho.html授权回调页面
# indes.html
```
<!doctype html>
<html>
<head>
    <meta property="wb:webmaster" content="18bc8c57bb236b22" />
    <meta charset="utf-8">
    <title>第三方登录测试</title>
    <meta name="author" content="第三方登录测试" />
    <meta name="description" content="第三方登录测试" />
    <link rel="stylesheet" href="//cdn.bootcss.com/bootstrap/3.3.4/css/bootstrap.min.css">
    <script src="//cdn.bootcss.com/jquery/1.11.2/jquery.min.js"></script>
    <script src="//cdn.bootcss.com/bootstrap/3.3.4/js/bootstrap.min.js"></script>
    <style>
        .mian{
            text-align: center;
            padding-top: 200px;
            display: flex;
            flex-direction: column;
            align-items: center;
        }
        .mian .login{
            background: red;
            color: #fff;
            cursor:pointer
        }
        .touxiang{
            width: 56px;
            height: 56px;
            border-radius: 100%;
            background: url("https://ss2.baidu.com/6ONYsjip0QIZ8tyhnq/it/u=392733608,842337915&fm=58");
            background-size: cover;
            margin-bottom: 60px;
        }
        .none{
            display: none;
        }
    </style>
</head>
<body>
<div class="mian">
    <div class="touxiang none"></div>
    <div><button class="login none" onclick="weiboLogin()">微博登录</button></div>
</div>

<script type="text/javascript">
    $(function (e) {
       //这里需要检测是否已经登录
       isLogin()?$('.touxiang.none').show():$('.login.none').show();
    });
    function weiboLogin(){
        let openUrl = "login.html?appid=1234567&redirect_uri=index.html&code=weibo";//打開地址
        var autho = window.open(
            openUrl,
            '',
            'height=400,width=600,top=200,left=200,toolbar=no,menubar=no,scrollbars=no,resizable=no,location=no,status=no'
        );
        var loop = setInterval(function() {
            if(autho.closed) {
                clearInterval(loop);//停止循环执行
                if(isLogin()){
                    location.reload()
                }
            }
        }, 1000);
    }
    //登录检测方法
    const isLogin = function () {
        let autho = localStorage.getItem('autho');
        return autho ? true:false;
    }
</script>
</body>
</html>
```
# login.html
```<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>微博授权登录中心</title>
    <style>
        .main{
            display: flex;
            justify-content: space-evenly;
            height: 400px;
            align-items: center;
        }
        .login{
            width: 80px;
            height: 36px;
            cursor:pointer
        }

    </style>
</head>
<body>
<div class="main">
    <button class="login" onclick="confirm()">确认授权</button>
    <button class="login" onclick="cancel()">取消</button>
</div>

</body>
<script>
    //确认授权
    function confirm() {
      window.location.href = 'autho.html?appid=1234567';//调转
    }
    //取消
    function cancel() {
        //关闭窗口
        window.close();
    }

</script>
</html>
```
# autho.html
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>授权回调页面</title>
    <link rel="stylesheet" href="//cdn.bootcss.com/bootstrap/3.3.4/css/bootstrap.min.css">
    <script src="//cdn.bootcss.com/jquery/1.11.2/jquery.min.js"></script>
    <script src="//cdn.bootcss.com/bootstrap/3.3.4/js/bootstrap.min.js"></script>
</head>
<body>
<div class="main">
 <h2>这里是授权回调</h2>
</div>
</body>
<script>
    $(function (e) {
        let appid = GetQueryString('appid');
        alert("你的appid为"+appid);
        if(appid){
            localStorage.setItem('autho',appid);//保存登录状态
            window.close();
        }
    });

    function GetQueryString(name)
    {
        var reg = new RegExp("(^|&)"+ name +"=([^&]*)(&|$)");
        var r = window.location.search.substr(1).match(reg);
        if(r!=null)return  unescape(r[2]); return null;
    }

</script>
</html>
```



