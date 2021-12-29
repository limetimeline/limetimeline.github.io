---
title:  "37.file-download-1"
categories:
  - wargame
tags:
  - wargame
  - Webhacking
last_modified_at: 2021-12-29
toc: true
toc_sticky: true
---

# Question info.
File Download 취약점이 존재하는 웹 서비스입니다.
flag.py를 다운로드 받으면 플래그를 획득할 수 있습니다.

# Function analysis
![37_1](/assets/images/wargame/37_1.PNG)
![37_2](/assets/images/wargame/37_2.PNG)
![37_3](/assets/images/wargame/37_3.PNG)

Upload My Memo에 파일명과 내용을 작성하면 Home에 업로드 되는듯하다.

# Code analysis
```python
#!/usr/bin/env python3
import os
import shutil

from flask import Flask, request, render_template, redirect

from flag import FLAG

APP = Flask(__name__)

UPLOAD_DIR = 'uploads'


@APP.route('/')
def index():
    files = os.listdir(UPLOAD_DIR)
    return render_template('index.html', files=files)


@APP.route('/upload', methods=['GET', 'POST'])
def upload_memo():
    if request.method == 'POST':
        filename = request.form.get('filename')
        content = request.form.get('content').encode('utf-8')

        if filename.find('..') != -1:
            return render_template('upload_result.html', data='bad characters,,')

        with open(f'{UPLOAD_DIR}/{filename}', 'wb') as f:
            f.write(content)

        return redirect('/')

    return render_template('upload.html')


@APP.route('/read')
def read_memo():
    error = False
    data = b''

    filename = request.args.get('name', '')

    try:
        with open(f'{UPLOAD_DIR}/{filename}', 'rb') as f:
            data = f.read()
    except (IsADirectoryError, FileNotFoundError):
        error = True


    return render_template('read.html',
                           filename=filename,
                           content=data.decode('utf-8'),
                           error=error)


if __name__ == '__main__':
    if os.path.exists(UPLOAD_DIR):
        shutil.rmtree(UPLOAD_DIR)

    os.mkdir(UPLOAD_DIR)

    APP.run(host='0.0.0.0', port=8000)
```

```python
from flag import FLAG

if filename.find('..') != -1:
            return render_template('upload_result.html', data='bad characters,,')
```

문제의 핵심은 이 두 부분이다.
분명 외부의 flag.py를 import하고 있으며 파일명 작성 시 '..'이 들어가면 'bad characters,,'라고 주의를 준다.

추측할 수 있는 내용은 flag.py에 FLAG가 들어 있고 upload할 때 파일명을 ../flag.py라고 못한다는 것이다.

# Solve
![37_4](/assets/images/wargame/37_4.png)

/read?name=test.txt 부분에 파일이름이 적혀있다.
그럼 여기를 ../flag.py라고 해주면 답이 나온다.

![37_5](/assets/images/wargame/37_5.png)

