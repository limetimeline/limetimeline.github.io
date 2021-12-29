---
title:  "AramByeol Project"
excerpt: "학교식당 식단표에 별점을 매기는 웹페이지."
categories:
  - Project
tags:
  - Project
  - Web
  - Scraping
last_modified_at: 2021-11-15
toc: true
toc_sticky: true
---




![AramByeol!!](/assets/images/project/arambyeol/Logo.png)

`학교 식당인 아람관 식단표 메뉴에 별점을 매기는 기능을 가진 웹페이지.`
![Arambyeol](http://arambyeol.kro.kr/,"Arambyeol Link")

# Develop things.
- Database : MariaDB
- API & Server File : Python Lib.(flask, Pymysql, datetime)
- Scraping & Put to DB : Python Lib.(Selenium, bs4, pyvirtualdisplay, subprocess, Pymysql), google_chrome, chrome_diver
- Html, CSS ,JavaScript Lib.(Jquery)
- Server : OracleCloud free server (Ubuntu 20.04)

# Directory Structure
```bash
📂/
├─__init__.py
├─chromedriver
├─db.py
├─get_data.py
├─get_auth.py
├─schema.sql
├─user.py
├─📂/static
│   ├─📂/static/css
│   │   ├─📂/static/css/error
│   │   │   └─error.css
│   │   ├─📂/static/css/member
│   │   │   ├─login.css
│   │   │   └─register.css
│   │   └─index.css
│   ├─📂/static/js
│   │   ├─📂/static/js/error
│   │   │   └─error.js
│   │   ├─📂/static/js/member
│   │   │   └─register.js
│   │   └─index.js
│   └─📂/static/images
│       ├─empty_star.png
│       ├─error.png
│       ├─favicon.ico
│       ├─full_star.png
│       ├─login.png
│       ├─logo.png
│       ├─logout.png
│       ├─x_icon.png
│       ├─뒤로가기.png
│       └─홈_로고.png
└─📂/templates
    ├─📂/templates/member
    │   ├─login.html
    │   └─register.html
    ├─📂/templates/error
    │   └─error.html
    └─index.html
```

