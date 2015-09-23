---
category: category
title:  "[ES6] Promise"
date:   2015-09-23 16:53
tags: javascript, es6, promise
comments: true
---
#Promise
Promise란 비동기, 또는 연기된 연산작업을 할 때 사용되는 기술이다. (출처: [공식문서](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)) 기존에도 여러 라이브러리에서도 자체구현하거나, Polyfill로 사용하고있었는데, ES6에서 공식지원하기 시작했다.

#문법
	new Promise(executor);
	new Promise(function(resolve, reject) { ... });
	
executor는 함수 오브젝트이고, 그 함수는 resolve, reject를 인자로 받는다. 이름그대로, resolve는 promise가 성공적으로 수행되었을때(fulfilled), reject는 실패/거절할때(rejected) 한번씩 호출할 함수들이다.

#개요
Promise는 비동기작업에 대해서, 성공, 실패의 핸들러를 가질 수 있다. 어떠한 비동기함수가 Promise를 리턴하게 된다면, 마치 동기함수처럼 리턴값을 가지는 형태로 작업을 처리할 수 있을 것이다.

Promise의 상태는 3가지로 정의할수 있는데,  Pending, Fulfilled, Rejected 이다. 처음 init 되면 Pending에서 시작하며, promise의 `then`메소드가 호출됨으로써 Fulfilled, Rejected 각각의 핸들러가 큐잉된다. Promise가 Fulfilled, Rejected상태로 변경된 상태를 settle되었다고 표현한다.

`then`, `catch` 메소드는 promise를 리턴하므로, 체이닝하여 사용할 수 있다.(!)

#예제
먼저, Promise를 생성하는 방법을 알아보자.

	var promise = new Promise(function(resolve, reject) {	
	  if (something.works) {
	    resolve("worked!");
	  }
	  else {
	    reject(Error("woops!"));
	  }
	});
	
인자로 함수를 받아, 특정조건을 만족하면, 콜백을 호출해 주는 식이다. 앞서 말한 것처럼, 성공하면 resolve를, 실패하면 reject를 호출한다.

이것을 사용하는 법은 다음과 같다.

	promise.then(function(result) {
		console.log(result); // "worked!"
	}, function(err) {
		console.log(err); // Error: "woops!"
	});
	
실제로, 이미지를 로딩한다고 하면 다음과같이 쓸 것이다.

	img1.ready().then(function() {
	  // 로딩되었을 때
	}, function() {
	  // 실패했을 때
	});

	// 그리고…
	Promise.all([img1.ready(), img2.ready()]).then(function() {
	  // 모두 로딩되었을 때
	}, function() {
	  // 하나 이상이 실패했을 때
	});

Promise.all은 인자로 온 promise들이 전부 완료 되면, settle상태로 들어가는 역할을 한다. 

내가 참고한 예제사이트가 글이 참 재미있게 되어있고, 심도있는 내용을 담고있으니 꼭 확인해보기 바란다
([예제참고](http://www.html5rocks.com/ko/tutorials/es6/promises/))

#마무리
얼마전 XCode가 7로 버전업 되면서 강제로 swift 2.0을 사용하게 되었는데, 나한테 가장 큰 변화점이 'async로 다 바꿔!' 라는 거 였다. 머리로는 이해하지만, 귀찮다고 생각을 했는데, 문득 promise를 보다보니 아! 얘네들이 이런걸 지향한거구나! 라고 알게되었다. 코드 사이사이에 있던 dispatch()등이 이러한 노력의 산물이었던 것 같다.([Promises in Swift](https://medium.com/@robringham/promises-in-swift-66f377c3e403))  

ES6 generator를 이용한 yield 코루틴에 대한 내용도 재밌어보여서 공부해서 또 글을 써봐야겠다. 