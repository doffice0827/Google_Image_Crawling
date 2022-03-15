# Google-Image-Crawling
selenium을 사용한 구글 이미지 크롤링

## 1. Selenium 설치 - Install
<pre>
<code>pip install selenium</code> 
</pre>

## 2. Chrome WebDriver 설치
url에서 사용중인 크롬 버전에 맞게 다운로드\
https://sites.google.com/a/chromium.org/chromedriver/downloads

## 3. Google_Image_Crawling.py 코드 수정
### 3-1. import
<pre>
<code>from selenium import webdriver
from selenium.webdriver.common.keys import Keys
import time
import urllib.request</code> 
</pre>

### 3-2. 조건설정
<pre>
<code>search = "Cross driver"
file_name = "Cross_driver"
save_path = "D:---"
save_folder = "Cross_driver"
count_max = 1000</code> 
</pre>
search : 검색할 키워드\
file_name : 이미지를 저장할 이름\
save_path : 이미지를 저장할 경로\
save_folder : 저장할 폴더 이름\
count_max : 최대 크롤링 갯수

### 3-3. Chrome WebDriver 경로 설정
<pre>
<code>driver = webdriver.Chrome(r"D:---")
driver.get("https://www.google.co.kr/imghp?hl=ko&tab=wi&authuser=0&ogbl")

elem = driver.find_element_by_name("q")
elem.send_keys(search)
elem.send_keys(Keys.RETURN)

SCROLL_PAUSE_TIME = 1
# Get scroll height
last_height = driver.execute_script("return document.body.scrollHeight")</code> 
</pre>
(r"D:---") 부분에 Chrome WebDriver의 경로를 작성

### 3-4. Crawling
<pre>
<code>while True:
    # Scroll down to bottom
    driver.execute_script("window.scrollTo(0, document.body.scrollHeight);")
    # Wait to load page
    time.sleep(SCROLL_PAUSE_TIME)
    # Calculate new scroll height and compare with last scroll height
    new_height = driver.execute_script("return document.body.scrollHeight")
    if new_height == last_height:
        try:
            driver.find_elements_by_css_selector(".mye4qd").click()
        except:
            break
    last_height = new_height

images = driver.find_elements_by_css_selector(".rg_i.Q4LuWd")
count = 1
for image in images:
    try:
        image.click()
        time.sleep(2)
        imgUrl = driver.find_element_by_css_selector(".n3VNCb").get_attribute("src")
        opener=urllib.request.build_opener()
        opener.addheaders=[('User-Agent','Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/36.0.1941.0 Safari/537.36')]
        urllib.request.install_opener(opener)
        urllib.request.urlretrieve(imgUrl, save_path + save_folder + "/" + file_name + str(count) + ".jpg")
        count = count + 1

        if count > count_max:
            break

    except:
        pass

driver.close()</code>
</pre>

## 4. Code
<pre>
<code>from selenium import webdriver
from selenium.webdriver.common.keys import Keys
import time
import urllib.request

search = "Cross driver"
file_name = "Cross_driver"
save_path = "D:---"
save_folder = "Cross_driver"
count_max = 1000

driver = webdriver.Chrome(r"D:---")
driver.get("https://www.google.co.kr/imghp?hl=ko&tab=wi&authuser=0&ogbl")

elem = driver.find_element_by_name("q")
elem.send_keys(search)
elem.send_keys(Keys.RETURN)

SCROLL_PAUSE_TIME = 1
# Get scroll height
last_height = driver.execute_script("return document.body.scrollHeight")
while True:
    # Scroll down to bottom
    driver.execute_script("window.scrollTo(0, document.body.scrollHeight);")
    # Wait to load page
    time.sleep(SCROLL_PAUSE_TIME)
    # Calculate new scroll height and compare with last scroll height
    new_height = driver.execute_script("return document.body.scrollHeight")
    if new_height == last_height:
        try:
            driver.find_elements_by_css_selector(".mye4qd").click()
        except:
            break
    last_height = new_height

images = driver.find_elements_by_css_selector(".rg_i.Q4LuWd")
count = 1
for image in images:
    try:
        image.click()
        time.sleep(2)
        imgUrl = driver.find_element_by_css_selector(".n3VNCb").get_attribute("src")
        opener=urllib.request.build_opener()
        opener.addheaders=[('User-Agent','Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/36.0.1941.0 Safari/537.36')]
        urllib.request.install_opener(opener)
        urllib.request.urlretrieve(imgUrl, save_path + save_folder + "/" + file_name + str(count) + ".jpg")
        count = count + 1

        if count > count_max:
            break

    except:
        pass

driver.close()
</code>
</pre>
