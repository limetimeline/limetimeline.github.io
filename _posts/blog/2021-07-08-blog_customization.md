---
title:  "Blog Customization"
excerpt: "minimal-mistake 테마를 사용한 깃허브 블로그를 꾸며보자!"
categories:
  - Blog
tags:
  - Blog
last_modified_at: 2021-07-08
toc: true
---

# Navigation

- `/_data/navigation.yml`에서 설정.

```markdown
main:
  - title: "About"
    url: /about/
  - title: "Categories"
    url: /categories/
```

## - About

- `/_pages/about.md` 생성

```markdown
---
	permalink: /about/
	title: "About"
	last_modified_at: 2021-07-08
	toc: true
---
내용을 쓰면 된다!
```

- 이렇게만 적으면 내 프로필이 안나온다!!!
- `/_config.yml`을 열어서 아래와 같이 추가해주자!

```markdown
# Defaults
defaults:
# _pages
  - scope:
      path: ""
      type: pages
    values:
      layout: single
      author_profile: true
```

- 이제 잘 나오는 것을 확인할 수 있다!

## - Category

- `/_pages/category-archive.md` 생성

```markdown
---
title: "Posts by Category"
layout: categories
permalink: /categories/
author_profile: true
---
```

# favicon
- 주소창에 아이콘을 적용해보자!

1. https://favicon.io/emoji-favicons 같은 곳에서 원하는 아이콘을 다운로드 받자!
2. 압축을 풀어서 안의 파일을 모두 블로그 `Repository 최상위 디렉토리`에 다 넣자!
3. `png`나 `jpg` 이미지 파일을 https://realfavicongenerator.net/ 에서 `select your favicon image`버튼을 눌러 업로드하자!
4. 곧 `Generate your Favicons and HTML code`가 나타면 클릭하고 HTML코드를 복사하자!
5. HTML태그를 `/_includes/head/custom.html`에 붙여넣고 저장하자!

- 아래 코드를 참고!

```html
<link rel="apple-touch-icon" sizes="180x180" href="/apple-touch-icon.png">
<link rel="icon" type="image/png" sizes="32x32" href="/favicon-32x32.png">
<link rel="icon" type="image/png" sizes="16x16" href="/favicon-16x16.png">
<link rel="manifest" href="/site.webmanifest">
<link rel="mask-icon" href="/safari-pinned-tab.svg" color="#5bbad5">
<meta name="msapplication-TileColor" content="#00aba9">
<meta name="theme-color" content="#ffffff">
```

# Highlight
- Code Highlight : Programing Lang을 입력해주면 해당 언어의 변수나 함수에 색을 칠해준다.
- Inline Highlight : 원하는 문장에 색깔을 입혀 강조할 수 있다.

## - Code Highlight 

````
```Python
	def syntaxHighlight():
	val = 10
	return val
```
````

<br>

~~~Python
	def syntaxHighlight():
	val = 10
	return val
~~~

## - Inline Highlight

```
	`강조해보자!`
```

`강조해보자!`

	
