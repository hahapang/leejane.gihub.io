## Web自动化测试技巧之Selenium

Web端自动化测试，一般分为后台接口测试和前端页面测试。后台接口测试使用工具postman。前台页面测试一般web端使用Selenium比较多，移动端用Appinum比价多。接口测试工具postman的使用之前介绍过，今天给大家介绍的是Selenium web端测试工具。


##Selenium + WebDriver 各浏览器驱动下载地址
Chrome
点击下载chrome的webdriver： http://chromedriver.storage.googleapis.com/index.html
不同的Chrome的版本对应的chromedriver.exe 版本也不一样，下载时不要搞错了。如果是最新的Chrome, 下载最新的chromedriver.exe 就可以了。
把chromedriver的路径也加到环境变量里。
 
Firefox
Firefox驱动下载地址为：https://github.com/mozilla/geckodriver/releases/
根据自己的操作系统下载对应的驱动即可，使用的话，需要把驱动的路径和火狐浏览器的路径加入到环境变量里面才可以
 
IE
IE浏览器驱动下载地址为：http://selenium-release.storage.googleapis.com/index.html
根据自己selenium版本下载对应版本的驱动即可，python的话，下载里面的IEDriverServerxxx.zip即可，这个是区分32和64位系统的，根据自己的系统下载即可，需要注意的是，如果要打开IE浏览器的话，需要在浏览器的Internet选项中的安全页里有4个安全选项，Internet、本地Internet、受信任的站点、受限制的站点，这4个里面都有一个启用保护模式，都需要勾选上才可以，还得把驱动的路径加入到环境变量中。
## 页面操作：Selenium定位元素使用方法

定位元素先要掌握基本的HTML语法：

http://blog.csdn.net/jojoy_tester/article/details/53222425

http://blog.csdn.net/jojoy_tester/article/details/53228674

webdriver属于selenium体系中设计出来操作浏览器的一套API，webdriver是python的一个用于实现web自动化的第三方库。

自动化要做的就是模拟鼠标和键盘来操作来操作这些元素，点击、输入、鼠标悬停等等。操作这些元素前首先要找到它们，WebDriver提供了8中定位元素的方法，python语言



from selenium import webdriver
 
url = "http://managerx.biddingx.com/#"
driver = webdriver.Firefox()
driver.implicitly_wait(20)
driver.get(url)
driver.find_element_by_id("LoginFormEmail").clear()  #通过id定位元素
driver.find_element_by_id("LoginFormEmail").send_keys("qqq.com")
driver.find_element_by_id("LoginFormPass").clear()   #通过id定位元素
driver.find_element_by_id("LoginFormPass").send_keys("xxxxxx")
driver.find_element_by_xpath(".//*[@id='SCENES_LOGIN']/div/div/div[2]/div[4]/button").click() #通过xpath定位，层级与属性结合定位
 
driver.quit()

所对应的方法如下：

id定位
webdriver提供的id定位的方法是通过元素的id属性来查找元素的：

find_element_by_id()

eg：

driver.find_element_by_id("LoginFormEmail")

name定位
find_element_by_name()

eg：通过name定位百度输入框

<input autocomplete="off" maxlength="255" value="" class="s_ipt" name="wd" id="kw">

find_element_by_name("wd").send_keys("python")

class 定位
通过元素类名定位元素，

find_element_by_class_name()

eg:元素类名定位百度输入框

find_element_by_class_name("s_ipt").send_keys("selenium")

tag定位（标签）
每一个元素本质就是一个tag，但是HTML页面的tag重复性很厉害，一般很少用这个定位。

find_element_by_tag_name()

eg：

find_element_by_tag_name（"input"） 通过input标签定位

link定位
link定位专门用来定位文本链接的。

find_element_by_link_text()

eg：

百度首页的导航栏代码如下：

<a class="mnav" name="tj_trnuomi" href="http://www.nuomi.com/?cid=002540">糯米</a>
<a class="mnav" name="tj_trnews" href="http://news.baidu.com">新闻</a>
<a class="mnav" name="tj_trhao123" href="http://www.hao123.com">hao123</a>
<a class="mnav" name="tj_trmap" href="http://map.baidu.com">地图</a>
<a class="mnav" name="tj_trvideo" href="http://v.baidu.com">视频</a>
<a class="mnav" name="tj_trtieba" href="http://tieba.baidu.com">贴吧</a>
<a class="lb" onclick="return false;" name="tj_login" href="https://passport.baidu.com/v2/?login&tpl=mn&u=http%3A%2F%2Fwww.baidu.com%2F">登录</a>
<a class="pf" name="tj_settingicon" href="http://www.baidu.com/gaoji/preferences.html">设置</a>
<a class="bri" style="display: block;" name="tj_briicon" href="http://www.baidu.com/more/">更多产品</a>
页面中这些文本是唯一的，所有可以通过link定位:
find_element_by_link_text("糯米").click()

