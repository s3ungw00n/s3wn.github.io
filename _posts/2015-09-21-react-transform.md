---
layout: post
category: web
title:  "React-Transform"
date:   2015-09-21 15:00
tags: javascript, react, flux, redux 
comments: true
---
#React-Transform
React Transform은 기본적으로 [babel](http://nodejs.github.io/iojs-ko/articles/2015/05/11/story-about-js-and-babel/) plugin이다. 트랜스파일러인 babel을 이용해, React Component들을 커스터마이징해서 쓸 수 있게 해주는 것이다.  

현재 React Hot Loader도 deprecated되고 react-transform 기반으로 만들어진 react-transform-hmr로 넘어간 상태다. 

이름은 react-transform-(이름) 형식으로 지어지며, 현재 [공식페이지](https://github.com/gaearon/babel-plugin-react-transform)에는 4개의 react-transform들이 올라와있다. 공식페이지에서는 transform을 만드는 법 까지 가이드를 하고 있는데, 다양한 유저들의 참여를 바라는 듯하니 관심이있다면 참여해보자.

샘플 프로젝트는 [이곳](react-transform-boilerplate)에서 볼 수 있다.

#사용법

우선, 플러그인을 설치한다.

`npm install --save-dev babel-plugin-react-transform`

사용하고싶은 react-transform을 설치한다.
`npm install --save-dev react-transform-hmr`

`.babelrc` 이 없다면 파일을 만들고, `extra.react-transform`을 include한다. 해당 필드는 사용하고 싶은 transform들의 array여야한다.

예제)

	{
	  "stage": 0,
	  "env": {
	    "development": {
	      "plugins": ["react-transform"],
	      "extra": {
	        "react-transform": [{
	          "target": "react-transform-hmr",
	          "imports": ["react"],
	          "locals": ["module"]
	        }]
	      }
	    }
	  }
	}
	
찬찬히 살펴보자.  

일단, `env.development`안에 작성하여 모든 것이 개발버전에서만 돌아가도록한다. `plugins`로 `react-transform`을 정의해주고, 그 밑으로 위에서 언급한 `extra.react-transform`을 정의한다.  

해당 인자로는 **배열**(중요)이 들어가며, 사용할 transform들의 정보를 넣어주면 된다.  

각 transform들에 들어가는 필드를 알아보자 .  
`target`은 npm module이름이나, local path가 들어갈 수 있으며, 보통 사용하는 transform의 이름이 된다. `imports`, `locals`는, 해당 transform이 사용하는 의존성이나, 인자로 들어갈 변수이다. 해당 필드에 들어가는 내용은 사용하는 transform의 document를 잘 읽고 넣어야한다.

#Transform만들어보기
transform이 어떻게 동작하는지, `imports` `locals`가 무슨 용도인지 알고싶은 분들, 이런 transform이 있으면 좋겠는데! 라는 아이디어가 있으신 분들은 transform을 직접 만들어보는 것도 좋은 경험이 될 것 같다.

1. 이름정하기  
이름은 `react-transform-*`의 형태로 만들어서, 이름규칙을 맞춘다.  

2. function export하기  
Transform의 전체 구조는 function을 리턴하는 function이다.  
예제) 공식페이지 발췌
		
		export default function myTransform(options) {
		  return function wrap(ReactClass, uniqueId) {
		    return ReactClass;
		  }
		}
		
예제코드를 살펴보면, `ReactClass`가 인자로 들어온다. React Component가 사용되기전, 후킹되어 이 함수로 들어온다고 짐작할 수 있다. 우리는 이 함수에서 컴포넌트를 지지고 볶아서 리턴하면 되는 것이다.

`options`에는 앞서 언급한 `imports`, `locals`등 사용할 의존성이나 변수들이 담겨져 있어서, 원하는 동작을 할 수 있도록 만들 수 있을 것이다.

더 자세한 내용은 [공식페이지](https://github.com/gaearon/babel-plugin-react-transform#writing-a-transform)에서 확인 할 수 있다.

#마무리
정말정말 업데이트가 빠르다. 설치예제에서 사용한 `react-transform-hmr`만해도, 이 글을 작성하는 도중에 `react-transform-webpack-hmr`에서 이름이 바뀌었다.(-_-)

그래도, error를 렌더링화면에 띄워준다던가, 업데이트 된 것을 highlight 해준다던가 하는 것을, 후킹한 함수에서 처리한다는 발상은 참신한 것 같다. 나도 아이디어가 생기면 transform을 만들어서 NPM에 하나 등록해보고싶다.