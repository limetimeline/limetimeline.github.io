---
title:  "session-basic"
excerpt: "session-basic"
categories:
  - wargame
tags:
  - wargame
  - Webhacking
last_modified_at: 2022-09-03
toc: true
toc_sticky: true
---

# Question info.
쿠키와 세션으로 인증 상태를 관리하는 간단한 로그인 서비스입니다.   
``admin`` 계정으로 로그인에 성공하면 플래그를 획득할 수 있습니다.있습니다.

# Function analysis
![session-basic_1](/assets/images/wargame/session-basic_1.png)  

![session-basic_2](/assets/images/wargame/session-basic_2.png)  

About은 열리지 않는다.  
Login을 해야하는듯 하다.

# Code analysis
```python
#!/usr/bin/python3
from flask import Flask, request, render_template, make_response, redirect, url_for

app = Flask(__name__)

try:
    FLAG = open('./flag.txt', 'r').read()
except:
    FLAG = '[**FLAG**]'

users = {
    'guest': 'guest',
    'user': 'user1234',
    'admin': FLAG
}


# this is our session storage 
session_storage = {
}


@app.route('/')
def index():
    session_id = request.cookies.get('sessionid', None)
    try:
        # get username from session_storage 
        username = session_storage[session_id]
    except KeyError:
        return render_template('index.html')

    return render_template('index.html', text=f'Hello {username}, {"flag is " + FLAG if username == "admin" else "you are not admin"}')


@app.route('/login', methods=['GET', 'POST'])
def login():
    if request.method == 'GET':
        return render_template('login.html')
    elif request.method == 'POST':
        username = request.form.get('username')
        password = request.form.get('password')
        try:
            # you cannot know admin's pw 
            pw = users[username]
        except:
            return '<script>alert("not found user");history.go(-1);</script>'
        if pw == password:
            resp = make_response(redirect(url_for('index')) )
            session_id = os.urandom(32).hex()
            session_storage[session_id] = username
            resp.set_cookie('sessionid', session_id)
            return resp 
        return '<script>alert("wrong password");history.go(-1);</script>'


@app.route('/admin')
def admin():
    # what is it? Does this page tell you session? 
    # It is weird... TODO: the developer should add a routine for checking privilege 
    return session_storage


if __name__ == '__main__':
    import os
    # create admin sessionid and save it to our storage
    # and also you cannot reveal admin's sesseionid by brute forcing!!! haha
    session_storage[os.urandom(32).hex()] = 'admin'
    print(session_storage)
    app.run(host='0.0.0.0', port=8000)

```

```
@app.route('/admin')
def admin():
    # what is it? Does this page tell you session? 
    # It is weird... TODO: the developer should add a routine for checking privilege 
    return session_storage
```
``/admin``에 접속해보면 무언가 나올 것 같다.

# Solve
![session-basic_3](/assets/images/wargame/session-basic_3.png)

``/admin``에 접속해보면 ``seesion_storage``에 있는 값들이 ID와 매칭되어서 나온다.   
이 세션값을 세션ID에 매칭시키면 플래그가 나올 것이다.

![session-basic_4](/assets/images/wargame/session-basic_4.png)   
ID : user 또는 guest   
PW : user1234 또는 guest   
로그인 한 후 ``document.cookie``로 쿠키ID와 쿠키 값을 알 수 있다.

![session-basic_5](/assets/images/wargame/session-basic_5.png)   
위와 같이 쿠키값을 admin 세션값으로 변경해주고 새로고침을 해주면 플래그를 구할 수 있다. 

![session-basic_6](/assets/images/wargame/session-basic_6.png)






