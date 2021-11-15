---
title:  "AramByeol Project"
excerpt: "학교식당 식단표에 별점을 매기는 웹페이지."
categories:
  - Project, Web
tags:
  - Project, Web, Scraping
last_modified_at: 2021-11-15
toc: true
toc_sticky: true
---

![AramByeol!!](/assets/images/project/arambyeol/Logo.png){: width="300" height="300")


``
학교 식당인 아람관 식단표 메뉴에 별점을 매기는 기능을 가진 웹페이지.
``

# Develop things.
- Database : MariaDB
- API & Server File : Python Lib.(flask, Pymysql, datetime)
- Scraping & Put to DB : Python Lib.(Selenium, bs4, pyvirtualdisplay, subprocess, Pymysql), google_chrome, chrome_diver
- Html, CSS ,JavaScript Lib.(Jquery)
- Server : OracleCloud free server (Ubuntu 20.04)

# Directory Structure
```bash
📂/
├───__init__.py
├───chromedriver
├───db.py
├───get_data.py
├───schema.sql
├───user.py
├───📂/static
│   ├───📂/static/css
│   │   ├───📂/static/css/layout
│   │   │   ├──footer.css
│   │   │   └──header.css
│   │   ├───📂/static/css/member
│   │   │   ├──login.css
│   │   │   └──register.css
│   │   ├───📂/static/css/review
│   │   │   └──review.css
│   │   └──index.css
│   ├───📂/static/js
│   │   ├───📂/static/js/layout
│   │   │   ├──footer.js
│   │   │   └──header.js
│   │   ├───📂/static/js/member
│   │   │   ├──login.js
│   │   │   └──register.js
│   │   ├───📂/static/js/review
│   │   │   └──review.js
│   │   └─index.js
│   └───📂/static/images
│       ├──empty_star.png
│       ├──full_star.png
│       └──x_icon.png
└───📂/templates
    ├───📂/templates/member
    │   ├───login.html
    │   └───register.html
    ├───📂/templates/review
    │   └───review.html
    └───index.html
```

