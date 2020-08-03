# selenium设置等待时间

[转载](javascript:;)2015-06-18 07:56:10

标签：[pythonselenium](http://search.sina.com.cn/?c=blog&q=pythonselenium&by=tag)[自动化测试](http://search.sina.com.cn/?c=blog&q=%D7%D4%B6%AF%BB%AF%B2%E2%CA%D4&by=tag)

**#导入 WebDriverWait 包**

****from  selenium.webdriver.support.ui import WebDriverWait

**#导入 time 包**import time

**sleep()**    设置固定等待时间

——如：time.sleep(5)  #等待5秒

**implicitly_wait()**    等待一个元素被发现，或一个命令完成，超出了设置时间则抛出异常

——如：driver.implicitly_wait(30)

​           driver.find_element_by_id("id").click()

**WebDriverWait()**    在设置时间内，默认每隔一段时间检测

检测一次当前页面元素是否存在，如果超过设置时间检测不到则抛出异常

WebDriverWait(driver, timeout, poll_frequency=0.5, ignored_exceptions=None)

——driver：WebDriver 的驱动程序(Ie, Firefox, Chrome 或远程)

——timeout：最长超时时间，默认以秒为单位

——poll_frequency：休眠时间的间隔（步长）时间，默认为 0.5 秒

——ignored_exceptions：超时后的异常信息，默认情况下抛 NoSuchElementException 异常

——如1：element = WebDriverWait(driver, 10).until(lambda

 x : x.find_element_by_id("id"))

​           element.send_keys("selenium")

——如2：element = WebDriverWait(driver, 10).until(lambda x: x.find_element_by_id(“Id”))

​            is_disappeared = WebDriverWait(driver, 30, 1, (ElementNotVisibleException)).until_not(lambda

 x: x.find_element_by_id(“someId”).is_displayed())

WebDriverWai()一般由 unit()或 until_not()方法配合使用:

——until(method, message=’’)      调用该方法提供的驱动程序作为一个参数，直到返回值不为 False。——until_not(method, message=’’)      调用该方法提供的驱动程序作为一个参数，直到返回值为 False。