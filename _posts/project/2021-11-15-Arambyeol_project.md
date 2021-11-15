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
ㄴ`📂/`
    ㄴ`__init__.py`
    ㄴ`chromedriver`
    ㄴ`db.py`
    ㄴ`get_data.py`
    ㄴ`schema.sql`
    ㄴ`user.py`
    ㄴ`📂/static`
        ㄴ`📂/static/css`
            ㄴ`📂/static/css/layout`
                ㄴ`footer.css`
                ㄴ`header.css`
            ㄴ`📂/static/css/member`
                ㄴ`login.css`
                ㄴ`register.css`
            ㄴ`📂/static/css/review`
                ㄴ`review.css`
            ㄴ`index.css`
        ㄴ`📂/static/js`
            ㄴ`📂/static/js/layout`
                ㄴ`footer.js`
                ㄴ`header.js`
            ㄴ`📂/static/js/member`
                ㄴ`login.js`
                ㄴ`register.js`
            ㄴ`📂/static/js/review`
                ㄴ`review.js`
            ㄴ`index.css`
        ㄴ`📂/static/images`
            ㄴ`empty_star.png`
            ㄴ`full_star.png`
            ㄴ`x_icon.png`
