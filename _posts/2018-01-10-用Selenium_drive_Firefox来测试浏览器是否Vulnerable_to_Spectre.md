+  蹭个热点，看到已经有个网页版本的测试浏览器是否Vulnerable to Spectre，我就弄了这么个玩意儿。
这个网页URL是：<p>
http://xlab.tencent.com/special/spectre/spectre_check.html<p>
我研究了下，检测的JS脚本是用Ajax做的，当浏览器下载所有JS脚本后，点击测试就是在本地完成的，而且页面没有button这种html的元素存在，刚开始有点忐忑，因为知道button我可以click() ，但是怕在这里，我是对一个div click() 有点心里没底，但是结果却是非常好。 

+ 代码如下

        from selenium import webdriver 
        import time
        
        def spectre_checker(url):
            #Create a Firefox session 
            driver = webdriver.Firefox()
            driver.implicitly_wait(30)
            driver.maximize_window()
            
            #Fetch the application home page 
            driver.get(url)
            time.sleep(2)
            clicker=driver.find_element_by_xpath('//div[@class="progressbar"]')
            clicker.click()
            
            time.sleep(2)
            answer=driver.find_element_by_xpath('//pre[@id="notification"]/span')
        
            print(answer.text)
            
            #close browser window
            driver.quit()
        
        url='http://xlab.tencent.com/special/spectre/spectre_check.html'
        spectre_checker(url)


+  我在Pyzo里面测试的结果如下（当然我早知道这个结果了)
在微博上看到过”Libre盖子“的说法
”Firefox 可以在 about:config 里将 javascript.options.shared_memory 设置为 False“

- 感觉比较郁闷的一点是在本地retext里面和push到github里面最终呈现出来的内容是不一样的，搞得我很郁闷。
