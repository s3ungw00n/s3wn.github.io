---
layout: post
title: 'React.js에서 Mocha 로 TDD 해보기'
categories: [dev]
tags: [react, tdd]
---

  최근에 만난 멘토님께 TDD에 관하여 멘토링을 받았는데 너무 충격적이고 재미있어서! TDD포스팅을 해보려한다. 멘토링 자체는  [CyberDojo](http://www.cyber-dojo.org) 에서 진행하였는데, 다양한 언어로 TDD로 다양한 문제들을 풀어볼 수 있어서 자주 들어가서 이용해야겠다. 
  
  현재 진행하는 React.js 프로젝트에서, 테스트 코드를 작성해보기 위하여, mocha을 공부하면서 포스팅을 해봐야겠다.
 	
 	
# TDD Cycle
  TDD를 진행할 때는 다음 3가지 Workflow를 따른다.
  
	1. 실패하는 테스트 코드 작성
	2. 가장 빠르게 테스트를 통과하는 코드 작성
	3. 리팩토링

  멘토님과 페어프로그래밍을 하면서 (내가 거의 코드를 못짜서 내 실력의 부족을 많이 느꼈다!) 이 3가지를 주고받으며 코딩을하니 매우 재미있었다. 이것을 PingPong 이라고 한다고 하셨다.
  
  가장 빠르게 테스트를 통과하는 코드 작성을 할 때는 약간 폭탄돌리기 느낌이었다! 재미있는 부분이기도 했지만 리팩토링때 어차피 고쳐야하기 때문에 풀고자 하는 방향자체는 잡고 가야하는 것 같다. 
  
  
# 시작해보기
  TDD예제로는 PrimeFactor(소인수) 찾기를 TDD로 코딩해보고자 한다. 
  작은 소인수부터 리스트로 리턴하는 함수를 구현하면 된다. 기대값은 아래와 같다. 
  
>2 => [2]  
>3 => [3]  
>4 => [2, 2]  
>6 => [2, 3]  
>12 => [2, 2, 3]  
  
 * ES6기반 React.js 프로젝트에서 시작한다고 가정한다.  
 
# Mocha.js 설치
	npm install --save-dev mocha
 
 Mocha는 테스트 환경을 제공해주고, assertion library는 제공하지 않는다. expect, should 스타일 등을 설치해서 사용할 수 있는데, 모두 지원하는 chai를 사용해보고자 한다.
	npm install --save-dev chai  
	
# 테스트 환경 준비하기
 우선, test폴더를 만들어야한다. 해당 위치에 임의의 이름으로 테스트 소스 하나를 만든다. 빈 테스트 suite를 하나 준비해보았다.
 
 test/test.js  
 
	import chai from 'chai'
	
	chai.should()

	describe ('Find Prime Number', () => {
	  it ('returns prime factor array', () => {
	  })
	})

다음, 테스트를 실행할 스크립트를 작성한다.

package.json
	
	...
	"test": "cross-env NODE_ENV=test mocha --recursive --compilers js:babel-register -w"
	
cross-env를 이용해 실행 환경을 세팅하여주고, --recursive로 test 폴더내의 테스트들을 모두 실행하고, es6 Babel transcompiler를 이용하기 위해 babel-register hook을 이용했다. -w 옵션은 파일이 수정될 때마다 테스트를 돌려주는 옵션이다.

이러면 테스트 준비 끝이다. 앞으로 'npm test' 를 실행시키면 파일이 바뀔 때마다 테스트 결과를 알려줄 것이다.

# 실패하는 테스트 케이스 만들기
src/utils/findPrime.js

	export default function findPrime(value) {
	  return value
	}
	
test/test.js

	import findPrime from '../src/utils/findPrime'
	...
	describe ('Find Prime Number', () => {
	  it ('returns prime factor array', () => {
	    findPrime(2).should.deep.equal([ 2 ])
	  })
	})
	
첫 번째 실패하는 테스트 케이스르 만들었다. 현재 findPrime은 array를 리턴하지 않기때문에 테스트가 실패한다.  
여기서 'deep'을 사용한 이유는, array의 동치성을 표현하기 위함이다. 그냥 equal을 하게되면, [2]라는 결과를 제대로 리턴해도, object 자체를 비교하기 때문에 테스트가 실패하게 된다. deep 을 이용해 각 요소를 비교하는 테스트를 하는 것이다.  
다른 방법이라면 Immutable을 리턴하고, 비교하는 방법이 있을 것 같다.

# 가장 빠르게 테스트를 통과시키자
해결하려는 문제는 '소인수'를 찾는 것이지만, 현재 Step 의 가장 중요한 것은 가장 빠르게, 가장 변화를 일으키지 않는 선에서 테스트를 통과시키는 것이다.

src/utils/findPrime.js

	export default function findPrime(value) {
	  const result = []
	  result.push(value)
	  return result
	}

test에 통과했다. 부정행위를 저지른 느낌이지만 괜찮다.  
아직은 리팩토링이 필요한 단계는 아니기 때문에, 다시 테스트케이스를 작성한다.

# Cycle !
이제부턴 반복이다.  
실패하는 케이스 케이스를 작성하자.  

test/test.js

	...
	findPrime(4).should.deep.equal([ 2, 2 ])
	...


테스트케이스를 통과하자.  

src/utils/findPrime.js

	export default function findPrime(value) {
	  const result = []
	  let remain = value
	  
	  if (remain === 4) {
	    remain /= 2
	    result.push(remain)
	  }
	  
	  result.push(remain)
	  return result
	}
	
내키는 대로 4일때만 2로 나눈 수와 남은 수를 push해서 테스트를 통과 했다. 아직 공부중이라 이 부분이 어려운데, 테스트 케이스만을 통과시키기 위하여 소인수를 구하는 것과를 전혀 상관없는 룰을 만들어 코드를 작성했다. 분명 문제가 있는 것 같은데, Test Driven이기 때문에 그대로 진행해 보았다.  
다음은 리팩토링 단계인데, 나는 이 부분에서 고민을 많이 했다. 내가 임의로 통과한 룰에서의 리팩토링을 할 것인가? 아니면 처음 목적에 부합하게 뜯어고칠 것인가? 두번쨰를 선택한다면 Test Driven하게 가고 있는 것일까? 고민을 하였다.  
이러한 고민들을 정리해서 각 단계별로 기준을 정해보아야겠다는 생각을 했다.  

	1. 테스트만 통과하기 위하여 작성된 코드를 리팩토링 하지는 말자. 차라리 건너뛰자.
	2. 현재 테스트 단계에서 규칙이 보이지 않는 다면 다음 테스트를 진행해보자.  


그래서 다음 테스트를 진행하였다.  
test/test.js

	...
	findPrime(6).should.deep.equal([ 2, 3 ])
	...

통과시킨다.   

src/utils/findPrime.js

	...
	if (remain === 6) {
	  remain /= 3
	  result.push(remain)
	  remain++
	}
	...

이쯤 오니까 생각이 명확해졌다. 리팩토링을 하지 않고 지나오니, 내가 임의로 짠 로직이 진화 중이다. 하지만, 어느정도 규칙이 보이기때문에, 리팩토링이 가능할 것 같다.  

src/utils/findPrime.js

	export default function findPrime(value) {
	  const result = []
	  let remain = value
	  let prime = 2
	  
	  while(remain !== 1) {
	    if (remain%prime === 0) {
	      result.push(prime)
	      remain /= prime
	    } else {
	      prime++
	    }
	  }
	  
	  return result
	}

더이상 남는 수를 가지고 해서는 진행이 안될 것 같고, return되어야하는 것은 소인수 이므로, 소인수 변수를 만들고, 나누어 떨어지지 않을 때까지 나눈 후 다음 소인수로 시도한다. 소인수를 구하는 로직이 모두 들어있으므로 모든 테스트케이스에 통과할 것으로 예상이 된다.  

멘토님은 좀 더 리팩토링을 깔끔하게 하셨는데, 다음 소인수를 시도하기 전에 미리 모두 나누는 로직을 짜셨다. 그 방법이 직관적인 것 같다.

src/utils/findPrime.js ( 멘토님 버전 )

	...
	while(remain !== 1) {
	  while (remain%prime === 0) {
	    result.push(prime)
	    remain /= prime
	  }
	  prime++
	}
	...

 나는 왜 최종 결과물이 이렇게 안나왔을까? 중간에 짠 의미없는 로직을 리팩토링 하지 않아서 그런걸까? 아니면 내가 최종결과물을 리팩토링할 때 잘 못 짠 탓일까? 어떤 부분을 타겟팅하여 의도적 수련을 해야할 지 잘 감이 안 온다.  
 
 멘토님이 직접 시연해 주신것을 혼자 하는 것도 시간이 오래걸렸는데, 매우 재미있는 경험이다. 정렬을 TDD로 짜면 퀵 소트가 나온다는데, 좀 더 연습해서 그것도 해보아야겠다. TDD에서 단계별로 기준을 세우는 것도 연습을 하면서 정립해보아야겠다.
 