## 超星学习通自动签到
原作者`https://www.bilibili.com/video/av94208525`食用方法`https://yuban10703.xyz/archives/527`  
有问题直接issue提,别在blog写.....  
<!-- 填上账号密码运行运行就好了,别忘记装模块   -->
复制`account.json.example`到`account.json`并填写其中的内容  
支持学习通的所有签到  
支持server酱推送  
能自定义照片,地址以及名字  
用screen后台挂住就好了,断网的话可能会boom(一般不会断网⑧..)  
<!-- <img src="https://cdn.jsdelivr.net/gh/yuban10703/BlogImgdata/img/20200325023103.png"/> -->
<!-- <img src="https://cdn.jsdelivr.net/gh/yuban10703/BlogImgdata/img/20200325023145.jpg"/> -->
![test1](https://cdn.jsdelivr.net/gh/yuban10703/BlogImgdata/img/20200325023103.png)
![test2](https://cdn.jsdelivr.net/gh/yuban10703/BlogImgdata/img/20200325023145.jpg)

## 云函数版本使用教程
1. 去腾讯云创建一个云函数,目前测试广州好像不能用.其他地区可以.
2. 环境选择`Python 3.6`
3. 内存最低实测`64M`可以跑.
4. 时间看个人课程数量.详细解释看下面
5. 上传`Serverless`版,还有`account.json`
6. 测试运行,成功后设定定时触发.关于如何设定Cron也可以看下面

## 关于时间设定
<script type="text/javascript" src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=default"></script>

在`Serverless`版里面,所有课程扫描/签到一次就会停止,这样做是为了符合云函数的设计,同时也节省运行时间.  
最简单的估算办法就是:
* 签到或扫描的请求撑死不会超过2S,记为`x`
* 中间等待时间由`account.json`决定,记为`y`
* 课程数量,记为`z`
那么最后总时间应该是:   
$$z*(x+y)$$

绝大多数情况下这个请求时间可以直接忽略.

再懒人一点的话,可以直接在命令行运行并统计时间,参考Linux命令`time` 

## 关于Cron设定
我们先来看一个例子

```
0 */10 8-12,14-18 * * 1-5
```

云函数的Cron跟Linux Cron非常相似,不过左边多了一个秒.  
以上Cron可以解释为: 

```
0 */10 8-12,14-18 * * 1-5
| |    |          | |  |
| |    |          | |  .------- 在周几     (周一到周五)
| |    |          | .---------- 在哪个月   (所有)  
| |    |          .------------ 在哪一天   (所有)
| |    .----------------------- 在几点     (8-12和14-18) 
| .---------------------------- 在第几分钟 (每隔10分钟)
.------------------------------ 在第几秒   (第0秒)
```
这是比较符合现在上课的情况的Cron,各位也可以根据这个列表进行自定义