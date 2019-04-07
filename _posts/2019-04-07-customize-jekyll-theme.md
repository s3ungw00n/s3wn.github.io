---
layout  : post
title   : Jekyll 테마 커스터마이징 하기
date    : 2019-04-07 20:54:00 +0900
updated : 2019-04-07 21:06:08 +0900
tags    : 
toc     : true
public  : true
parent  : 
latex   : false
---
* TOC
{:toc}

# 원리
`jekyll new <PATH>` 로 최초 생성하고나면, `assets`, `_layouts`, `_includes`, `_sass`
폴더는 테마의 gem안에 숨겨져 있다.

해당 폴더/파일들을 jekyll 폴더구조에 복사해오면, 해당 파일들은 테마를 override
한다.

참고로 최초로 설정되어있는 테마 이름은 `minima`이다.

# 테마의 gem 위치 찾기
```
# On Mac OS
open $(bundle show [테마이름])
```

`bundle show [테마이름]`은 해당 테마의 path를 리턴해준다. 해당 폴더를 열어 
필요한 파일/폴더를 복사해오면된다.

# 참고 링크 및 추가사항
<https://jekyllrb.com/docs/themes/>