find_element_by_link_text("新闻").click()

find_element_by_link_text("hao123").click()

find_element_by_link_text("地图").submit()

find_element_by_link_text("视频").click()

find_element_by_link_text("贴吧").click()

find_element_by_link_text("登录").click()

find_element_by_link_text("设置").submit()

find_element_by_link_text("更多产品").click()

##partial link 定位
partial定位是对link定位的一种补充，有些文本很长，这时候我们可以取文本的一部分定位。

find_element_by_partial_link_text()通过元素标签对之间的部分文本就能点位元素了

例子：

<a href="http://www.hao123.com" target="_blank" class="fuks" id="browser">我是一个很长的链接，你点击我就能打开百度了哈哈。</a>

find_element_by_partial_link_text("我是一个很长的链接")

find_element_by_partial_link_text("你点击我就能打开百度了")

##xpath定位
find_element_by_xpath()

绝对路径定位
从html标签开始，一层一层往下写标签，直到这个标签位置，这就是绝对路径。

登录126邮箱xpath定位：div[3]表示当前层级下的第三个div标签

driver.find_element_by_xpath("/html/body/div[2]/div[2]/div[2]/form/div/div/div[2]/input").send_keys("username")
driver.find_element_by_xpath("/html/body/div[2]/div[2]/div[2]/form/div/div[2]/div[2]/input").send_keys("xxx")
driver.find_element_by_xpath("/html/body/div[2]/div[2]/div[2]/form/div/div[6]/a").click()

###利用元素属性定位
find_element_by_xpath("//标签[@属性名=属性值]")  属性名可以是id、name、class或者其他可唯一标识该标签的元素

driver.find_element_by_xpath(".//input[@id='kw']").send_keys("haha")
driver.find_element_by_xpath(".//*[@id='su']").click()

//表示当前页面某个目录下，input表示定位元素的标签名，不指定标签名可以用*代替，[@id='kw']表示这个元素id属性值是kw。也可以用class、name属性值来定位。

find_element_by_xpath("//*[@name='wd']").send_keys("haha")

find_element_by_xpath(".//input[@class='bg s_btn']").submit()

其实也可以用其他属性值都可以，只要他能唯一标识这个元素：(定位126邮箱登录)

find_element_by_xpath("//*[@placeholder='邮箱帐号或手机号']").send_keys('username')

find_element_by_xpath("//*[@maxthlenght='16']").send_keys("password")

find_element_by_xpath("//input[@id='dologin']").submit()

##层级与属性结合
如果一个元素标签本身没有唯一的属性值标识自身，就可以结合层级来定位它了。

driver.find_element_by_xpath("//div[@class='m-container']/input").send_keys("username")

div[@class='m-container']通过class属性定位到父元素，后面/input就表示父元素下的子元素。如果父元素没有唯一的属性值，则可以找其爷爷元素。

find_element_by_xpath("//div[@class='hsfhg']/div[2]/input")

