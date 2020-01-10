---
layout  : post
title   : Jekyll + Github Pages 블로그에 Github Comment 기능 추가하기
date    : 2019-04-06 16:32:01 +0900
updated : 2019-04-06 17:03:07 +0900
tags    : 
toc     : true
public  : true
parent  : 
latex   : false
---
* TOC
{:toc}

# Github Comment?
여기서 말하는 Github Commnet 기능이란, Github의 Issue 기능을 이용해 댓글기능을
구현하는 것인데, 아이디어가 좋다.

블로그 포스트에 Github repository와 연결된 스크립트를 추가해서, 댓글을 남기면 
해당 repository의 issue로 등록이 된다. issue이름은 해당 블로그 포스트 링크로 
생성된다.(설정에서 바꿀 수 있음) 그리고 포스트에는 해당 이슈들이 파싱되어 나열
된다. 그리고 이렇게 남긴 댓글은 내가 지정한 label이 달린 issue로 저장되는데,
이 것만 필터링하면 issue페이지를 댓글 관리자페이지(!!!)로 사용할 수 있다.

댓글창도 Github와 같은 경험을 하게끔 디자인 되었고, Markdown도 사용할 수 있고
Preview기능도 지원된다. 

이 엄청난 아이디어는 [utterances](https://utteranc.es/)를 이용해 사용할 수 있다.
그리고 블로그에 추가하는 방법도 너무나 쉽다. 소프트웨어는 이렇게 만들어야한다.

# 추가방법
1. Utterances App 연결
[Utterances App](https://github.com/apps/utterances)로 들어가서 Github issue가
등록될 reporsitory와 연결해준다. Github Pages 블로그를 이용하는 경우 해당 
repository를  그대로 사용하면 같은 저장소에서 댓글을 확인할 수 있으니 일석이조다.

2. [Utterances 설정페이지](https://utteranc.es/)로 들어가서 `configuration`쪽의 
내용을 채운다.
- `Repository - repo` 1번에서 설정한 repository 를 입력한다.
- `Blog Post ↔️ Issue Mapping` 이슈와 맵핑될 방식을 지정한다. 나는 제일 첫 번째인
`pathname`으로 지정했다.
- `Issue Label` 에서는 댓글이 Github Issue에 등록될 때 달릴 Label을 지정한다.
본인 Github Pages Repository - Issue페이지에가서 Label하나를 생성한 후 그대로
복사해 입력한다. Emoji도 먹는다. 나는`💬blog comment`로 지정했다. 
- `Theme` 본인 블로그에 맞는 Theme를 선택한다.
- `Enable Utterances`의 스크립트를 복사한다.

3. Script 복붙
2번에서 복사한 스크립트를 Jekyll쪽에 붙여넣기한다.
Jekyll Theme에 따라서 다르겠지만, 일반적인 경우  `_layout/post.html` 쪽 하단에 
붙여넣기하면 바로 동작한다.

# 문제점
댓글창이 좀 늦게 뜬다. 로드타이밍을 좀 다르게 가져가면 될 것 같긴한데, 이건
시간이 나면 수정해봐야겠다.

