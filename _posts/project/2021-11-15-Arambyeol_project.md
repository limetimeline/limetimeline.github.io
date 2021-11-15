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

학교 식당인 아람관 식단표 메뉴에 별점을 매기는 기능을 가진 웹페이지.

# Develop things.
- Database : MariaDB
- API & Server File : Python Lib.(flask, Pymysql, datetime)
- Scraping & Put to DB : Python Lib.(Selenium, bs4, pyvirtualdisplay, subprocess, Pymysql), google_chrome, chrome_diver
- Html, CSS ,JavaScript Lib.(Jquery)
- Server : OracleCloud free server (Ubuntu 20.04)

# Directory Structure
`ㄴ📂/
    ㄴ__init__.py
    ㄴchromedriver
    ㄴdb.py
    ㄴget_data.py
    ㄴschema.sql
    ㄴuser.py
    ㄴ📂/static
        ㄴ📂/static/css
            ㄴ📂/static/css/layout
                ㄴfooter.css
                ㄴheader.css
            ㄴ📂/static/css/member
                ㄴlogin.css
                `register.css
            ㄴ📂/static/css/review
                ㄴreview.css
            ㄴindex.css
        ㄴ📂/static/js
            ㄴ📂/static/js/layout
                ㄴfooter.js
                ㄴheader.js
            ㄴ📂/static/js/member
                ㄴlogin.js
                ㄴregister.js
            ㄴ📂/static/js/review
                ㄴreview.js
            ㄴindex.css
        ㄴ📂/static/images
            ㄴempty_star.png
            ㄴfull_star.png
            ㄴx_icon.png`
