--- 
layout: post
title: aws lambda에서 chrome driver 사용하기
categories: Python
tags:
 - python
 - lambda
 - aws
 - ubuntu
 - crawling
---

1. [여기]( https://github.com/inahjeon/AWS-LAMBDA-LAYER-Selenium)에서 SELENIUM_LAYER.zip을 내려받는다.    


2. aws lambda에서 왼쪽 메뉴 클리 > [계층] > [계층 생성]을 클릭한다.    
<img width="922" alt="계층생성" src="https://user-images.githubusercontent.com/63631604/102613309-9d3e0a80-4175-11eb-8127-9a153cffc5db.PNG">

3. SELENIUM_LAYER.zip을 업로드하고, 호환 런타임에 파이썬 3.6 선택 > [생성] 클릭   
<img width="645" alt="계층생성2" src="https://user-images.githubusercontent.com/63631604/102613311-9e6f3780-4175-11eb-824b-e55f516c3679.PNG">

4. 함수 생성 메인 페이지에서 함수 이름 박스 아래에 Layer 박스 클릭 > [add layer] 클릭     
<img width="844" alt="계층생성3" src="https://user-images.githubusercontent.com/63631604/102613314-9e6f3780-4175-11eb-9042-b188ae83b517.PNG">

5. [사용자 지정 계층] 선택, 방금 만든 레이어 선택. 처음 만들었으므로 버전은 1   
<img width="667" alt="계층생성4" src="https://user-images.githubusercontent.com/63631604/102613315-9f07ce00-4175-11eb-9a4a-b97156115648.PNG">

6. 메모리 용량은 최소 300M 이상 넉넉하게 잡아준다. 테스트 시에 에러 없이 무한로딩 된다면 메모리 용량을 늘려주면 된다.     
<img width="567" alt="계층생성5" src="https://user-images.githubusercontent.com/63631604/102613320-9fa06480-4175-11eb-82ad-bae015eabfc9.PNG">

7. 이제 아래와 같이 사용할 수 있다. 

```
from selenium.webdriver.chrome.options import Options
from selenium import webdriver

 
def get_driver(): #git에서 다운받은 SELENIUM_LAYER.zip을 사용했다면, 아래 설정 중 아무것도 건드리지 않는다. 
    chrome_options = Options()
    chrome_options.add_argument('--headless')
    chrome_options.add_argument('--no-sandbox')
    chrome_options.add_argument('--disable-gpu')
    chrome_options.add_argument('--window-size=1280x1696')
    chrome_options.add_argument('--user-data-dir=/tmp/user-data')
    chrome_options.add_argument('--hide-scrollbars')
    chrome_options.add_argument('--enable-logging')
    chrome_options.add_argument('--log-level=0')
    chrome_options.add_argument('--v=99')
    chrome_options.add_argument('--single-process')
    chrome_options.add_argument('--data-path=/tmp/data-path')
    chrome_options.add_argument('--ignore-certificate-errors')
    chrome_options.add_argument('--homedir=/tmp')
    chrome_options.add_argument('--disk-cache-dir=/tmp/cache-dir')
    chrome_options.add_argument('user-agent=Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/61.0.3163.100 Safari/537.36') 
    chrome_options.binary_location = "/opt/python/bin/headless-chromium" 
    
    driver = webdriver.Chrome('/opt/python/bin/chromedriver', chrome_options=chrome_options)
    return driver
    
    
def lambda_handler(event, context): 
    driver = get_driver()
    driver.get('https://www.google.com/')
    page_data = driver.page_source
    driver.close()
    
    return page_data
```




