# Google-Image-Crawling
selenium을 사용한 구글 이미지 크롤링

## 1. Selenium 환경 셋팅
### 1-1. Selenium 설치 - Install
<pre>
<code>$ pip install selenium</code> 
</pre>
vscode Terminal을 통해 selenium 설치

### 1-2. venv 가상환경 생성
<pre>
<code>$ python -m venv selenium
$ cd selenium/Scripts
$ activate</code> 
</pre>
vscode에서 작업할 폴더를 선택하고 venv 가상환경을 사용하기 위해 venv를 설치\
그 후 selenium 폴더 내에 있는 Scripts폴더로 들어가 가상환경을 생성\
만약 activate 명령이 실행되지 않는 경우 '.\activate'로 명령어를 입력

## 2. WebDriver 설치
url에서 사용중인 크롬 버전에 맞게 다운로드\
Chrome : https://sites.google.com/a/chromium.org/chromedriver/downloads \
Edge : https://developer.microsoft.com/en-us/microsoft-edge/tools/webdriver/ \
Firefox : https://github.com/mozilla/geckodriver/releases \
Safari : https://webkit.org/blog/6900/webdriver-support-in-safari-10/ \
\
이번 프로젝트에서 사용한 Webdriver은 Chrome 

## 3. Google_Image_Crawling.py 코드 수정
### 3-1. 관련 라이브러리 import
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

### 3-3. 키워드 검색
<pre>
<code>driver = webdriver.Chrome(r"D:---")
driver.get("https://www.google.co.kr/imghp?hl=ko&tab=wi&authuser=0&ogbl")

elem = driver.find_element_by_name("q")
elem.send_keys(search)
elem.send_keys(Keys.RETURN)

# Get scroll height
last_height = driver.execute_script("return document.body.scrollHeight")</code> 
</pre>
webdriver.Chrome(r"D:---") : Chrome WebDriver의 경로를 작성\
driver.get( )에는 구글 이미지 검색의 url을 작성\
driver.find_element_by_name("q") : q는 검색창, 관련정보는 크롭에서 F12 눌러서 확인\
elem.send_keys(search) : 검색할 키워드\
elem.send_keys(Keys.RETURN) : 엔터키를 입력하는 코드\

### 3-4. 스크롤
<pre>
<code>SCROLL_PAUSE_TIME = 1

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
count = 1</code>
</pre>
SCROLL_PAUSE_TIME : 스크롤을 진행하면서 이미지가 로딩되는 시간\
driver.find_elements_by_css_selector(".mye4qd").click() : 더보기 버튼 클릭



### 3-4. 이미지 저장
<pre>
<code>for image in images:
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
driver.find_element_by_css_selector(".n3VNCb").get_attribute("src") : 이미지의 url을 뷸러오는 코드\
urllib.request.urlretrieve( ) : 이미지 저장


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
