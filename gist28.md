[猎聘 requests post ](https://www.zhihu.com/question/54320433)
```js
作者：陈二白
链接：https://www.zhihu.com/question/54320433/answer/140348520
来源：知乎
著作权归作者所有，转载请联系作者获得授权。

from requests import Session

import md5

passwd = "passwod"
username = "username"

mpass = md5.new()
mpass.update(passwd)
passwd = mpass.hexdigest()

session = Session()
headers = {'Host': 'passport.liepin.com',
           'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; WOW64; rv:50.0) Gecko/20100101 Firefox/50.0',
           'Accept': 'application/json, text/javascript, */*; q=0.01',
           'Accept-Language': 'zh-CN,zh;q=0.8,en-US;q=0.5,en;q=0.3',
           'Accept-Encoding': 'gzip, deflate, br',
           'Content-Type': 'application/x-www-form-urlencoded',
           'X-Alt-Referer': 'https://www.liepin.com/',
           'X-Requested-With': 'XMLHttpRequest',
           'Referer': 'https://passport.liepin.com/ajaxproxy.html',
           'Content-Length': '117',
           'Cookie': '',
           'Connection': 'keep-alive'
           }

需要导出一下他的 证书文件，这个请网上搜索
content = session.post("https://passport.liepin.com/c/login.json?__mn__=user_login",
                       data=dict(layer_from='wwwindex_rightbox_new',
                                 user_login=username,
                                 user_pwd=passwd,
                                 chk_remember_pwd="on"), verify="/path/to/veryfy.crt",
                       headers=headers)
print content.text
```
[干货 | 28 张相见恨晚的速查表（完整版）——（ Python 速查表 | 机器学习、R 、概率论、大数据速查表）](https://zhuanlan.zhihu.com/p/25778895)
http://link.zhihu.com/?target=https%3A//pan.baidu.com/s/1cCdC8Q 
[itchat+pillow实现微信好友头像爬取和拼接](https://zhuanlan.zhihu.com/p/25782937)
http://link.zhihu.com/?target=https%3A//github.com/15331094/wxImage  
```js
from numpy import *
import itchat
import urllib
import requests
import os

import PIL.Image as Image
from os import listdir
import math

itchat.auto_login(enableCmdQR=True)

friends = itchat.get_friends(update=True)[0:]

user = friends[0]["UserName"]

print(user)

os.mkdir(user)

num = 0

for i in friends:
    img = itchat.get_head_img(userName=i["UserName"])
    fileImage = open(user + "/" + str(num) + ".jpg",'wb')
    fileImage.write(img)
    fileImage.close()
    num += 1

pics = listdir(user)

numPic = len(pics)

print(numPic)

eachsize = int(math.sqrt(float(640 * 640) / numPic))

print(eachsize)

numline = int(640 / eachsize)

toImage = Image.new('RGBA', (640, 640))


print(numline)

x = 0
y = 0

for i in pics:
    try:
        #打开图片
        img = Image.open(user + "/" + i)
    except IOError:
        print("Error: 没有找到文件或读取文件失败")
    else:
        #缩小图片
        img = img.resize((eachsize, eachsize), Image.ANTIALIAS)
        #拼接图片
        toImage.paste(img, (x * eachsize, y * eachsize))
        x += 1
        if x == numline:
            x = 0
            y += 1


toImage.save(user + ".jpg")


itchat.send_image(user + ".jpg", 'filehelper')
```
[Python爬虫实战入门六：提高爬虫效率—并发爬取智联招聘](https://zhuanlan.zhihu.com/p/24930071)
```js
import requests
from bs4 import BeautifulSoup
import re

url = 'http://sou.zhaopin.com/jobs/searchresult.ashx?jl=全国&kw=python&p=1&kt=3'
wbdata = requests.get(url).content
soup = BeautifulSoup(wbdata, 'lxml')

items = soup.select("div#newlist_list_content_table > table")
count = len(items) - 1
# 每页职位信息数量
print(count)

job_count = re.findall(r"共<em>(.*?)</em>个职位满足条件", str(soup))[0]
# 搜索结果页数
pages = (int(job_count) // count) + 1
print(pages)

import requests
from bs4 import BeautifulSoup
from multiprocessing import Pool

def get_zhaopin(page):
    url = 'http://sou.zhaopin.com/jobs/searchresult.ashx?jl=全国&kw=python&p={0}&kt=3'.format(page)
    print("第{0}页".format(page))
    wbdata = requests.get(url).content
    soup = BeautifulSoup(wbdata,'lxml')

    job_name = soup.select("table.newlist > tr > td.zwmc > div > a")
    salarys = soup.select("table.newlist > tr > td.zwyx")
    locations = soup.select("table.newlist > tr > td.gzdd")
    times = soup.select("table.newlist > tr > td.gxsj > span")

    for name, salary, location, time in zip(job_name, salarys, locations, times):
        data = {
            'name': name.get_text(),
            'salary': salary.get_text(),
            'location': location.get_text(),
            'time': time.get_text(),
        }
        print(data)
http://zmister.com/  https://www.zhihu.com/people/zmister/pins/posts
if __name__ == '__main__':
    pool = Pool(processes=2) 实例化一个进程池，设置进程为2；
    pool.map_async(get_zhaopin,range(1,pages+1))调用进程池的map_async()方法，接收一个函数(爬虫函数)和一个列表(url列表)
    pool.close()
    pool.join()
```
[wxpy: 优雅的微信个人号](https://www.v2ex.com/t/343685)
```js
WebQQ 不过 tody.ml/webqq/ 用来做广西联通流量自动充值
@hug.get('/send_msg')
def private_msg(content, username:hug.types.text="filehelper"):
nameArr = username.split()
name = '';
for i in range(len(nameArr)):
name = nameArr[i]
print("users:{name} content : {content}".format(**locals()))
itchat.send_msg(content, toUserName=name)
return '{"result":1}'
hug 可实现 api 接口了，这样通用性更好，可以给其他服务调用，非常简洁 双开 APP ，一天开一下小号就行 
无责任推荐双开工具: http://parallel-app.com/  PHP 版本的 https://www.v2ex.com/t/335534
itchat 的用例，有一些只需要修改变量就可以直接使用了，比如直接加群主填写特定验证信息自动邀请加群的。 
https://github.com/discountry/itchat-examples 
希望楼主有空研究研究怎么处理红包或其他特殊类消息。
from wxpy import get_wechat_logger

# 获得 Logger
logger = get_wechat_logger()

# 发送警告
logger.warning('这是一条 WARNING 等级的日志！')

# 捕获可能存在的异常，并发送
try:
    1 / 0
except:
    logger.exception('又出错啦！')
```

[用微信控制深度学习训练：中国特色的keras插件](https://zhuanlan.zhihu.com/p/25670072)
http://link.zhihu.com/?target=https%3A//github.com/QuantumLiu/wechat_callback  
[构建一个pip安装的车辆路径显示的Python包](https://zhuanlan.zhihu.com/p/25857134)
  pip install carpathview -i https://pypi.python.org/pypi
[google sheet excel](https://www.zhihu.com/question/47883186/answer/151846965)
实现抓取的函数是：=IMPORTXML(“URL”,”Xpath expression”)
常见的函数：=IMPORTHTML(“URL”,”QUERY”, Index)
![xx](https://pic3.zhimg.com/v2-e76fef9a28340d80ebe6754100f8db26_b.png)
其中，Xpath expression就是你粘贴过来的那部分代码，需要注意的是，代码中“”号需要变成‘’号：
[图片转字符](https://helloacm.com/tools/image-to-ascii/)
[Python 安装库的姿势](https://www.v2ex.com/t/348386#reply36)
去 Pypi 下载.whl 文件  pandas 就够了
然后 pip install *.whl 可以选择 Anaconda ，作为一个独立的 Python 发行版，它有巨大的预编译仓库。
http://www.lfd.uci.edu/~gohlke/pythonlibs/ windows 老老实实 anaconda  apt install (python-lxml | python3-lxml) 
弱类型：
> "1"+2
'12'
强类型：
>>> "1"+2
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: cannot concatenate 'str' and 'int' objects
动态类型：
>>> a = 1
>>> type(a)
<type 'int'>
>>> a = "s"
>>> type(a)
<type 'str'>
静态类型：
Prelude> let a = "123" :: Int

<interactive>:2:9:
    Couldn't match expected type `Int' with actual type `[Char]'
    In the expression: "123" :: Int
    In an equation for `a': a = "123" :: Int
###[在微信中监控你的 Python 程序](https://zhuanlan.zhihu.com/p/25768417)
```js
from wxpy import get_wechat_logger

# 获得 Logger
logger = get_wechat_logger()

# 发送警告
logger.warning('这是一条 WARNING 等级的日志！')

# 捕获可能发生的异常，并发送
try:
    1 / 0
except:
    logger.exception('又出错啦！')

 
```
[零基础如何学爬虫技术？](https://www.zhihu.com/question/47883186/answer/151959866)
双击图标，打开Excel，  依次点击，数据-从网站 在弹出的对话框中，输入目标网址，Games sales https://link.zhihu.com/?target=https%3A//steamspy.com/   ，点击转到，go  等待网页加载，点击你需要的数据区域，点击导入，import
然后会弹出一个数据存放区域的对话框，随便找个地方，点击ok
知乎数据爬取的部分采用了github repo: 7sDream/zhihu-oauth  https://link.zhihu.com/?target=https%3A//github.com/7sDream/zhihu-oauth

https://link.zhihu.com/?target=https%3A//gephi.org/  数据可视化，随手找了个开源软件 图像生成采用了Gephi
[效率君推荐|五个超级实用的政府网站](https://zhuanlan.zhihu.com/p/25814366)
[如何批量下载 A 股招股说明书？](https://www.zhihu.com/question/21872457/answer/152010418)
https://github.com/tsauliu/ipofiles/blob/master/pyscrapy.py
```js
#-*-coding:utf-8 -*-
import sys
import pprint
reload(sys)
sys.setdefaultencoding('utf-8')
import requests
urldict={}
import os
try:
    os.mkdir('./output')
except:
    pass

#从巨潮资讯解析出pdf的真实下载地址
f=open('stkcd.csv','r')
fout=open('./output/urls.csv','w')
for line in f:
    stkcd = str(line[:6])
    # 这一行把“招股说明书”换成　“年报”　“半年报”　之类的，即可批量下载其他的公告
    response=requests.get('http://www.cninfo.com.cn/cninfo-new/fulltextSearch/full?searchkey='+stkcd+'+招股说明书&sdate=&edate=&isfulltext=false&sortName=nothing&sortType=desc&pageNum=1')
    dict=response.json()
    for i in dict['announcements']:
        if '摘要'.decode('utf-8') not in i['announcementTitle']:
            print i['announcementTitle']
            url='http://www.cninfo.com.cn/'+i['adjunctUrl']
            print url
            secname=i['secName']
            date=i['adjunctUrl'][10:20]
            urldict.update({stkcd+'-'+secname+'-'+date:url})
            csvtowrite=stkcd+','+secname+','+date+','+url+'\n'
            fout.write(csvtowrite.encode('gbk'))
pprint.pprint(urldict)
fout.close()

#根据解析出的pdf地址下载到output，并重命名成有规律的文件
import urllib2
for name in urldict:
    url=urldict[name]
    response = urllib2.urlopen(url)
    file = open('./output/'+name+".pdf", 'wb')
    file.write(response.read())
    file.close()
    print name
```
[如何找到适合需求的 Python 库?](https://www.zhihu.com/question/26909125/answer/152114857)
https://awesome-python.com/
[腾讯QQ号被永久冻结，怎么办？](https://www.zhihu.com/question/26416069/answer/129736018)
嘿嘿，幸亏遇到我，告诉你一个百分百解决方法。首先冻结了不要对腾讯公司有任何解冻的幻想，打电话都是机器人自动录音的。找人工得花几百块电话费还是解决不了问题。骂死客服一整天，他们依然不会解决冻结。客服根本没有权利，只是训练好话术的拒绝你解决奴隶而已。你可以买车票飞机票去腾讯公司解决有可能解冻，这样太浪费钱了。告诉你一个百分百解冻方法 ，如果你的QQ有付费业务最好，没有开通业务就充个QQ会员一个月。然后直接打工信部电话或者工信部邮箱或者网站投诉，投诉他们要求退QQ付费业务款项，或者说我的苹果手机锁了，需要QQ邮箱进不了，苹果手机永久不能使用了，然后你反馈上去，腾讯吃不了兜着走。反正我的就是通过工信部网站投诉，腾讯立刻解冻，官大一级压死腾讯了。不投诉白不投诉，老百姓血汗钱整天难道容他们在办公室聊天，上网，喝茶，喝咖啡，看报纸吗。
[Python 程序如何高效地调试？](https://www.zhihu.com/question/21572891/answer/26046582)
import sys

class ExceptionHook:
    instance = None

    def __call__(self, *args, **kwargs):
        if self.instance is None:
            from IPython.core import ultratb
            self.instance = ultratb.FormattedTB(mode='Plain',
                 color_scheme='Linux', call_pdb=1)
        return self.instance(*args, **kwargs)

sys.excepthook = ExceptionHook()
然后在你的项目代码某个地方 import crash_on_ipy 就可以了
[程序员必备的好网站有哪些？](https://www.zhihu.com/question/20783828/answer/152069059)
清华大学开源软件镜像站 https://link.zhihu.com/?target=https%3A//mirrors.tuna.tsinghua.edu.cn/
[python小甜点-----100行代码实现个性化翻译](https://zhuanlan.zhihu.com/p/25818996)
```js
pip install translate pip install translate-html Windows 可以试试 Python -m translate
>translate "<div>my</div><table name='this is a name' style='width: 100px;color:blue'>The style property ins
ide the tag  won't be translate</table>"

<div>我的</div><table name='this is a name' style='width: 100px;color: blue'>标签内的样式属性不会被翻译</table>
translate -pl proxy_list -pa user:password name
名称
# proxy_list是一个文件地址，里面每一行是一个代理，可以配置多个代理，格式如下
192.168.1.12:12345
192.168.2.12:12345
...
# pa 参数指的代理的用户验证，没有可以不写。
>>> from translate import Translate
>>> t = Translate()
>>> t.translate("name")
'名称'
```
[实时展示Github排名](https://link.zhihu.com/?target=http%3A//awesomegithub.com)
https://link.zhihu.com/?target=https%3A//github.com/hating/AwesomeGithub
[识别山寨不再难！快用Python爬评论，无需再等315](https://zhuanlan.zhihu.com/p/25786182)
```js
作者：[已重置]
链接：https://zhuanlan.zhihu.com/p/25786182
来源：知乎
著作权归作者所有，转载请联系作者获得授权。

import requests
import json
import time
import simplejson

headers = {
'Connection': 'keep-alive',
'User-Agent': 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10.12; rv:51.0) Gecko/20100101 Firefox/51.0'
}
base_url = 'https://rate.tmall.com/list_detail_rate.htm?itemId=38975978198&' \
'spuId=279689783&sellerId=92889104&order=3&callback=jsonp698'
#在base_url后面添加&currentPage=1就可以访问不同页码的评论

for i in range(2, 98, 1):
url = base_url + '&currentPage=%s' % str(i)
#将响应内容的文本取出
tb_req = requests.get(base_url, headers=headers).text[12:-1]
print(tb_req)

#将str格式的文本格式化为字典
tb_dict = simplejson.loads(tb_req)

#编码： 将字典内容转化为json格式对象
tb_json = json.dumps(tb_dict, indent=2) #indent参数为缩紧，这样打印出来是树形json结构，方便直观
#print(type(tb_json))

#print(tb_json)
#解码： 将json格式字符串转化为python对象
review_j = json.loads(tb_json)
for p in range(1, 20, 1):
print(review_j["rateDetail"]["rateList"][p]['rateContent'])
time.sleep(1)


```


[发送日志到微信](http://sc.ftqq.com/3.version)
https://www.v2ex.com/t/348105  Sentry 短信+邮件+内部 APP
常用告警方式邮件、短信、语音、钉钉、微信大概这些，都有接口调用的方式
 参考 mojo-weixin github 上有这个项目
[利用 [微信公众号通知] 给你的网站加上异常提醒吧](https://laravel-china.org/articles/3854/the-use-of-wechat-public-notice-to-your-web-site-with-unusual-reminder)
https://github.com/HanSon/wechat-notice  https://github.com/sjdy521/Mojo-Weixin  使用Perl语言（不会没关系）编写的微信/weixin/wechat客户端框架
[DBA的40条军规](https://mp.weixin.qq.com/s/PtIaqAjs298uH6edEYb2xg)
```js
删除默认空密码账号。

delete from mysql.user where user='' and password='';
flush privileges;

使用InnoDB存储引擎。

支持事务，行级锁，更好的恢复性，高并发下性能更好。
InnoDB表避免使用COUNT(*)操作，因内部没有计数器，需要一行一行累加计算，计数统计实时要求较强可以使用memcache或者Redis。
所有表和字段都需要添加中文注释。 避免使用外键，外键用来保护参照完整性，可在业务端实现。

外键会导致父表和子表之间耦合，十分影响SQL性能，出现过多的锁等待，甚至会造成死锁。
禁止使用分区表。查询的字段必须是分区键，否则会遍历所有的分区表，并不会带来性能上的提升。此外，分区表在物理结构上仍旧是一张表，此时我们更改表结构，一样不会带来性能上的提升。所以应采用切表的形式做拆分，如程序上需要对历史数据做查询，可通过union all的方式关联查询。

用DECIMAL代替FLOAT和DOUBLE存储精确浮点数。
mysql> CREATE TABLE t3 (c1 float(10,2),c2 decimal(10,2));       
Query OK, 0 rows affected (0.05 sec)
>mysql> insert into t3 values (999998.02, 999998.02);    
Query OK, 1 row affected (0.01 sec)
>mysql> select * from t3;
+-----------+-----------+
| c1        | c2        |
+-----------+-----------+
| 999998.00 | 999998.02 |
采用tinyint完全可以满足需要，int占用的是4字节，而tinyint才占用1个字节。
int(10)和int(1)没有什么区别，10和1仅是宽度而已，在设置了zerofill扩展属性的时候有用
create table test(id int(10) zerofill,id2 int(1));insert into test values(1,1);
insert into test values(1000000000,1000000000);
select * from test;
+------------+------------+
| id         | id2        |
+------------+------------+
| 0000000001 |          1 |
| 1000000000 | 1000000000 |
查询的字段必须创建索引。

如：1、SELECT、UPDATE、DELETE语句的WHERE条件列；2、多表JOIN的字段。
低效查询
  SELECT * FROM t WHERE name LIKE '%de%';
  ----->
  高效查询
  SELECT * FROM t WHERE name LIKE 'de%';
联合索引IX_a_b_c(a,b,c) 相当于 (a) 、(a,b) 、(a,b,c)，那么索引 (a) 、(a,b) 就是多余的。
不使用SELECT *，只获取必要的字段。

消耗CPU和IO、消耗网络带宽；
无法使用覆盖索引。
36、用IN来替换OR。

低效查询
SELECT * FROM t WHERE LOC_ID = 10 OR LOC_ID = 20 OR LOC_ID = 30;
----->
高效查询
SELECT * FROM t WHERE LOC_IN IN (10,20,30);
在MySQL 5.7开始会在初始化后随即生成一个初始密码，可以在初始化日志中查找。内容类似下面的形式：

error.log:2017-02-15T15:47:15.132874+08:001 [Note] A temporary password is generated for root@localhost: Y9srj<pdn9Lj

3、在mysql.user中默认值为mysql_native_password，不再支持mysql_old_password。

>select distinct plugin from mysql.user;
+-----------------------+
|plugin                |
+-----------------------+
|mysql_native_password |
+-----------------------+
```


[php中用redis存储session](https://segmentfault.com/q/1010000008721376)
```js
php的session确实是会在一次完整的http请求结束后，才会设置session，下面是我根据redis自带的monitor监控了一下redis的状态
session_start();
$redis->auth(123456);

http请求完成以后redis中有一个setex命令，其实这个就是php内核告诉redis写入session，这是一个自动的过程，session之所以第一次没有打印出来，也是因为第一次http请求结果后，php才会把session写入redis中，第二次请求的时候session已经成功写入
session要等你请求走完了才会去写的，你要是想设置了就去写可以session_write_close ,不过这样session也就结束了
```
![imh](https://segmentfault.com/img/bVKM1o?w=806&h=273)
![o](https://segmentfault.com/img/bVKM1H?w=648&h=116)




[网页游戏常见安全问题、防御方式与挽救措施](http://www.cnxct.com/experience-with-webgame-of-security-and-defense/)
```js
充值接口的验证方式
// 返回参数列表
$signKey = array('openid','appid','ts','payitem','token','billno','version','zoneid','providetype','amt','payamt_coins','pubacct_payamt_coins');
$sign = array();
//从GET参数中，对比找出上面参数的值
foreach($signKey as $key ) {
    if (isset($data[$key]))
    {
    $sign[$key] = $data[$key];  //只有 GET里有的参数，才参与sig的计算
    }
}
######开始生成签名############
//1: URL编码 URI
$url = rawurlencode($url);
//2:按照key进行字典升序排列
ksort($sign);
//3: &拼接，并URL编码
$arrQuery = array();
foreach ($sign as $key => $val ) 
{ 
    $arrQuery[] = $key . '=' . str_replace('-','%2D',$val);
}   
$query_string = join('&', $arrQuery);
//4 以POST方式拼接 1、3 以及URL
$src = 'GET&'.$url.'&'.rawurlencode($query_string);
// ## 构造密钥
$key = $this->config->get('qq_appkey').'&';
//### 生成签名
$sig = base64_encode(hash_hmac("sha1", $src, strtr($key, '-_', '+/'), true));
if ( $sig != $data['sig'] ) {
    $return['ret'] = 4;
    $return['msg'] = '请求参数错误：（sig）';
    $this->output->set(json_encode($return));
    return ;
}

$a = 162.95;
$b = 16295;

$c = (int)($a*100);

if($c!=(int)$b)
{
	
	echo "不相等,c=".$c.",b=".$b;
	
}else{
	
	echo "相等";
	
}
$a = 162.95;
        $b = 16295;
        if ( round($a * 100, 2) != round($b, 2)){

反正先统一取2位小数，或者四舍五入再比较。
16295 转int类型就变成16294了
用户并发请求锁的实现，php中session以文件形式存储时，php会对session文件加锁，不释放(如果不特意执行session_write_close)，知道当前响应完成。另外一个线程才可以正常读取，这简介的形成了单个用户的并发请求锁，但是，后面的进程一直处于等待状态，也会占用一个php-fpm进程，阻塞其他用户的正常请求对php线程的使用。为此，我们使用NOSQL的K-V形式结构，以user_name为key的形式，实现用户并发请求锁，比如 redis的setnx接口，原子性判断操作有则返回false，，没有就添加一个，返回true。那么，对于下一个请求，setnx时，返回false，有这个key了，那么立刻抛出异常，结束响应，FLASH根据异常内容，提醒用户不要进行恶意操作。即不会发生并发请求，又不会阻塞请求处理。同时，在请求结束的析构函数里，对这个锁进行删除操作，不影响下一个正常请求。若因为程序异常，发生语法错误，导致析构函数没法执行，没有删除用户锁时，可以在生成锁的时候，设置过期时间，比如5秒，甚至2秒，利用nosql的过期机制，实现用户解锁，避免用户长时间无法正常游戏。
把UPDATE语句改为UPDATE `role_gold` SET gold = gold + 100 WHERE role_id = 1或者UPDATE `role_gold` SET gold = 150 WHERE role_id = 1 AND gold = 100来解决，但这种多个事务同时操作修改多个表的多条记录时，还容易引发死锁问题，比如《webgame中Mysql Deadlock ERROR 1213 (40001)错误的排查历程》。而且，当条件为跨表内数据是否存在，或者另外条件不在MYSQL中，而在其他网络接口的响应中时，如何做呢？
```
###[对CSRF（跨站请求伪造）的理解](https://my.oschina.net/jasonultimate/blog/212554)
```js
从一个网站A中发起一个到网站B的请求，而这个请求是经过了伪装的，伪装操作达到的目的就是让请求看起来像是从网站B中发起的，也就是说，让B网站所在的服务器端误以为该请求是从自己网站发起的，而不是从A网站发起的。当然，请求一般都是恶意的。
之所以要伪装成从B网站发起的，是因为Cookie是不能跨域发送的。结合上面这个例子来说就是：如果从A网站直接发送请求到B网站服务器的话，是无法将B网站中产生的Cookie一起发给B服务器的。

为什么非要发送Cookie呢？这是因为服务器在用户登录后会将用户的一些信息放到Cookie中返回给客户端，然后客户端在请求一些需要认证的资源的时候会把Cookie一起发给服务器，服务器通过读取Cookie中的信息来进行用户认证，认证通过后才会做出正确的响应。 攻击者盗用了你的身份，以你的名义发送恶意请求。
受害者必须依次完成两个步骤：

1.登录受信任网站A，并在本地生成Cookie。

2.在不登出A的情况下，访问危险网站B。

cookie是在你计算机里的一个文件 (看得见实物总比抽象的概念来得容易理解)， 它记录了你的用户ID，密码、浏览过的网页、停留的时间等信息，当你再次来到该网站时，网站通过读取Cookie，得知你的相关信息，就可以做出相应的动作，如在页面显示欢迎你的标语，或者让你不用输入ID、密码就直接登录等等。 Cookie就是作用解决HTTP协议无状态的缺陷提出的一种方案。值得注意的是对比Session这个概念，Cookie机制是保存客户端状态的方案，而Session是保存服务端状态的方案。
session机制都使用会话cookie来保存session id，而关闭浏览器后这个session id就消失了，再次连接服务器时也就无法找到原来的session。如果服务器设置的cookie被保存到硬盘上，或者使用某种手段改写浏览器发出的 HTTP请求头，把原来的session id发送给服务器，则再次打开浏览器仍然能够找到原来的session。

　　恰恰是由于关闭浏览器不会导致session被删除，迫使服务器为seesion设置了一个失效时间，当距离客户端上一次使用session的时间超过这个失效时间时，服务器就可以认为客户端已经停止了活动，才会把session删除以节省存储空间。

存在CSRF漏洞Html代码：
<form action="Transfer.php" method="POST">
　　　　<p>ToBankId: <input type="text" name="toBankId" /></p>
　　　　<p>Money: <input type="text" name="money" /></p>
　　　　<p><input type="submit" value="Transfer" /></p>
</form>

以上漏洞的攻击代码：
<form method="POST" action="http://www.Bank.com/Transfer.php">
<input type="hidden" name="toBankId" value="hyddd">
<input type="hidden" name="money" value="10000">
</form>
<script>
document.usr_form.submit();
</script>

 

      如果用户在登陆www.Bank.com后，访问带有以上攻击代码的页面，该用户会在毫不知情下，给hyddd转账10000块。这就是CSRF攻击。

2.包含不同域的Js脚本【隐患】
```
###[Selenium私房菜系列--总章](http://www.cnblogs.com/hyddd/archive/2009/05/30/1492536.html)

###[Ajax跨域](https://my.oschina.net/jasonultimate/blog/550737)
```js
客户端请求通过Nginx转发

原理：客户端的所有请求都直接发到客户端所在域名下，但是在客户端服务器增加一台nginx服务器，作为代理，如果是后端的url，直接代理转发到服务端，这样就不存在前端的跨域问题了。
举例：

server {
    listen       80;
    server_name  static.demo.com;    #可配置多个主机头
    charset utf-8,gbk,gb2312,gb18030; #可以实现多种编码识别

    location / {
        root   /home/wy/www/static.demo.com/ROOT;  #网站文件路径

        autoindex on;
        autoindex_exact_size off;
        autoindex_localtime on;
        index  default.html;
    }
    #所有/server/开头的请求都会走这里
    location /server/ {
            proxy_pass http://server.demo.com:8080;  ##转发到server
            proxy_set_header    Host             $host;
            proxy_set_header    X-Real-IP        $remote_addr;
            proxy_set_header    X-Forwarded-For  $proxy_add_x_forwarded_for;
        }
}
```
###[nginx上，http状态200响应，PHP空白返回的问题](http://www.cnxct.com/php-return-empty-result-on-nginx-without-script_filename/?utm_source=tool.lu)
在nginx配置的 fastcgt_params中加上SCRIPT_FILENAME的配置（在ubuntu的apt-get形式安装nginx配置中，默认是有这条的），比如

 
fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
###[eval gzinflate base64_decode pack gzuncompress 等php函数在线解密](http://decode.cnxct.com/)
###[正则表达式与数学](http://www.cnxct.com/%e6%ad%a3%e5%88%99%e8%a1%a8%e8%be%be%e5%bc%8f%e4%b8%8e%e6%95%b0%e5%ad%a6/)
```js
^1?$|^(11+?)\1+$ 可以判断素数（换成n个1的形式，n为数字的大小。比如5转换为11111；3转换为111；2转换为11。）
if (preg_match('/^1?$|^(11+?)\1+$/i', $subject)) {
    #不是素数
} else {
    # 是素数
}
```
###[PPT下载：web开发与运维安全浅见PPT下载](http://www.cnxct.com/web-development-security-ppt/)
###[MySQL大数据量表中删除重复记录](https://blog.skyx.in/archives/135/)
```js
CREATE TABLE `tmptable` AS (SELECT `title` FROM `info` GROUP BY `title` HAVING COUNT( `title` ) >1);
CREATE TABLE `idtable` AS ( SELECT min(a.`id`) AS id, a.`title` FROM `info` a, `tmptable` t WHERE a.`title` = t.`title` GROUP BY a.`title`);
DELETE a FROM `info` a,`idtable` t WHERE a.`id` = t.`id`;
配置ssh免密码登录linux机器
ssh-keygen -t rsa -C comment
scp id_rsa.pub root@hostname:~/id_rsa.pub
cat ~/id_rsa.pub >> ~/.ssh/authorized_keys
chmod 600 ~/.ssh/authorized_keys

用户量超过1亿：
首先最核心是数据库表设计（扩展性一定要好，这个时候应该有DBA和运维工程师出场了）
开发上 做好2点：数据服务器分离（laravel已经自带了，可配置读的数据源，写入的数据源……）
还有session的分布式管理（比如redis，memcache等等）

最后如果数据有压力了，查询要优化，DBA会叫开发一起优化调整。
如果web有压力了，运维工程师，会把多台服务器负载均衡做好，项目代码部署到 多台服务器就行了，反正session已经独立出去统一管理了。
mysql优化，一般就开启慢查询日志。
经常要去分析的，这个最好是和运维一起搞。或者你项目已经开发完成，没多少事情干，才有时间去优化
比如PHP异常和错误有什么区别？

当线上有错误的时候如何检查？

检查思路是什么？
这种类似大数据的，一句话：分而治之。
高并发的：优化业务流程，启用多级缓存等
```
###最近几天数据
```js
fixData(['2017-01-01=>100','2017-01-11=>100',],strtotime(date('Y-m-d'))-86400*7,strtotime(date('Y-m-d')),'day');

function fixData(&$data,$startTime,$endTime,$granularity){
        switch ($granularity){
            case 'minute':
                $step=60;
                $format='Y-m-d H:i:s';
                break;
            case 'hour':
                $step=3600;
                $format='Y-m-d H:i:s';
                break;
            case 'day':
                $step=86400;
                $format='Y-m-d';
                break;
            case 'week': // be unused so far
                $step=86400*7;
                $format='Y-m-d';
                break;
            default:
                $step=86400;
                $format='Y-m-d';
        }
        for($i=$startTime;$i<$endTime;$i+=$step){
            $date=date($format,$i);
            if(!isset($data[$date])){
                $data[$date]=0;
            }
        }
        ksort($data);

        return $data;
    }
```
###[php几率算法问题](https://www.zendstudio.net/archives/heartbreak-for-php-probability-algorithm/)
```js
//初始化数组 几率生成一颗（50%）、两颗（16%）和三颗（2%）宝石
$stone_arr = array( 
		array( 'num' => 1, 'prob' => '50%' ),
		array( 'num' => 2, 'prob' => '16%' ),
		array( 'num' => 3, 'prob' => '2%' )
		 );
//随机获得一个幸运数字
$luck_num = mt_rand( 0, 99 );
//初始化几率区间和最终宝石生产数目
$lucky_range = $made_num = 0;
 
foreach( $stone_arr as $sa ){
	$prob = intval( $sa['prob'] );
	if( $luck_num >= $lucky_range && $luck_num < $lucky_range + $prob ){
		$made_num = $sa['num'];
		break;
	}
	else{
		$lucky_range += $prob;
	}
}
 
for( $i = 0; $i < $made_num; $i++ ){
	//生产宝石的逻辑
}

$a = array_fill(0,50, 1);
$b = array_fill(0,16, 2);
$c = array_fill(0,2, 3);
$d = array_fill(0,32, 0);
$arr = array_merge($a, $b, $c);
//var_dump($arr);
$d = mt_rand(0,99);
echo $arr[$d];
function a(){
$n = mt_rand(0,99);
$result = 0;
if($n >= 0 && $n = 0 && $n = 2 && $n = 50 && $n <= 99){
$result = 1;
}
return $result;
}
$seed = mt_rand(1,100);
if($seed>0 and $seed50 and $seed<67){
$re =2;
}else if($seed==67 or $seed == 68){
$re =3;
}else{
$re =0;
}

function a(){
$n = mt_rand(0,99);
$result = 0;
if($n >= 0 && $n = 1){
$result = 1;
}
if($n >=2 && $n = 50 && $n <=99){
$result = 3;
}
return $result;
}
```
###[将被撤回的微信消息发送到文件传输助手](https://gist.github.com/youfou/e1ccbb4dea75790daaabac2b19ccd802)
```js
from xml.etree import ElementTree as ETree
from wxpy import *

bot = Bot()

@bot.register(msg_types=NOTE)
def get_revoked(msg):
    revoked = ETree.fromstring(msg.raw['Content']).find('revokemsg')
    if revoked:
        revoked_msg = bot.messages.search(id=int(revoked.find('msgid').text))[0]
        bot.file_helper.send(revoked_msg)

bot.start()
#利用wxpy内置的图灵机器人自动回复指定好友
from wxpy import *

bot = Bot()

tuling = Tuling('你的 API KEY (http://www.tuling123.com/)')
my_friend = ensure_one(bot.friends().search('好友的名称'))


@bot.register(my_friend, TEXT)
def tuling_reply(msg):
    tuling.do_reply(msg)


bot.start()

```


###[博大精深的农历算法PHP代码](https://www.zendstudio.net/archives/php-code-of-lunarcalendar/)
一个下划线引发的IE6不能登录的问题
后台的地址/xxxx_xxxx/login.php，居然成了/xxxx%5Fxxxx/login.php，即被urlencode了，哦！问题就出在这里了，因为我们的登录验证会严格判断登录来源，显然xxxx_xxxx不等于xxxx%5Fxxxx，所以，就认为非法用户登录了，被踢！后来，经过测试，我们在后台进行urldecode，以期不再出现%5F的情况，事实上，问题并不是出现在这里，而是IE6，该死的IE6会自作聪明的进行编码，即将下划线转成%5F
###[MYSQL性能优化分享](https://www.zendstudio.net/archives/mysql-performance-optimization-trivial/)
```js
有一个1000多万条记录的用户表members,查询起来非常之慢，同事的做法是将其散列到100个表中，分别从members0到members99，然后根据mid分发记录到这些表中
for($i=0;$i< 100; $i++ ){
	//echo "CREATE TABLE db2.members{$i} LIKE db1.members<br>";
	echo "INSERT INTO members{$i} SELECT * FROM members WHERE mid%100={$i}<br>";
}
不停机修改mysql表结构
建一个临时表：

/*创建临时表*/
CREATE TABLE members_tmp LIKE members
然后修改members_tmp的表结构为新结构，接着使用上面那个for循环来导出数据，因为1000万的数据一次性导出是不对的，mid是主键，一个区间一个区间的导，基本是一次导出5万条吧，这里略去了
接着重命名将新表替换上去：

/*这是个颇为经典的语句哈*/
RENAME TABLE members TO members_bak,members_tmp TO members;

分表的话 mysql 的partition功能就是干这个的，对代码是透明的；在代码层面去实现貌似是不合理的
```
###[php设计](https://eev.ee/blog/2012/04/09/php-a-fractal-of-bad-design/?utm_source=tool.lu)
"foo" == TRUE, and "foo" == 0… but, of course, TRUE != 0
123 == "123foo"… although "123" != "123foo"
"6" == " 6", "4.2" == "4.20", and "133" == "0133". But note that 133 != 0133, because 0133 is octal. But "0x10" == "16" and "1e3" == "1000"
 (++) a NULL produces 1. Decrementing (--) a NULL produces NULL
 array("foo", "bar") != array("bar", "foo")
array("foo" => 1, "bar" => 2) == array("bar" => 2, "foo" => 1)
 Argument order: array_filter($input, $callback) versus array_map($callback, $input), strpos($haystack, $needle) versus array_search($needle, $haystack)
 https://www.toptal.com/php/10-most-common-mistakes-php-programmers-make?utm_source=tool.lu
 
 for ($i = ord('a'); $i <= ord('z'); $i++) {
    echo chr($i) . "\n";
}$letters = range('a', 'z');

for ($i = 0; $i < count($letters); $i++) {
    echo $letters[$i] . "\n";
}

###[PHP四种基础算法详解：冒泡，选择，插入和快速排序法](http://www.hoehub.com/PHP/168.html?utm_source=tool.lu)
```js
$arr=array(1,43,54,62,21,66,32,78,36,76,39);
function bubbleSort ($arr)
{
    $len = count($arr);
    //该层循环控制 需要冒泡的轮数
    for ($i=1; $i<$len; $i++) {
        //该层循环用来控制每轮 冒出一个数 需要比较的次数
        for ($k=0; $k<$len-$i; $k++) {
            if($arr[$k] > $arr[$k+1]) {
                $tmp       = $arr[$k+1]; // 声明一个临时变量
                $arr[$k+1] = $arr[$k];
                $arr[$k]   = $tmp;
            }
        }
    }
    return $arr;
}


```
###[微信公众号JS-SDK类库](http://www.hoehub.com/PHP/183.html)
```js
class JSSDK
{
    private $appId;
    private $appSecret;
 
    public function __construct($appId, $appSecret)
    {
        $this->appId = $appId;
        $this->appSecret = $appSecret;
    }
 
    public function getSignPackage()
    {
        $jsapiTicket = $this->getJsApiTicket();
        // 注意 URL 一定要动态获取，不能 hardcode.
        $protocol = !empty($_SERVER['HTTPS']) && $_SERVER['HTTPS'] !== 'off' || $_SERVER['SERVER_PORT'] == 443 ? "https://" : "http://";
        $url = "{$protocol}{$_SERVER['HTTP_HOST']}{$_SERVER['REQUEST_URI']}";
        $timestamp = time();
        $nonceStr = $this->createNonceStr();
        // 这里参数的顺序要按照 key 值 ASCII 码升序排序
        $string = "jsapi_ticket={$jsapiTicket}&noncestr={$nonceStr}&timestamp={$timestamp}&url={$url}";
        $signature = sha1($string);
        $signPackage = array("appId" => $this->appId, "nonceStr" => $nonceStr, "timestamp" => $timestamp, "url" => $url, "signature" => $signature, "rawString" => $string);
        return $signPackage;
    }
 
    private function createNonceStr($length = 16)
    {
        $chars = "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789";
        $str = "";
        for ($i = 0; $i < $length; $i++) {
            $str .= substr($chars, mt_rand(0, strlen($chars) - 1), 1);
        }
        return $str;
    }
 
    private function getJsApiTicket()
    {
        // jsapi_ticket 应该全局存储与更新，以下代码以写入到文件中做示例
        $data = json_decode(file_get_contents("wxjs/jsapi_ticket.json"));
        if ($data->expire_time < time()) {
            $accessToken = $this->getAccessToken();
            // 如果是企业号用以下 URL 获取 ticket
            // $url = "https://qyapi.weixin.qq.com/cgi-bin/get_jsapi_ticket?access_token=$accessToken";
            $url = "https://api.weixin.qq.com/cgi-bin/ticket/getticket?type=jsapi&access_token={$accessToken}";
            $res = json_decode($this->httpGet($url));
            $ticket = $res->ticket;
            if ($ticket) {
                $data->expire_time = time() + 7000;
                $data->jsapi_ticket = $ticket;
                $fp = fopen("wxjs/jsapi_ticket.json", "w");
                fwrite($fp, json_encode($data));
                fclose($fp);
            }
        } else {
            $ticket = $data->jsapi_ticket;
        }
        return $ticket;
    }
 
    private function getAccessToken()
    {
        // access_token 应该全局存储与更新，以下代码以写入到文件中做示例
        $data = json_decode(file_get_contents("wxjs/access_token.json"));
        if ($data->expire_time < time()) {
            // 如果是企业号用以下URL获取access_token
            // $url = "https://qyapi.weixin.qq.com/cgi-bin/gettoken?corpid=$this->appId&corpsecret=$this->appSecret";
            $url = "https://api.weixin.qq.com/cgi-bin/token?grant_type=client_credential&appid={$this->appId}&secret={$this->appSecret}";
            $res = json_decode($this->httpGet($url));
            $access_token = $res->access_token;
            if ($access_token) {
                $data->expire_time = time() + 7000;
                $data->access_token = $access_token;
                $fp = fopen("wxjs/access_token.json", "w");
                fwrite($fp, json_encode($data));
                fclose($fp);
            }
        } else {
            $access_token = $data->access_token;
        }
        return $access_token;
    }
 
    private function httpGet($url)
    {
        $curl = curl_init();
        curl_setopt($curl, CURLOPT_RETURNTRANSFER, true);
        curl_setopt($curl, CURLOPT_TIMEOUT, 500);
        curl_setopt($curl, CURLOPT_SSL_VERIFYPEER, false);
        curl_setopt($curl, CURLOPT_SSL_VERIFYHOST, false);
        curl_setopt($curl, CURLOPT_URL, $url);
        $res = curl_exec($curl);
        curl_close($curl);
        return $res;
    }
}

$appid       = '这是appid';
$appsecret   = '这是appsecret';
$jssdk       = new JSSDK( $appid, $appsecret );
$signPackage = $jssdk->GetSignPackage();
 
wx.config({
    debug: true, // 开启调试模式,调用的所有api的返回值会在客户端alert出来，若要查看传入的参数，可以在pc端打开，参数信息会通过log打出，仅在pc端时才会打印。
    appId:     '<?php echo $signPackage["appId"];?>',     // 必填，公众号的唯一标识
    timestamp: '<?php echo $signPackage["timestamp"];?>', // 必填，生成签名的时间戳
    nonceStr:  '<?php echo $signPackage["nonceStr"];?>',  // 必填，生成签名的随机串
    signature: '<?php echo $signPackage["signature"];?>', // 必填，签名，见附录1
    jsApiList: ['scanQRCode'] // 必填，需要使用的JS接口列表，所有JS接口列表见附录2
});
```
###[天才绝招！21张GIF动图让你轻松了解各种数学概念！](http://www.hoehub.com/choice/62.html)
###[javaScript 简单实现根据身份证号码识别性别年龄生日](http://www.hoehub.com/JavaScript/43.html)
```js
   function discriCard(){ 
        //获取输入身份证号码 
        var UUserCard = "XXXXXXXXXXXXXXXXXX"; 
        //获取出生日期 
        UUserCard.substring(6, 10) + "-" + UUserCard.substring(10, 12) + "-" + UUserCard.substring(12, 14); 
        //获取性别 
        if (parseInt(UUserCard.substr(16, 1)) % 2 == 1) { 
            alert("男"); 
        } else { 
            alert("女"); 
        } 
        //获取年龄 
        var myDate = new Date(); 
        var month = myDate.getMonth() + 1; 
        var day = myDate.getDate(); 
        var age = myDate.getFullYear() - UUserCard.substring(6, 10) - 1; 
        if (UUserCard.substring(10, 12) < month || UUserCard.substring(10, 12) == month && UUserCard.substring(12, 14) <= day) { 
            age++; 
        } 
        alert(age); 
        //年龄 age 
    } 
```
###[CentOS安装crontab及使用方法](http://www.hoehub.com/Linux/40.html)
```js
第1列表示分钟1～59 每分钟用或者 /1表示
第2列表示小时1～23（0表示0点）
第3列表示日期1～31
第4列表示月份1～12
第5列标识号星期0～6（0表示星期天）
第6列要运行的命令
30 21 * * * /usr/local/etc/rc.d/lighttpd restart
// 上面的例子表示每晚的21:30重启apache。
 
45 4 1,10,22 * * /usr/local/etc/rc.d/lighttpd restart
// 上面的例子表示每月1、10、22日的4 : 45重启apache。
 
10 1 * * 6,0 /usr/local/etc/rc.d/lighttpd restart
//上面的例子表示每周六、周日的1 : 10重启apache。
 
0,30 18-23 * * * /usr/local/etc/rc.d/lighttpd restart
// 上面的例子表示在每天18 : 00至23 : 00之间每隔30分钟重启apache。
 
0 23 * * 6 /usr/local/etc/rc.d/lighttpd restart
// 上面的例子表示每星期六的11 : 00 pm重启apache。
 
* */1 * * * /usr/local/etc/rc.d/lighttpd restart
// 每一小时重启apache
 
* 23-7/1 * * * /usr/local/etc/rc.d/lighttpd restart
// 晚上11点到早上7点之间，每隔一小时重启apache
 
0 11 4 * mon-wed /usr/local/etc/rc.d/lighttpd restart
// 每月的4号与每周一到周三的11点重启apache
 
0 4 1 jan * /usr/local/etc/rc.d/lighttpd restart
// 一月一号的4点重启apache
 
*/30 * * * * /usr/sbin/ntpdate 210.72.145.44
// 每半小时同步一下时间
```
###[十张GIFs让你弄懂递归等概念](http://www.hoehub.com/PHP/recursion.html)



###[PHP修改PNG图片DPI](https://my.oschina.net/xiaoshuo/blog/209349?utm_source=tool.lu)
```js
$filename = 'img/example.png';
// ob_start();
// $im = imagecreatefrompng($filename);
// imagepng($im);
// $file = ob_get_contents();
// ob_end_clean();
$file = file_get_contents($filename);

//数据块长度为9
$len = pack("N", 9);
//数据块类型标志为pHYs
$sign = pack("A*", "pHYs");
//X方向和Y方向的分辨率均为300DPI（1像素/英寸=39.37像素/米），单位为米（0为未知，1为米）
$data = pack("NNC", 300 * 39.37, 300 * 39.37, 0x01);
//CRC检验码由数据块符号和数据域计算得到
$checksum = pack("N", crc32($sign . $data));
$phys = $len . $sign . $data . $checksum;

$pos = strpos($file, "pHYs");
if ($pos > 0) {
	//修改pHYs数据块
	$file = substr_replace($file, $phys, $pos - 4, 21);
} else {
	//IHDR结束位置（PNG头固定长度为8，IHDR固定长度为25）
	$pos = 33;
	//将pHYs数据块插入到IHDR之后
	$file = substr_replace($file, $phys, $pos, 0);
}

header("Content-type: image/png");
header('Content-Disposition: attachment; filename="' . basename($filename) . '"');
echo $file;
```
###[php中的foreach问题（1）](http://www.cnblogs.com/driftcloudy/p/3142013.html)
```js
 foreach($arr as $k => $v) 结构隐含了如下操作，分别将数组当前的'键'和当前的'值'赋给变量k和k和v。具体展开形如：

foreach($arr as $k => $v){ 
    //在用户代码执行之前隐含了2个赋值操作
    $v = currentVal(); 
    $k = currentKey();

    //继续运行用户代码
    ……
}
根据上述理论，现在我们重新来分析下第一个foreach：

第1遍循环，由于$v是一个引用，因此$v = &$arr[0]，$v=$v*2相当于$arr[0]*2，因此$arr变成2,2,3

第2遍循环，$v = &$arr[1]，$arr变成2,4,3

第3遍循环，$v = &$arr[2]，$arr变成2,4,6
 

随后代码进入了第二个foreach：

第1遍循环，隐含操作$v=$arr[0]被触发，由于此时$v仍然是$arr[2]的引用，即相当于$arr[2]=$arr[0]，$arr变成2,4,2

第2遍循环，$v=$arr[1]，即$arr[2]=$arr[1]，$arr变成2,4,4

第3遍循环，$v=$arr[2]，即$arr[2]=$arr[2]，$arr变成2,4,4

$arr = array('a','b','c');

foreach($arr as $k => $v) {
    echo key($arr), "=>", current($arr);
}

// 打印 1=>b 1=>b 1=>b
$arr = array(1, 2, 3);
$tmp = $arr;
foreach($tmp as $k => &$v){
    $v *= 2;
}
var_dump($arr, $tmp); 
创建了数组arr，随后将该数组赋给了arr，随后将该数组赋给了tmp，在接下来的foreach循环中，对v进行修改会作用于数组v进行修改会作用于数组tmp上，但是却并不作用到$arr。
从PHP5起，对象的便总是默认通过引用进行赋值，举例来说：

class A{
    public $foo = 1;
}
$a1 = $a2 = new A;
$a1->foo=100;
echo $a2->foo; // 输出100，$a1与$a2其实为同一个对象的引用

```

###[bmp图片转成jpg图片](https://blog.skyx.in/archives/123/)
```js
/**
 * BMP 创建函数
 * @author simon
 * @modified by 天心流水
 * @param string $filename path of bmp file
 * @example who use,who knows
 * @return resource of GD
 */
function imagecreatefrombmp( $filename ) {
    if ( !$f1 = fopen( $filename, "rb" ) )
        return FALSE;
 
    $FILE = unpack( "vfile_type/Vfile_size/Vreserved/Vbitmap_offset", fread( $f1, 14 ) );
    if ( $FILE['file_type'] != 19778 )
        return FALSE;
 
    $BMP = unpack( 'Vheader_size/Vwidth/Vheight/vplanes/vbits_per_pixel' . '/Vcompression/Vsize_bitmap/Vhoriz_resolution' . '/Vvert_resolution/Vcolors_used/Vcolors_important', fread( $f1, 40 ) );
    $BMP['colors'] = pow( 2, $BMP['bits_per_pixel'] );
    if ( $BMP['size_bitmap'] == 0 )
        $BMP['size_bitmap'] = $FILE['file_size'] - $FILE['bitmap_offset'];
    $BMP['bytes_per_pixel'] = $BMP['bits_per_pixel'] / 8;
    $BMP['bytes_per_pixel2'] = ceil( $BMP['bytes_per_pixel'] );
    $BMP['decal'] = ($BMP['width'] * $BMP['bytes_per_pixel'] / 4);
    $BMP['decal'] -= floor( $BMP['width'] * $BMP['bytes_per_pixel'] / 4 );
    $BMP['decal'] = 4 - (4 * $BMP['decal']);
    if ( $BMP['decal'] == 4 )
        $BMP['decal'] = 0;
 
    $PALETTE = array();
  if ($BMP['colors'] < 16777216 && $BMP['colors'] != 65536)
  {
    $PALETTE = unpack('V'.$BMP['colors'], fread($f1,$BMP['colors']*4));
  }
 
    $IMG = fread( $f1, $BMP['size_bitmap'] );
    $VIDE = chr( 0 );
 
    $res = imagecreatetruecolor( $BMP['width'], $BMP['height'] );
    $P = 0;
    $Y = $BMP['height'] - 1;
    while( $Y >= 0 ){
        $X = 0;
        while( $X < $BMP['width'] ){
            if ( $BMP['bits_per_pixel'] == 32 ){
                $COLOR = unpack( "V", substr( $IMG, $P, 3 ) );
                $B = ord(substr($IMG, $P,1));
                $G = ord(substr($IMG, $P+1,1));
                $R = ord(substr($IMG, $P+2,1));
                $color = imagecolorexact( $res, $R, $G, $B );
                if ( $color == -1 )
                    $color = imagecolorallocate( $res, $R, $G, $B );
                $COLOR[0] = $R*256*256+$G*256+$B;
                $COLOR[1] = $color;
            } elseif ( $BMP['bits_per_pixel'] == 24 ) {
                $COLOR = unpack( "V", substr( $IMG, $P, 3 ) . $VIDE );
      } elseif ( $BMP['bits_per_pixel'] == 16 ){
        $COLOR = unpack("v",substr($IMG,$P,2));
        $blue  = (($COLOR[1] & 0x001f) << 3) + 7;
        $green = (($COLOR[1] & 0x03e0) >> 2) + 7;
        $red   = (($COLOR[1] & 0xfc00) >> 7) + 7;
        $COLOR[1] = $red * 65536 + $green * 256 + $blue;
            } elseif ( $BMP['bits_per_pixel'] == 8 ){
                $COLOR = unpack( "n", $VIDE . substr( $IMG, $P, 1 ) );
                $COLOR[1] = $PALETTE[$COLOR[1] + 1];
            } elseif ( $BMP['bits_per_pixel'] == 4 ){
                $COLOR = unpack( "n", $VIDE . substr( $IMG, floor( $P ), 1 ) );
                if ( ($P * 2) % 2 == 0 )
                    $COLOR[1] = ($COLOR[1] >> 4);
                else
                    $COLOR[1] = ($COLOR[1] & 0x0F);
                $COLOR[1] = $PALETTE[$COLOR[1] + 1];
            } elseif ( $BMP['bits_per_pixel'] == 1 ){
                $COLOR = unpack( "n", $VIDE . substr( $IMG, floor( $P ), 1 ) );
                if ( ($P * 8) % 8 == 0 )
                    $COLOR[1] = $COLOR[1] >> 7;
                elseif ( ($P * 8) % 8 == 1 )
                    $COLOR[1] = ($COLOR[1] & 0x40) >> 6;
                elseif ( ($P * 8) % 8 == 2 )
                    $COLOR[1] = ($COLOR[1] & 0x20) >> 5;
                elseif ( ($P * 8) % 8 == 3 )
                    $COLOR[1] = ($COLOR[1] & 0x10) >> 4;
                elseif ( ($P * 8) % 8 == 4 )
                    $COLOR[1] = ($COLOR[1] & 0x8) >> 3;
                elseif ( ($P * 8) % 8 == 5 )
                    $COLOR[1] = ($COLOR[1] & 0x4) >> 2;
                elseif ( ($P * 8) % 8 == 6 )
                    $COLOR[1] = ($COLOR[1] & 0x2) >> 1;
                elseif ( ($P * 8) % 8 == 7 )
                    $COLOR[1] = ($COLOR[1] & 0x1);
                $COLOR[1] = $PALETTE[$COLOR[1] + 1];
            }else
                return FALSE;
            imagesetpixel( $res, $X, $Y, $COLOR[1] );
            $X++;
            $P += $BMP['bytes_per_pixel'];
        }
        $Y--;
        $P += $BMP['decal'];
    }
    fclose( $f1 );
 
    return $res;
}
```



###[遭遇php的in_array低性能08/28/2013](http://www.zendstudio.net/archives/php-in_array-s-low-performance/?utm_source=tool.lu)
```js
$y="1800";
$x = array();
for($j=0;$j&lt;2000;$j++){
        $x[]= "{$j}";
}
 
for($i=0;$i&lt;3000;$i++){
        if(in_array($y,$x)){
                continue;
        }
}
strace -ttt -o xxx /usr/local/php/bin/php test.php
ltrace -c /usr/local/php/bin/php test.php

in_array这种松比较，会将两个字符型数字串先转换为长整型再进行比较，却不知性能就耗在这上面了。
最简单的就是为in_array加第三个参数为true，即变为严格比较，同时还要比较类型，这样避免了PHP自作聪明的转换类型
面对大数组查询的时候，在PHP中应该尽量采用key查询而不是value查询。函数in_array的性能是不好的。
所以文中的例子代码如果改为下面的，应该会快很多：
<?php
$y="1800";
$x = array();
for($j=0;$j<2000;$j++){
$x[{$j}]= true;
}
for($i=0;$i
总之，大数组查询，用in_array函数是个糟糕的选择。应该尽量用isset函数或array_key_exists函数来替代 。in_array函数的复杂度是O(n)，而isset函数的复杂度是O(1)。 大数据都不要用in_array，要么先array_flip,isset($arr[$key]),要么strpos

$elemCount = 1000;
$repeatCount = 1000000;
 
$vArr = range(1, $elemCount);
$kArr = array_flip($vArr);
 
$start = microtime(true);
for ($i = 0; $i < $repeatCount; $i++) {
    in_array($i, $vArr);
}
$inArrTime = microtime(true) - $start;
echo "in_array:{$inArrTime}<br>";
 
$start = microtime(true);
for ($i = 0; $i < $repeatCount; $i++) {
    array_key_exists($i, $kArr);
}
$keyTime = microtime(true) - $start;
echo "array_key_exists:{$keyTime}<br>";
 
$start = microtime(true);
for ($i = 0; $i < $repeatCount; $i++) {
    isset($kArr[$i]);
}
$issetTime = microtime(true) - $start;
echo "isset:{$issetTime}<br>";

in_array:1.6679329872131
array_key_exists:0.23828101158142
isset:0.022069931030273
```
###[第三方微信登录和支付开发记录](https://blog.skyx.in/archives/348/)

微信的网页应用、移动应用、公众号的上限都是10个，所有同一个账号下的应用获取到的 union_id 是相同的，open_id 不同，所以需注意应用数量是否会超过上限。
微信登录目前只有APP登录、扫码登录和公众号登录三种登录方式，在微信浏览器内打开网页使用的是公众号登录的方式，其他浏览器只能使用扫码登录，换句话说目前移动端非微信浏览器打开的网页基本无法使用微信登录。 通过微信自带浏览器打开网页时唤起微信授权登录页面进行授权登录。



https://open.weixin.qq.com/connect/oauth2/authorize?appid=wxa77178c0a05b6498&redirect_uri=http%3A%2F%2Fh5.2144.com%2Fsite%2Fauth%3Fauthclient%3Dweixin-mp&scope=snsapi_userinfo&response_type=code

https://open.weixin.qq.com/connect/oauth2/authorize?appid=wxa77178c0a05b6498&response_type=code&redirect_uri=http%3A%2F%2Fh5.2144.com%2Fsite%2Fauth%3Fauthclient%3Dweixin-mp&scope=snsapi_userinfo

上面两个链接除了参数的顺序不同之外完全相同，但是上面那个链接可以正常显示授权页面，下面那个则不可以

###[PHP修改apk文件的comment实现](https://blog.skyx.in/archives/319/)
```js
$comment = '123测试';
 
$file = fopen('R:\1.apk', 'r+');
fseek($file, -2, SEEK_END);
fwrite($file, pack('s', mb_strlen($comment, '8bit')));
fwrite($file, $comment);
fclose($file);
 
$zip = new ZipArchive();
 
$zip->open('R:\1.apk');
var_dump($zip->getArchiveComment());
//$zip->setArchiveComment($comment);
//var_dump($zip->getArchiveComment());
$zip->close();
```
###[使用phpbrew管理php版本](https://blog.skyx.in/archives/322/)
curl -L -O https://github.com/phpbrew/phpbrew/raw/master/phpbrew
chmod +x phpbrew
sudo mv phpbrew /usr/bin/phpbrew
###[开源一个简单轻量的高性能PHP路由实现](https://blog.skyx.in/archives/303/)
```js
Github:https://github.com/takashiki/cdo
$do = new \Mis\Cdo();
 
$do->get('/', function () {
    echo 'hello world';
});
 
$do->post('/', function () {
    $name = isset($_POST['name']) ? $_POST['name'] : 'world';
    echo "hello {$name}";
});
 
$do->any('/(\d+)', function ($id) {
    echo $id;
});
 
/**
 * When using named subpattern, order of parameters is not matter.
 * eg. /book/2
 */
$do->any('/(?P<type>\w+)/(?P<page>\d+)', function ($page, $type) {
    echo $type.'<br>'.$page;
});
 
$do->run();
```
###[使用db2md生成表结构](https://blog.skyx.in/archives/275/)
https://github.com/index0h/node-db2md
```js
这个简直是神器，写数据库文档再也不用头疼了。

首先要安装node和npm，这就不多说了。然后使用npm安装db2md：
npm install db2md -g
安装完成后创建配置文件db2md.json，示例如下：

{
    "user": "root",
    "pass": "123456",
    "database": "test",
    "tables": ".*"
}
配置完成后即可以开始导出：


db2md -o tables.md
```
###[极简图床、sm.ms图床curl上传图片](https://blog.skyx.in/archives/164/)
```js
$data = base64_encode(file_get_contents('test.jpg'));
 
$ch = curl_init('http://yotuku.cn/upload?name=');                                                                      
curl_setopt($ch, CURLOPT_CUSTOMREQUEST, "POST");                                                                     
curl_setopt($ch, CURLOPT_POSTFIELDS, $data);                                                                  
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);                                                                      
curl_setopt($ch, CURLOPT_HTTPHEADER, array(                                                                          
    'Content-Type: text/plain',                                                                                
    'Content-Length: ' . strlen($data))                                                                       
);                                                                                                                   
 
$result = curl_exec($ch);
echo $result;
$url = 'https://sm.ms/index.php?act=upload';
$image = curl_file_create(realpath('test.jpg'), 'image/jpg', 'test.jpg');
 
$ch = curl_init();
curl_setopt($ch, CURLOPT_URL, $url);
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
curl_setopt($ch, CURLOPT_SSL_VERIFYPEER, false);
curl_setopt($ch, CURLOPT_POST, true);
curl_setopt($ch, CURLOPT_POSTFIELDS, ['smfile' => $image]);
$data = curl_exec($ch);
curl_close($ch);
echo $data;
 
```
###[linux服务器上mysql无法远程连接的可能原因](https://blog.skyx.in/archives/152/)
```js
grant all privileges on *.* to root@"%" identified by "password" with grant option;
 
flush privileges;
第一个代表数据库名；第二个代表表名。root：授予用户账号。“%”：表示授权的用户IP，%代表任意的IP地址都能访问。“password”：分配账号对应的密码。第二行命令是刷新权限信息
如果telnet不通，我们先用netstat查看3306端口是否已监听所有ip地址的请求：


netstat -an | grep 3306
如果输出为


tcp        0      0 127.0.0.1:3306            0.0.0.0:*               LISTEN
则说明只监听了本地连接。解决方法：修改/etc/mysql/my.cnf文件。打开文件，找到下面内容：


# Instead of skip-networking the default is now to listen only on
# localhost which is more compatible and is not less secure.
bind-address  = 127.0.0.1
把上面bind-address = 127.0.0.1这一行注释掉或者把127.0.0.1换成合适的IP。
重新启动mysql再用netstat检测是否为：


tcp        0      0 0.0.0.0:3306            0.0.0.0:*               LISTEN
如果这样之后还是telnet不通，那基本就是防火墙的问题了，查看iptables的rules文件里是否包含


-A INPUT -p tcp -m tcp --dport 3306 -j ACCEPT
如果没有该规则的话加入该规则后重启iptables就可以了。
```

###[PHP解析\x22之类的十六进制字符串代码](https://blog.skyx.in/archives/192/)
function parse_hex($string) {
    return preg_replace_callback(
        "(\\\\x([0-9a-f]{2}))i",
        function($a) {return chr(hexdec($a[1]));},
        $string
    );
}

###[开源一个简单的短网址程序Ourls](https://blog.skyx.in/archives/183/)
在线演示地址：http://skyx.in/。

github地址：https://github.com/takashiki/Ourls
###[php读写crx文件](https://blog.skyx.in/archives/264/)
###[PHP Warning: Module 'modulename' already loaded in Unknown on line 0 产生原因及解决方法](https://blog.skyx.in/archives/288/)
该扩展在编译 PHP 时已经 enable 了，但是在 php.ini 中又写了动态调用该扩展的 so 文件。

这时候我们可以查看一下 phpinfo ：
 
php -i | grep 'modulename'
php -i | grep 'php.ini'
然后去对应的 php.ini 文件中去掉该扩展即可
###[PHPStorm中使用php-cs-fixer进行自动代码格式化](https://blog.skyx.in/archives/207/)
composer global require fabpot/php-cs-fixer
jsonp接口xss防范 
只要在header里输出类型设置为javascript即可：

header('Content-type: text/javascript;charset=utf-8');
git config --global core.autocrlf false Windows下git commit时设置不自动将LF转换为CRLF

*nix系统的换行符为LF(\n)，而Windows的换行符为CRLF(\r\n)，所以在Windows上的默认配置的Git会在git pull时将LF换行符换为CRLF，而git push时会再将换行符换回去。然而，当文件中含有中文时Git的这个功能会出现问题，pull时能正常转换，push时却无法正常执行，这时就会出现文件比对时整个文件内容都改变了，但肉眼却无法看出。

解决方法很简单，直接执行以下命令进行全局配置就可以了：

 
git config --global core.autocrlf false
###[Fork过来的项目拉取源项目的更新](https://blog.skyx.in/archives/276/)
git remote add upstream https://github.com/lj2007331/lnmp.git
在每次 Pull Request 前做如下操作，即可实现和上游版本库的同步。

 
git remote update upstream
git rebase upstream/{branch name}
注意的是在rebase操作之前，一定要将checkout到{branch name}所指定的branch
git Push代码
###[PHP源码阅读，PHP设计模式-胖胖的空间](http://www.phppan.com/2014/02/php-var-compare/?utm_source=tool.lu)
###[git发布脚本](http://stackoverflow.com/questions/279169/deploy-a-project-using-git-push?utm_source=tool.lu)
###[An object oriented PHP driver for FFMpeg binary](https://github.com/PHP-FFMpeg/PHP-FFMpeg?utm_source=tool.lu)
$ composer require php-ffmpeg/php-ffmpeg
```js
$ffmpeg = FFMpeg\FFMpeg::create();
$video = $ffmpeg->open('video.mpg');
$video
    ->filters()
    ->resize(new FFMpeg\Coordinate\Dimension(320, 240))
    ->synchronize();
$video
    ->frame(FFMpeg\Coordinate\TimeCode::fromSeconds(10))
    ->save('frame.jpg');
$video
    ->save(new FFMpeg\Format\Video\X264(), 'export-x264.mp4')
    ->save(new FFMpeg\Format\Video\WMV(), 'export-wmv.wmv')
    ->save(new FFMpeg\Format\Video\WebM(), 'export-webm.webm');
```
###[lxml 解析 html 以爬取 豆瓣电影主页本周口碑榜](http://blog.csdn.net/tanzuozhev/article/details/50442243)
```js
import lxml.html
str_url = 'http://movie.douban.com/'
request = urllib.request.Request(str_url)
html_text = urllib.request.urlopen(request).read()
root = lxml.html.fromstring(html_text)
# 获取本页面所有项目名称
movies_list = [a.text for a in  root.cssselect('div.billboard-bd tr td a')]
print(movies_list)
# 获取所有电影超链接
movies_href = [a.get('href') for a in  root.cssselect('div.billboard-bd tr td a')]
print(movies_href)
```
###[51job和智联招聘的自动刷新简历脚本](http://www.woowen.com/php/2014/09/20/51job%E5%92%8C%E6%99%BA%E8%81%94%E6%8B%9B%E8%81%98%E8%87%AA%E5%8A%A8%E5%88%B7%E6%96%B0%E7%AE%80%E5%8E%86%E8%84%9A%E6%9C%AC/)
```js
dl("php_curl.dll");

$ifupdate = false;

$rand = mt_rand(1, 5);

if($rand == 5)
$ifupdate = true;

if($ifupdate)
{
//智联招聘
$ch = curl_init();

curl_setopt($ch, CURLOPT_URL, "...填写刷新简历ajax链接...");

curl_setopt($ch,CURLOPT_COOKIE,"JSpUserInfo=...填写cookie参数...");

$result = curl_exec($ch);

curl_close($ch);

//前程无忧
$ch = curl_init();

curl_setopt($ch, CURLOPT_URL, "...填写刷新简历ajax链接...");

curl_setopt($ch,CURLOPT_COOKIE,"51job=...填写cookie参数...");

$result = curl_exec($ch);

curl_close($ch);

}else
{
echo 'rand not hit';
}

在CURLOPT_URL参数中写上你刷新简历时访问的ajax url 的地址.
在CURLOPT_COOKIE中写上你浏览器cookie中对应参数的value.
然后在linux中使用将脚本1小时一次的执行起来就可以了.
```

###[PHP内核](http://www.imsiren.com/archives/tag/php%e5%86%85%e6%a0%b8)
###[Nginx PHP 使用 limit_req,limit_conn 限制并发，外加白名单](http://www.imsiren.com/archives/1230)
nginx/conf/limit/whiteip.conf 里面是你要忽略限制的白名单IP地址
###[跨浏览器下PHP下载文件名中的中文乱码问题](http://www.cnblogs.com/jiji262/archive/2012/09/21/2697205.html?utm_source=tool.lu)
```js
$ua = $_SERVER["HTTP_USER_AGENT"];

$filename = "中文 文件名.txt";
$encoded_filename = urlencode($filename);
$encoded_filename = str_replace("+", "%20", $encoded_filename);

header('Content-Type: application/octet-stream');

if (preg_match("/MSIE/", $ua)) {
    header('Content-Disposition: attachment; filename="' . $encoded_filename . '"');
} else if (preg_match("/Firefox/", $ua)) {
    header('Content-Disposition: attachment; filename*="utf8\'\'' . $filename . '"');
} else {
    header('Content-Disposition: attachment; filename="' . $filename . '"');
}

print 'ABC';
```
###[JavaScript获取服务器时间](http://amals.org/?id=216)
```js
var xhr = null;
if(window.XMLHttpRequest){
   xhr = new window.XMLHttpRequest();
}else{
 // ie
   xhr = new ActiveObject("Microsoft")
}
// 通过get的方式请求当前文件
xhr.open("get","/");
xhr.send(null);
// 监听请求状态变化
xhr.onreadystatechange = function(){
   var time = null,curDate = null;
   if(xhr.readyState===2){
       // 获取响应头里的时间戳
       time = xhr.getResponseHeader("Date");
       console.log(xhr.getAllResponseHeaders())
       curDate = new Date(time);
       console.log("服务器时间："+curDate.getFullYear()+"-"+(curDate.getMonth()+1)+"-"+curDate.getDate()+" "+curDate.getHours()+":"+curDate.getMinutes()+":"+curDate.getSeconds());
   }
}
```
###[Laravel会成为最成功的PHP框架](http://www.hoehub.com/PHP/141.html?utm_source=tool.lu)
模块化和可扩展性 中间件 各种集成 事件处理 对象关系化映射（ORM）


###[PHP 实现 短URL](http://www.imsiren.com/archives/459)
```js
function base62($x){
    $show='';
    while($x>0){
        $s=$x%62;
        if($s>35){
            $s=chr($s+61);
        }elseif($s>9&&$s<=35){
            $s=chr($s+55);
        }
        $show.=$s;
        $x=floor($x/62);
    }
            return $show;
}
function urlShort($url){
    $url=crc32($url);
    $res=sprintf('%u',$url);
    return base62($res);
}
echo urlShort("http://www.imsiren.com");
```
###[redis 应用场景](http://www.imsiren.com/archives/982)
```js
将redis的List用作队列，这个很轻量级，不用引入别的队列服务器，缺点是可能会丢失数据，因为其持久化方案是redis通用的aof或者rdb方式

2.将排好序的实体id放到LIST中，然后以prefix 实体id为key，用hashtable存储具体的实体信息

3.用ZSET存储排名和带有权重信息的一些实体id，缺点是内存占用太厉害了。

4.用hashtable做一些映射，例如username=>user_id等

5.set可以支持一些逻辑操作，但是排序的时间复杂度不佳，所以我选择了用list

6.set用来做唯一性验证，如果验证某个用户是否已经对某篇文章进行了赞的操作

7.使用redis用来优化内存hash-max-zipmap-entries等参数减少内存使用量

8.排序好的id也可以用string的getRange和setRange命令来实现顺序访问

用LIST不好的是其顺序已经确定，其删除操作耗时O(n)，顺序查找并删除，而且不支持union inter等操作，这些操作可以模拟and 和or这两种关系操作。
```
###[php strtotime是否有bug](https://segmentfault.com/q/1010000002454116?utm_source=tool.lu)
```js
$beginMon=strtotime("-1 week Monday");
$endMon=strtotime("-1 week Tuesday")-1;
echo date("Y-m-d H:i:s", $beginMon);
echo('<br/>');
echo date("Y-m-d H:i:s", $endMon);
echo("<br />");

//上面是获取本周一的开始与结束时间戳
//结果如下：
//2015-01-05 00:00:00（错误）
//2014-12-29 23:59:59（正确）

$beginSun=strtotime("+0 week Sunday");
$endSun=strtotime("+1 week Monday")-1;
echo date("Y-m-d H:i:s", $beginSun)."<br />";
echo date("Y-m-d H:i:s",$endSun)."<br />";

//上面获取的是本周末的开始与结束时间戳
//结果如下：
//2015-01-04 00:00:00
//2015-01-11 23:59:59(错误)

//以上案例都是在今天测试(非周末 因为周末)
http://php.net/manual/en/datetime.formats.relative.php 
每星期的七天，'sunday' | 'monday' | 'tuesday' | 'wednesday' | 'thursday' | 'friday' | 'saturday' | 'sun' | 'mon' | 'tue' | 'wed' | 'thu' | 'fri' | 'sat' | 'sun' 应该是以当前时间往后推 7 * 24 小时来考量的。本例中，如果你想特指本周的某个星期一，你可以使用 strtotime("-1 week Monday this week"); ，其它的天，类似地，指定是在哪个自然周内（本例即 this week 指定为本自然周），不要让系统自己按自己默认的逻辑处理即可。
```

###[表分区](http://www.jianshu.com/p/52843a98acda)
```js
ALTER TABLE users MODIFY COLUMN `id` bigint(20) unsigned NOT NULL;
ALTER TABLE users DROP PRIMARY KEY;
ALTER TABLE users ADD INDEX `id`(`id`);
ALTER TABLE users PARTITION BY HASH(id) PARTITIONS 64;
分区表限制：
单表最多支持1024个分区
MySQL5.1只能对数据表的整型列进行分区，或者数据列可以通过分区函数转化成整型列;
MySQL5.5的RANGE LIST类型可以直接使用列进行分区
如果分区字段中有主键或唯一索引的列，那么所有的主键列和唯一索引列都必须包含进来
分区表无法使用外键约束
分区必须使用相同的Engine
对于MyISAM分区表，不能在使用LOAD INDEX INTO CACHE操作
对于MyISAM分区表，使用时会打开更多的文件描述符（单个分区是一个独立的文件）
t1#p#p0.ibd t1#p#p1.ibd
mysql> show table status from vhall like 'webinar_user_regs'\G
*************************** 1. row ***************************
           Name: webinar_user_regs
         Engine: InnoDB
        Version: 10
     Row_format: Compact
           Rows: 728334
 Avg_row_length: 174
    Data_length: 126992384
Max_data_length: 0
   Index_length: 210354176
      Data_free: 231735296
 Auto_increment: NULL
    Create_time: 2017-03-13 12:27:36
    Update_time: NULL
     Check_time: NULL
      Collation: utf8_unicode_ci
       Checksum: NULL
 Create_options: partitioned
        Comment:
1 row in set (0.02 sec)
无论创建何种类型的分区，如果表中存在主键或唯一索引时，分区列必须是唯一索引的一个组成部分
在MySQL5.6中，可以使用清空一个分区数据：alter table operation_log truncate partition2014-01;
清空该分区表所有分区数据：alter table operation_log truncate partition all;

http://blog.csdn.net/tjcyjd/article/details/11194489 

ALTER TABLE users ADD PARTITION PARTITIONS 8;  
            将分区总数扩展到8个。
	    
CREATE TABLE ti2 (id INT, amount DECIMAL(7,2), tr_date DATE)  
    ENGINE=myisam  
    PARTITION BY HASH( MONTH(tr_date) )  
    PARTITIONS 6;    
默认分区限制分区字段必须是主键（PRIMARY KEY)的一部分，为了去除此
限制：
[方法1] 使用ID 
[方法2] 将原有PK去掉生成新PK
alter table results drop PRIMARY KEY;   
```
###[phptrace 是一个追踪（trace）PHP执行流程的工具](http://chuansong.me/n/1031743)
Github：https://github.com/Qihoo360/phptrace
```js
$ ./phptrace -p 3130 -s 3130 为php-fpm的进程ID
phptrace 0.1 demo, published by infra webcore team
process id = 3130
script_filename = /home/renyongquan/opt/nginx//webapp/block.php
[0x7f27b9a99dc8]  sleep /home/renyongquan/opt/nginx/webapp/block.php:6
[0x7f27b9a99d08]  say /home/renyongquan/opt/nginx/webapp/block.php:3
[0x7f27b9a99c50]  run /home/renyongquan/opt/nginx/webapp/block.php:10
-p 参数指定进程pid， -s表示我们需要获取stack信息； -p参数是必需的，并且只能是PHP相关进程（php,php-cli,php-fpm）的pid， 这很好理解，因为我们获取的是PHP的调用信息。-s 如果省略，则phptrace不会打印调用栈，而是实时获取PHP函数执行流程，即phptrace 的第二个功能，也是其主要功能。现在我们仍然回到stack上来。
基础架构快报 
程序输出的第一行，是版本信息，第二行显示了其进程PID，第三行是当前执行的PHP脚本，从第四行开始就是调用栈信息，从打印的 信息可以看出，最外层run函数调用了say函数，最终调用了sleep函数。
```
###[温故而知新之PHP手册](http://www.woowen.com/php/2015/01/21/%E6%B8%A9%E6%95%85%E8%80%8C%E7%9F%A5%E6%96%B0%E4%B9%8BPHP%E6%89%8B%E5%86%8C(1)/?utm_source=tool.lu)
```js
不要使用AND 和 OR 尽量使用 && 和 || 来替代,因为 && 和 || 的优先级比AND 和 OR要高,连 = 的优先级都比AND 和 OR要高.

 //这个还是要记录下,虽然以前就知道.
    $c = $a or $b 跟 ($c = $a) or $b 同义.
    $c = $a || $b 跟 $c = ($a or $b) 同义.
    不要对each() 中需要遍历的数组赋值 例如
    $a = array('a','v','c');
    while(list($b,$c) = each($a)){          
        $e = $a;
        echo $b;
        echo $c;
    }
    //数组在赋值的时候会重置指针,上面代码会无限循环
传递参数使用sub.x 的时候,PHP会自动转成subx因此应该使用$GET['sub_x']来获取值    
   const BIT_5 = 1 << 5;  //PHP5.6之后可以这么做
    define('BIT_5', 1 << 5);//一直可以
    //define还可以这么写
    for ($i = 0; $i < 32; ++$i) {
        define('BIT_' . $i, 1 << $i);
    }
    //const不能这么定义
    if(1){
        const XX = 'xx';
    }
    //define可以
    if(1){
        define('XX','xx');
    }  
var_dump("10" == "1e1"); // 10 == 10 -> true
    var_dump(100 == "1e2"); // 100 == 100 -> true   
    数组比较,数组中的单元如果具有相同的键名和值则比较时相等
    $a = array("apple", "banana");
    $b = array(1 => "banana", "0" => "apple");

    var_dump($a == $b); // bool(true)
    var_dump($a === $b); // bool(false)
    $a = 1;
$b = 1.25;
debug_zval_dump($a);
debug_zval_dump($b);
$array = [0];
foreach ($array as &$val) {
    var_dump($val);
    $array[1] = 1;
}
PHP7之前，当数组通过 foreach 迭代时，数组指针会移动。现在开始，不再如此
$array = [0, 1, 2];
foreach ($array as &$val) {
    var_dump(current($array)); 

    // PHP7
    // return int(0)

    // PHP5
    // return int(1),int(2),bool(false)
}

$a = '9d9';
for ($i = 0; $i < 10; $i++) {
    $a++;
}
var_dump($a);

//echo 18
当运行到9e0的时候 ,科学计数法转换成了9,那么后面++就成了数字了..

$a = floor((0.1+0.7) * 10);
//返回的结果并不是8,而是7
$a = round((0.1+0.7) * 10);
//返回的结果 = 8
$a = 9 - 5.1;
$b = 3.9;
var_dump(round($a, 2) == round($b, 2));

如果需要判断是否为空,可以使用Isset这种语句,而不要使用Isnull函数.或者使用===Null方法.速度跟Isset差不多,含义跟Isnull一致
键值为数字时  使用 + : 会返回前一个数组的value
使用 array_merge : 返回合并之后的一个数组,且重置键值
键值为字符时 使用 + : 返回数组1的value
使用 array_merge : 返回数组2的value
键值重置 使用 + : 键值不会重置
使用 array_merge : 数字键值会从0开始重置,且返回的合并数组的键值排序是按照先数组1,再数组2的次序来的

当Errorreporting 设置 Notice之上的时候,Arraymerge函数第一个参数为Null也不会报错,但是确实不会返回任何数据了.

个人开发本地/测试环境还是把Errorreporting设置成EALL吧.
__FUNCTION__和METHOD的不同在于前者只会返回函数名称,后者会连类名一起返回
PHP7中在switch语句中不能出现多个default,而php5可以,并且会执行最后一个 新增方法随机数字random_int
ASP的元素标签被移除,不能再使用<% <%=以及<script language="php”> .新增方法随机字节random_bytes
新增intdiv方法,取参数1 除以参数2 然后取整
http://www.woowen.com/php/2015/12/07/PHP7%20%E6%96%B0%E7%89%B9%E6%80%A7,%E6%94%B9%E5%8F%98%E5%8F%98%E5%8C%96/ 

SELECT * FROM `module_images`  WHERE pid = 'xx' and appid = 'xx' and parent in (416,415,419,421,414) GROUP BY parent order by FIELD(parent,416,415,419,421,414)
前面的In里面的顺序可以随便改变,但是后面的需要按照顺序书写. 关键就是这个Order By FIELD.另外不要忘记Group By 不然是会出错的.

arrayshift 在使用arrayshift弹出一个非常大的数组的第一个元素的时候,执行效率会很低.
Array_Pop()的复杂度为O(1)
Array_Shift()的复杂度为O(N)
当你执行一个非常大的数组的时候会随着数组庞大而降低效率.因此当你给一个非常大的数组执行弹出首元素操作的时候可以使用,Arrayreverse() 和 Arraypop()结合的方式.
较频繁地作为查询条件的字段
唯一性太差的字段不适合建立索引
更新太频繁地字段不适合创建索引
不会出现在where条件中的字段不该建立索引


新增两个CSPRNG函数：random_int和random_bytes /dev/urandom 会被作为最后一个可使用的工具
session_start([
    'cache_limiter' => 'private',
    'read_and_close' => true,
]);
覆盖写在 php.ini 中的 session 配置项。
有了preg_replace_callback_array就可以让每个不同的表达式对应不同的回调函数。
$subject = 'Aaaaaa Bbb';
 
echo preg_replace_callback_array(
    [
        '~[a]+~i' => function ($match) {
            return str_repeat('1', strlen($match[0]));
        },
        '~[b]+~i' => function ($match) {
            return str_repeat('2', strlen($match[0]));
        }
    ],
    $subject
);
使用 intdiv() 计算整数除法
生成器支持 return
$gen = (function() {
    yield 1;
    yield 2;
 
    return 3;
})();
 
foreach ($gen as $val) {
    echo $val, PHP_EOL;
}
 
echo $gen->getReturn(), PHP_EOL;
class A {private $x = 1;}
 
// Pre PHP 7 code
$getXCB = function() {return $this->x;};
$getX = $getXCB->bindTo(new A, A::class); // intermediate closure
echo $getX();
 
// PHP 7+ code
$getX = function() {return $this->x;};
echo $getX->call(new A);

// converts all objects into __PHP_Incomplete_Class object
$data = unserialize($foo, ["allowed_classes" => false]);
 
// converts all objects into __PHP_Incomplete_Class object except those of MyClass and MyClass2
$data = unserialize($foo, ["allowed_classes" => ["MyClass", "MyClass2"]);
//emoji微笑
echo "\u{1F603}";
echo [1, 2, 3][0];
echo 'string'[0];
$result = isset($var) ? $var : 'none';
 
$result = $var ?? 'none';  现在可以支持大于 2GB 的文件上传。
```
###[PHP 单例模式和工厂模式](http://www.woowen.com/php/2014/10/22/php%E5%8D%95%E4%BE%8B%E6%A8%A1%E5%BC%8F%E5%92%8C%E5%B7%A5%E5%8E%82%E6%A8%A1%E5%BC%8F/)
这个类有且只有一个实例存在,这样可以方式被多个实例化,造成多个操作.或者操作同时进行,你可以将单例用在数据库类上,这样在同一时间只会有一个数据库实例存在,也就是只会有一个数据库连接的存在,这样避免的过多的数据库连接,和不必要的系统资源的浪费.
```js
//单例模式
class myClass{
    static $__staticvar; //静态成员变量
    private $_str; //私有的变量
    private function __construct(){
        $this->_str = '单例模式';
    }
    private function __clone(){} //重载clone方法
    //由于单例不能被其他类所实例化,也就是不能使用$test = new myclass();
    public static function getObject(){
    //判断静态成员中是否有该对象如果没有就重新实例化.有就直接返回
    if(!(self::$__staticvar instanceof self))
    {
        self::$__staticvar = new self();
    }
        return self::$__staticvar;
    }
    //单例的一个方法
    public function getStr(){
        return $this->_str;
    }
}

$dl = myClass::getObject();
print_r($dl->getStr());
工厂模式,就是你定义一个接口,在接口中写下哪些方法可能会被用到.然后将每个特定的类起调用这个接口,并且重写里面的方法,在通过一个工厂类来根据判断而申明不同的类.工厂类必须返回一个对象,一般的命名规则为factory的静态方法.
//工厂模式
public static function factory($var){
    switch($var){
        case 'xx':
        $test = new classname();
        break;
        case 'xxx':
        $test = new classname1();
        break;
    }
return $test;
}

```
###[208.130.29.30-35这个IP段换成CIDR格式](http://mp.weixin.qq.com/s?__biz=MzAwMTEwNzEyOQ%3D%3D&mid=2650009231&idx=1&sn=6845ceb44d7ea6bf6464c3e5d4e0b75c&chksm=82d9fb59b5ae724f13346d784adb54b65378f94cf7f8c08000aecc93681f7e496c1c37b59697&scene=0&utm_source=tool.lu#wechat_redirect)
```js
# 确定起始和结尾IP，无论多复杂都可以转换
startip = '208.130.29.30'
endip = '208.130.29.35'
cidrs = netaddr.iprange_to_cidrs(startip, endip)
for k, v in enumerate(cidrs):
    iplist = v
    print iplist
输出：
208.130.29.30/31
208.130.29.32/30

反过来，CIDR也能直接转成IP地址段：

from netaddr import *

ip = IPNetwork('192.0.2.16/29')
ip_list = list(ip)
print(ip_list)
```
###[php缺点](https://www.toptal.com/php/10-most-common-mistakes-php-programmers-make?utm_source=tool.lu)

```js
$arr = array(1, 2, 3, 4);
foreach ($arr as &$value) {
    $value = $value * 2;
}
// $arr is now array(2, 4, 6, 8)
unset($value);

```
###[Python 技巧总结](http://litaotao.github.io/python-materials?utm_source=tool.lu)
###[字符型图片验证码识别完整过程及Python实现](http://www.cnblogs.com/beer/p/5672678.html?utm_source=tool.lu)
###[python词云 wordcloud 入门](http://blog.csdn.net/tanzuozhev/article/details/50789226?utm_source=tool.lu)
```js
官网: https://amueller.github.io/word_cloud/ 
github: https://github.com/amueller/word_cloud 
wget  https://github.com/amueller/word_cloud/archive/master.zip
unzip master.zip
rm master.zip
cd word_cloud-master
pip install -r requirements.txt
python setup.py install
from os import path
from scipy.misc import imread
import matplotlib.pyplot as plt

from wordcloud import WordCloud, STOPWORDS, ImageColorGenerator

# 获取当前文件路径
# __file__ 为当前文件, 在ide中运行此行会报错,可改为
# d = path.dirname('.')
d = path.dirname(__file__)

# 读取文本 alice.txt 在包文件的example目录下
#内容为
"""
Project Gutenberg's Alice's Adventures in Wonderland, by Lewis Carroll

This eBook is for the use of anyone anywhere at no cost and with
almost no restrictions whatsoever.  You may copy it, give it away or
re-use it under the terms of the Project Gutenberg License included
with this eBook or online at www.gutenberg.org
"""
text = open(path.join(d, 'alice.txt')).read()

# read the mask / color image
# taken from http://jirkavinse.deviantart.com/art/quot-Real-Life-quot-Alice-282261010
# 设置背景图片
alice_coloring = imread(path.join(d, "alice_color.png"))

wc = WordCloud(background_color="white", #背景颜色max_words=2000,# 词云显示的最大词数
mask=alice_coloring,#设置背景图片
stopwords=STOPWORDS.add("said"),
max_font_size=40, #字体最大值
random_state=42)
# 生成词云, 可以用generate输入全部文本(中文不好分词),也可以我们计算好词频后使用generate_from_frequencies函数
wc.generate(text)
# wc.generate_from_frequencies(txt_freq)
# txt_freq例子为[('词a', 100),('词b', 90),('词c', 80)]
# 从背景图片生成颜色值
image_colors = ImageColorGenerator(alice_coloring)

# 以下代码显示图片
plt.imshow(wc)
plt.axis("off")
# 绘制词云
plt.figure()
# recolor wordcloud and show
# we could also give color_func=image_colors directly in the constructor
plt.imshow(wc.recolor(color_func=image_colors))
plt.axis("off")
# 绘制背景图片为颜色的图片
plt.figure()
plt.imshow(alice_coloring, cmap=plt.cm.gray)
plt.axis("off")
plt.show()
# 保存图片
wc.to_file(path.join(d, "名称.png"))
```
###[python2,3对比](http://python-future.org/compatible_idioms.html?utm_source=tool.lu)
###[python脚本集合](https://github.com/realpython/python-scripts?utm_source=tool.lu)
###[Python列表对象实现原理](https://foofish.net/python-list-implements.html?utm_source=tool.lu)
###[Php Imagick常用知识](http://www.woowen.com/php/2014/08/10/PHP%20Imagick%20%E8%B5%84%E6%96%99%E6%B1%87%E6%80%BB/)
html->pdf->imgick->pic

htmltopdf 有第三方公司提供接口http://pdfcrowd.com/

###[数据库设计需要注意的地方](http://www.woowen.com/mysql/2014/10/15/%E6%95%B0%E6%8D%AE%E5%BA%93%E8%AE%BE%E8%AE%A1%20%E6%B3%A8%E6%84%8F%E8%A6%81%E7%82%B9/)


###[多维数组根据键值排序](http://www.woowen.com/php/2014/10/28/php%E5%A4%9A%E7%BB%B4%E6%95%B0%E7%BB%84%E6%8E%92%E5%BA%8F,%E5%AD%97%E7%AC%A6%E7%9B%B8%E4%BC%BC%E5%BA%A6%E5%8C%B9%E9%85%8D%E5%BA%A6/)
```js
/**
     * @param $array 需要排序的数组
     * @param $keyName 数组键值名称
     * @param int $order
     * @return array
     * @desc 多维数组根据键值排序,排序之后重置键值
     * @明心 php 获取2个字符之间的相似度 similar_text()
     */
    public static function array_sort($array, $keyName, $order=SORT_ASC)
    {
        $new_array = array();
        $sortable_array = array();
        if (count($array) > 0) {
            foreach ($array as $k => $v) {
                if (is_array($v)) {
                    foreach ($v as $k2 => $v2) {
                        if ($k2 == $keyName) {
                            $sortable_array[$k] = $v2;
                        }
                    }
                } else {
                    $sortable_array[$k] = $v;
                }
            }

            switch ($order) {
                case SORT_ASC:
                    asort($sortable_array);
                    break;
                case SORT_DESC:
                    arsort($sortable_array);
                    break;
            }
            foreach ($sortable_array as $k => $v) {
                $new_array[$k] = $array[$k];
            }
        }
        //重置混乱键值
        return array_values($new_array);
    }
```

###[PHP二维码类库(phpqrcode.php)详解及应用](http://www.hoehub.com/PHP/176.html?utm_source=tool.lu)
###[php文章索引](http://tool.lu/article/tag/php?page=2)
###[说说 PHP 的 die 和 exit](http://0x1.im/blog/php/php-exit-die.html?utm_source=tool.lu)
###[max/min 函数（PHP）的一个小 BUG](http://0x1.im/blog/php/bug-of-php-function-max.html?utm_source=tool.lu)
>>> max(0, ceil(-0.5))
=> 0
>>> $a = ceil(-0.5)
=> -0.0
>>> max($a, 0)
=> -0.0
###[php的curl调用远程接口](https://my.oschina.net/u/222608/blog/808708?utm_source=tool.lu)
改了curl一个参数CURLOPT_CONNECTTIMEOUT_MS
strace -o output.txt -f -T -tt -e trace=all php test.php
###[Differences between array_replace and array_merge in PHP](http://stackoverflow.com/questions/34367511/differences-between-array-replace-and-array-merge-in-php/34367698?utm_source=tool.lu)
```js
$a = array('a' => 'hello', 'b' => 'world');
$b = array('a' => 'person', 'b' => 'thing', 'c'=>'other', '15'=>'x');

print_r(array_merge($a, $b));
/*Array
(
    [a] => person
    [b] => thing
    [c] => other
    [0] => x
)*/

print_r(array_replace($a, $b));
/*Array
(
    [a] => person
    [b] => thing
    [c] => other
    [15] => x
)*/
$a = array('0'=>'a', '1'=>'c');
$b = array('0'=>'b');

print_r(array_merge($a, $b));
/*Array
(
  [0] => a
  [1] => c
  [2] => b
)*/

print_r(array_replace($a, $b));
/*Array
(
  [0] => b
  [1] => c
)*/
```
###[当cpu飙升时，找出php中可能有问题的代码行](http://www.bo56.com/%E5%BD%93cpu%E9%A3%99%E5%8D%87%E6%97%B6%EF%BC%8C%E6%89%BE%E5%87%BAphp%E4%B8%AD%E5%8F%AF%E8%83%BD%E6%9C%89%E9%97%AE%E9%A2%98%E7%9A%84%E4%BB%A3%E7%A0%81%E8%A1%8C/?spm=0.0.0.0.mlzm0G&utm_source=tool.lu)
###[php命令行](http://www.phpsh.org/?utm_source=tool.lu)
###[JavaScript + PHP(正则表达式)](http://amals.org/?id=1&utm_source=tool.lu)
```js
var pattern = /\w[-\w.+]*@([A-Za-z0-9][-A-Za-z0-9]+\.)+[A-Za-z]{2,14}/,
	str = '';
        console.log(pattern.test(str));
	
 $str = '';
        $isMatched = preg_match('/\w[-\w.+]*@([A-Za-z0-9][-A-Za-z0-9]+\.)+[A-Za-z]{2,14}/', $str, $matches);
        var_dump($isMatched, $matches);
匹配双字节字符(包含汉字)var pattern = /[^\x00-\xff]/,
匹配时间(时:分:秒):var pattern = /([01]?\d|2[0-3]):[0-5]?\d:[0-5]?\d/,
匹配身份证: var pattern = /\d{15}|\d{17}[0-9Xx]/,
```
###[php 实现同一个账号同时只能一个人登录](http://blog.51yip.com/php/1698.html?utm_source=tool.lu)
实现原理
1,用户在电脑A登录,session信息存放在redis当中，并将session_id存到mysql数据库中。
2,同一用户在电脑B登录，验证完用户名和密码后，将该用户信息从数据库读出，取得用户在电脑A登录的session_id，然后在到redis中验证session是否过期。
3,如果过期，不用openfire推送提示信息。如果没有过期，php利用openfire推送消息后，在将redis中用户在电脑A中登录的session删除掉，删除后，在将用户在电脑B登录的个人信息放到session中，并将电脑B登录的session_id更新到数据库中，在这里一定要先发送推送，然后在清空session，不然用户在电脑A收不到xmpp发过来的消息。
https://phpbestpractices.org/?utm_source=tool.lu  https://phptrends.com/?utm_source=tool.lu
###[Markdown Parser in PHP](http://parsedown.org/?utm_source=tool.lu)
###[php安全](http://phpsecurity.readthedocs.io/en/latest/index.html?utm_source=tool.lu)
###[APP后端开发系列：登陆系统设计中的注意问题](https://helei112g.github.io/2016/07/12/1-APP%E5%90%8E%E7%AB%AF%E5%BC%80%E5%8F%91%E7%B3%BB%E5%88%97%EF%BC%9A%E7%99%BB%E9%99%86%E7%B3%BB%E7%BB%9F%E8%AE%BE%E8%AE%A1%E4%B8%AD%E7%9A%84%E6%B3%A8%E6%84%8F%E9%97%AE%E9%A2%98/)
```js
验证通过后，把用户uid+username+salt等md5后，作为token返回到客户端。
对token加入时间戳，过期后客户端重新提交username + pwd验证后再发一个token到客户端
服务端生成一个token后下发到客户端，客户端按照约定的规则加密后请求服务端
参考oauth2.0，用户提交username + pwd后，服务端返回以下信息
{
    "access_token":"2YotnFZFEjr1zCsicMWpAA",
    "expires_in":3600,
    "refresh_token":"tGzv3JOkF0XG5Qx2TlKWIA"
}
access_token 是用来进行访问的接口的，expires_in 是他的过期时间，到达过期时间后，需要用 refresh_token 来请求服务端刷新 access_token。

这里几个重点是：refresh_token 仅能使用一次，使用一次后，将被废弃。另外这个 access_token 只在 expires_in 有效期内有效。

注意： 这里的 expires_in 仅返回秒数就好了。别返回时间戳。因为各个平台计算s的时间戳，不一致，这样子做更方便处理。
token常用的一种生成方式：

该用户的uid，如：8888
该用户的口令，如： 123123
用户对应的salt，如：abcd
过期时间戳，如：1468293948
把上面几部分拼接起来：888:123123:abcd:1468293948

token = md5(‘888:123123:abcd:1468293948’);
```
