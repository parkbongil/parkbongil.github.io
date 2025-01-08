---
layout: post
title: Windows 10 64bit에서 Apache 64bit에 Flask with Python 3.6 배포
tags: [Apache, Flask]
---

최종적으로는 Ubuntu + Nginx에 Flask를 배포하겠지만, 윈도우에서 한번 테스트 해볼려다가 삽질한 내용을 정리해 본다. 사실 앞으로도 이렇게 할 일이 없어서 의미가 없을 것 같다.

OS : Windows 10 64bit  
Apache : 2.4.25 64bit (C:\Apache24)  
Python : 3.6 64bit (C:\Python\Python36)  
Flask : 0.12  
Project : hello_world (D:\Project\python\flaskwsgi\hello_world)  

취미로 Flask를 이용해서 웹사이트 만들어 보고 있는데, 새롭게 시작하는거라 윈도우, 파이썬, 아파치 모두 최신버전의 64bit를 설치했는데, mod_wsgi가 최신버전의 아파치와 파이썬을 지원하는 컴파일 버전을 제공하지 않았다.

### 0.
파이썬, 아파치, Flask 설치는 구글링해서 설치. 이슈 없음.

### 1. mod_wsgi 설치
C:\Apache24>pip install mod_wsgi  
C:\Python\Python36\Scripts>mod_wsgi-express module-config  
실행하면 에러가 나는데 C++ 컴파일러가 없어서 난다. 컴파일러 다운받아서 실행하면 된다.  
컴파일러 : [Visual C++ 2015 Build Tools](https://landinghub.visualstudio.com/visual-cpp-build-tools)

### 2. 프로젝트 샘플파일 생성
D:\Project\python\flaskwsgi\hello_world\__init__.py

~~~python
app = Flask(__name__)@app.route("/")
def hello_world():
   return "Hello World! 안녕하세요!"
~~~
D:\Project\python\flaskwsgi\hello_world.wsgi

~~~python
import sys
sys.path.insert(0, 'D:/Project/python/flaskwsgi')
from hello_world import app as application
~~~

### 3. Apache 세팅
아파치 설정파일인 httpd.conf파일을 열어서 LoadModule 설정된 부분의 하단에 모듈을 추가한다.  
LoadModule wsgi_module "C:/Python/Python36/Lib/site-packages/mod_wsgi/server/mod_wsgi.cp36-win_amd64.pyd"

프로젝트를 WSGI로 응답하도록 설정한다.  
WSGIScriptAlias / D:\Project\python\flaskwsgi\hello_world.wsgi
~~~
<Directory "D:/Project/python/flaskwsgi">
 WSGIApplicationGroup %{GLOBAL}
 Order deny,allow
 Allow from all
 Require all granted
</Directory>
~~~

### 4. Apache 시작 및 브라우저서 확인
Apache를 윈도우 서비스에 등록했으므로 작업관리자에서 재시작 하면 된다.
윈도우 서비스 등록 :  C:\Apache24\bin>httpd -k install

http://localhost

### 참고사이트
[http://www.flask.moe/windows](http://www.flask.moe/windows)   
[https://github.com/GrahamDumpleton/mod_wsgi/issues/188](https://github.com/GrahamDumpleton/mod_wsgi/issues/188)   
[https://groups.google.com/forum/#!topic/modwsgi/OTb_gN8kX7w](https://groups.google.com/forum/#!topic/modwsgi/OTb_gN8kX7w)   