find_element_by_xpath(.//*[@id='SCENES_LOGIN']/div/div/div[2]/div[4]/button)

##通过逻辑运算符定位
find_element_by_xpath("//input[@id='a' and @class='su']/span/input")

xpath语法通过firepath插件可以生成，可以直接使用的。

css定位
find_element_by_css_selector()

css使用选择器来为页面元素绑定属性，可以较为灵活地选择控件的人已属性，一般情况下定位速度要比xpath快。

下面是百度搜索框的代码：

......
<span class="bg s_ipt_wr quickdelete-wrap">
<span class="soutu-btn"></span>
<input id="kw" class="s_ipt" autocomplete="off" maxlength="255" value="" name="wd">
<a id="quickdelete" class="quickdelete" href="javascript:;" title="清空" style="top: 0px; right: 0px; display: none;"></a>
</span>
<span class="bg s_btn_wr">
<input id="su" class="bg s_btn" type="submit" value="百度一下">
</span>
......
##通过class属性定位
find_element_by_css_selector(".类属性值")  class选择器选择class="a"的所有元素，点号(.)表示通过class属性定位元素

driver.find_element_by_css_selector(".s_ipt").send_keys("css")
driver.find_element_by_css_selector(".bg s_btn").click()
##通过id定位元素
find_element_by_css_selector("#id值") id选择器选择id="a"的所有元素，#号表示通过id属性定位元素

driver.find_element_by_css_selector("#kw").send_keys("huh")
driver.find_element_by_css_selector("#su").submit()

##通过标签名定位元素（少用）
find_element_by_css_selector("input")

##通过标签属性定位
find_element_by_css_selector("[属性名=属性值]")

driver.find_element_by_css_selector("[ maxlength='255']").send_keys("dfaf")
driver.find_element_by_css_selector("[ value='百度一下']").submit()

对于属性值来说，可加引号，也可以不加，但注意和整个字符串的引号进行区分！！！

##通过标签父子关系定位（少用吧）
find_element_by_css_selector("父标签>子标签") 

find_element_by_css_selector("span>input")表示选择父标签为span的所有input元素

##组合定位（通过父子标签和其属性结合）
组合定位可以大大加强定位元素的唯一性！

driver.find_element_by_css_selector("form#form>span>input").send_keys("asfa")
driver.find_element_by_css_selector("form#form>span>input#su").click()

要定位的元素是input，父元素是span，爷元素是form；

要定位的元素是input（可以结合它的属性值），父元素是span，爷元素是form（可以结合它的属性值）

其实通过css定位也可以像xpath定位那样通过firebug工具获得再修改，或选择要定位的标签后右键-》复制css路径也行。

##通过By定位
By定位元素是统一调用find_element()的方法。find_element()方法只用于定位元素，它有两个参数，第一个是定位的类型，由By提供；第二个参数是定位的具体方式。

使用By之前需要先导入By类：

from selenium .webdriver.commom.by import By

find_element(By.类型,"具体定位方式")

如：

driver.find_element(By.ID,"kw").send_keys("dsfads")
driver.find_element(By.CLASS_NAME,"bg s_btn").click()

find_element(By.NAME,"wd")

find_element(By.XPATH,"//*[@id='id']")

find_element(By.LINK_TEXT,"新闻")

find_element(By.PARTIAL_LINK_TEXT,"登")

find_element(By.TAG_NAME,"input")

##Selenium上传图片




## Selenium使用中的坑

###unknown error: call function result missing 'value'

上网查了应该是Chromedriver过老不支持当前浏览器版本，在网上搜了首先去chromedriver官网上下载最新驱动文件，替换后再试，替换后就好了。

注：chromedriver官网打不开的话，可以去淘宝镜像，地址：http://npm.taobao.org/mirrors

###无法定位到页面元素：Unable to locate element: 

这个问题比较常见，每种页面元素都有不同的属性和定位方式，需要找对定位方式一般就能够搞定，
有classname就用classname定位。链接一般用linktext，如果没有，xpath方法一般也比较常用。

###Class属性名有空格：Compound class names not permitted

当class的属性值，中间有空格时，通过by方法会报错，这个复合类名字不允许，其实就是嵌套类，使用最后一个作为类名即可

###无法聚焦页面元素：unknown error: cannot focus element
定位元素 程序没有报错确实定位成功，且执行click()事件时鼠标也的确能点击到，但是用send_keys输入内容时却报错，如图：unknown error: cannot focus element

###页面元素不可点击：Element is not clickable at point
原因及解决方法
无外乎四种原因

####元素未加载
没加载出来就等待元素加载出来，再往下执行。 
可以使用python库time

import time 
time.sleep(3)

不过最好还是使用selenium自带WebDriverWait

from selenium.webdriver.support.ui import WebDriverWait

WebDriverWait(driver, 10).until(EC.title_contains("元素"))

WebDriverWait的具体用法请点击参考文档。

####在iframe里
如果元素在iframe里，在窗口里找是找不到元素的，更是无法点击。所以，要切换到iframe里去找元素。

driver.switch_to_frame("frameName")  # 根据框架名来切换
driver.switch_to_frame("frameName.0.child")  # 子框架
driver.switch_to_default_content()  # 返回

其他相关切换，请点击参考文档

####不在视窗里，需要拉滚动条
很多网站的列表页不是立马返回所有内容，是根据视图来显示的。所以，我们就需要拖动滚动条来把要获取的内容显示到视窗里才可以获取到。

page = driver.find_element_by_partial_link_text(u'下一页')
driver.execute_script("arguments[0].scrollIntoView(false);"， page)
WebDriverWait(driver, 10).until(EC.element_to_be_clickable((By.PARTIAL_LINK_TEXT, u'下一页'))).click()


####要点击的元素被覆盖
可以使用事件链来解决 
例如下拉菜单，通过hover，让子菜单显示，就可以点击了。

menu = driver.find_element_by_css_selector(".nav")
hidden_submenu = driver.find_element_by_css_selector(".nav #submenu1")

ActionChains(driver).move_to_element(menu).click(hidden_submenu).perform()

关于事件链详情，请点击文档。

###find elements和find elment
findElements是用来查找一组元素，而findElement是用来查找匹配表达式的第一个元素。用混淆了会报错。'list' object has no attribute 'click'

##最后再总结一下，各种方式在选择的时候应该怎么选择：

1. 当页面元素有id属性时，最好尽量用id来定位。但由于现实项目中很多程序员其实写的代码并不规范，会缺少很多标准属性，这时就只有选择其他定位方法。

2. xpath很强悍，但定位性能不是很好，所以还是尽量少用。如果确实少数元素不好定位，可以选择xpath或cssSelector。

3. 当要定位一组元素相同元素时，可以考虑用tagName或name。

4. 当有链接需要定位时，可以考虑linkText或partialLinkText方式。