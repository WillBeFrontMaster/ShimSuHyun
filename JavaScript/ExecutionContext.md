
# 실행 컨텍스트 (Execution Context)
ECMAScript 에 따르면 실행 컨텍스트를 <b>실행 가능한 코드를 형상화하고 구분하는 추상적인 개념</b>이라고 정의한다.
<br/>
=> 실행 컨텍스트는 실행 가능한 코드가 실행되기 위한 환경이라고 말할 수 있다.<br/>
<b>실행 가능한 코드</b>란,   
- 전역 코드 : 전역 영역에 존재하는 코드
- Eval 코드 : eval 함수로 실행되는 코드
- 함수 코드 : 함수 내에 존재하는 코드
<br/>

일반적으로 실행 가능한 코드는 전역 코드와 함수 코드이다.<br/>
<br/>
자바스크립트 엔진은 코드를 실행하기 위하여 실행에 필요한 여러가지 정보를 알고 있어야 한다.<br/>
실행에 필요한 여러가지 정보란,
- 변수 : 전역변수, 지역변수, 매개변수, 객체의 프로퍼티
- 함수 선언
- 변수의 유효범위(Scope)
- this   

이와 같이 실행에 필요한 정보를 형상화하고 구분하기 위해 자바스크립트 엔진은 실행 컨텍스트를 물리적 형태의 객체로 관리한다.<br/>
```js
	var x = 'xxx';
	
    function foo(){
    	var y = 'yyy';
        
        function bar(){
        	var z = 'zzz';
            console.log(x + y + z);
        }
        bar();
    }
    foo();
```
위 코드를 실행하면 아래와 같이 실행 컨텍스트 스택이 생성하고 소멸한다. 현재 실행중인 컨텍스트에서 이 컨텍스트와 관련 없는 코드(ex. 다른 함수)가 실행되면 새로운 컨텍스트가 생성된다. 이 컨텍스트는 스택에 쌓이게 되고, 컨트롤(제어권)이 이동한다.<br/>
<center><img src="https://poiemaweb.com/img/ec_1.png" width="60%"/></center>
1. 컨트롤이 실행 가능한 코드로 이동하면 논리적 스택 구조를 가지는 새로운 실행 컨텍스트 스택이 생성된다.<br/>
2. 전역 코드(Global code)로 컨트롤이 진입하면 전역 실행 컨텍스트가 생성되고 새로운 실행 컨텍스트 스택에 쌓인다. 전역 실행 컨텍스트는 애플리케이션이 종료될 때까지 유지된다.<br/>
3. 함수를 호출하면 해당 함수의 실행 컨텍스트가 생성되며 직전에 실행된 코드 블록의 실행 컨텍스트 위에 쌓인다.<br/>
4. 함수 실행이 끝나면 해당 함수의 실행 컨텍스트를 파기하고 직전의 실행 컨텍스트에 컨트롤을 반환한다.<br/>
<br/>

### 실행 컨텍스트의 3가지 객체<br/>
실행 컨텍스트는 실행 가능한 코드를 형상화하고 구분하는 추상적인 개념이지만 물리적으로는 객체의 형태를 가지며 아래의 3가지 프로퍼티를 소유한다.<br/>
#### 1. Variable Object (VO, 변수객체)<br/>
실행 컨텍스트가 생성되면 자바스크립트 엔진은 실행에 필요한 여러 정보들을 담을 객체를 생성한다. 이를 <b>Variable Object (VO, 변수객체)</b>라고 한다. Variable Object는 코드가 실행될 때 엔진에 의해 참조되며 코드에서는 접근할 수 없다.<br/>
Variable Object는 아래의 정보들을 담는 객체이다.
- 변수
- 매개변수와 인자
- 함수 선언(함수 표현식 제외)

Variable Object 는 실행 컨텍스트의 프로퍼티이기 때문에 값을 갖는데, 이 값은 다른 객체를 가리킨다.전역 코드 실행시 생성되는 전역 컨텍스트와 함수 실행시 생성되는 함수 컨텍스트는 가리키는 객체가 다르다. 이는 전역 코드와 함수의 내용이 다르기 때문이다. 예를 들면 함수는 매개변수를 갖지만 전역 코드는 매개변수가 없다.<br/>
<br/>
Variable Object 가 가리키는 객체는 아래와 같다.<br/>
- 전역 컨텍스트의 경우
  * Variable Object는 유일하며 최상위에 위치하고 모든 전역 변수, 전역 함수 등을 포함하는 <b>전역 객체 (Global Object, GO)</b>를 가리킨다. 전역 객체는 전역에 선언된 전역 변수와 전역 함수를 프로퍼티로 소유한다.<br/><br/>
  <img src="https://poiemaweb.com/img/ec-vo-global.png" width="30%"/>
- 함수 컨텍스트의 경우
	* Variable Object는 <b>활성객체(Activation Object, AO)</b>를 가리키며 매개변수와 인수들의 정보를 배열의 형태로 담고 있는 객체인 arguments object가 추가된다.<br/><br/>
	<img src="https://poiemaweb.com/img/ec-vo-foo.png" width="30%"/>
>이해가... 안된다................
#### 2. Scope Chain (SC)
스코프 체인(Scope Chain)은 일종의 리스트로서 전역 객체와 중첩된 함수의 스코프의 레퍼런스를 차례로 저장하고 있다. 스코프 체인은 해당 전역 또는 함수가 참조할 수 있는 변수, 함수 선언 등의 정보를 담고 있는 전역 객체(GO) 또는 활성 객체(AO)리스트를 가리킨다.<br/>
현재 실행 컨텍스트의 활성 객체(AO)를 선두로 하여 순차적으로 상위 컨텍스트의 활성 객체(AO)를 가리키며 마지막 리스트틑 전역 객체(GO)를 가리킨다.<br/>

<p align="center"><img src="https://poiemaweb.com/img/ec-sc.png" width="45%"/></p>

<br/>
>스코프 체인은 식별자 중에서 객체(전역 객체 제외)의 프로퍼티가 아닌 식별자, 즉 <b>변수를 검색하는 메커니즘</b>이다.<br/>
>>식별자 중에서 변수가 아닌 객체의 프로퍼티(메소드 포함)를 검색하는 메커니즘은 프로토타입 체인(Prototype Chain)이다.


<br/>
엔진은 스코프 체인을 통해 렉시컬 스코프(함수를 어디서 선언했는지)를 파악한다. 함수가 중첩 상태일 때 하위함수 내에서 상위함수의 스코프와 전역 스코프까지 참조할 수 있는데 이것은 스코프 체인을 검색을 통해 가능하다. 함수가 중첩되어 있으면 중첩될 때마다 부모 함수의 Scope가 자식 함수의 Scope Chain에 포함된다. 함수 실행중에 변수를 만나면 그 변수를 먼저 현재 Scope, 즉 Activation Object에서 검색해보고, 만약 검색에 실패하면 스코프 체인에 담겨진 순서대로 그 검색을 이어가게 되는 것이다.<br/>
이것이 스코프 체인이라고 불리는 이유이다.<br/>
<br/>
예를 들어 함수 내의 코드에서 변수를 참조하면 엔진은 스코프 체인의 첫번째 리스트가 가리키는 활성 객체에 접근하여 변수를 검색한다. 만약 검색에 실패한다면 다음 리스트가 가리키는 활성 객체(또는 전역 객체)를 검색한다. 이와 같이 순차적으로 Scope Chain에서 변수를 검색하는데, 결국 검색에 실패하면 정의되지 않은 변수에 접근하는 것으로 판단하여 Reference Error를 발생시킨다. 스코프 체인은 함수의 프로퍼티인 `[[Scope]]`로 참조할 수 있다.<br/>
<br/>

#### 3. this value
this 프로퍼티에는 this 값이 할당된다. this에 할당되는 값은 함수 호출 패턴에 의해 결정된다.<br/>
<br/>

### 실행 컨텍스트의 생성 과정
```js
	var x = 'xxx';
    
    function foo(){
    	var y = 'yyy';
        
        function bar(){
        	var z = 'zzz';
            console.log(x + y + z);
        }
        bar();
     }
     
     foo();
```
#### 1. 전역 코드로의 진입
컨트롤이 실행 컨텍스트에 진입하기 이전에 유일한 전역 객체(GO)가 생성된다. 전역 객체는 단일 사본으로 존재하며 이 객체의 프로퍼티 코드의 어떠한 곳에서도 접근할 수 있다. 초기 상태의 전역 객체에는 빌트인 객체(Math, Atring, Array 등)와 BOM, DOM이 설정되어 있다.<br/>
<p align="center"><img src="https://poiemaweb.com/img/ec_3.png" width="50%"/></p>
전역 객체가 생성된 이후, 전역 코드로 컨트롤이 진입하면 전역 실행 컨텍스트가 생성되고 실행 컨텍스트 스택에 쌓인다.<br/>
<p align="center"><img src="https://poiemaweb.com/img/ec_4.png" width="50%"/></p>
그리고 이후 이 실행 컨텍스트를 바탕으로 이하의 처리가 실행된다.<br/>

>1. 스코프 체인의 생성과 초기화
>2. Variable Instantiation(변수 객체화) 실행
>3. this value 결정

#### 1-1. 스코프 체인의 생성과 초기화
실행 컨텍스트가 생성된 이후 가장 먼저 스코프 체인의 생성과 초기화가 실행된다. 이 때 스코프 체인은 전역 객체의 레퍼런스를 포함하는 리스트가 된다.<br/>
<p align="center"><img src="https://poiemaweb.com/img/ec_5.png" width="50%"></p>

#### 1-2. Variable Instantiation(변수 객체화) 실행
스코프 체인의 생성과 초기화가 종료되면 변수 객체화가 실행된다.<br/>
변수 객체화는 변수 객체에 프로퍼티와 값을 추가하는 것을 의미한다. 변수, 매개변수와 인수 정보(arguments), 함수 선언을 변수 객체에 추가화여 객체화하기 때문에 <b>변수 객체화</b>라고도 한다.<br/>
전역 코드의 경우, 변수 객체는 전역 객체를 가리킨다.

<p align="center"><img src="https://poiemaweb.com/img/ec_6.png" width="50%"></p>
<br/>
변수 객체화는 아래의 순서로 변수 객체에 프로퍼티와 값을 set 한다.<br/><br/>

>1. (함수 코드인 경우) <b>매개변수(parameter)</b>가 변수 객체의 프로퍼티로, 인수(argument)가 값으로 설정된다.<br/>
>2. 대상 코드 내의 <b>함수</b> 선언(함수 표현식 제외)을 대상으로 함수명이 변수 객체의 프로퍼티로, 생성된 함수 객체가 값으로 설정된다. <b>(변수 호이스팅)</b><br/>
>3. 대상 코드 내의 <b>변수</b> 선언을 대상으로 변수명이 변수 객체의 프로퍼티로, undefined가 값으로 설정된다. <b>(변수 호이스팅)</b><br/>


#### 1-3. this value 결정

변수 선언 처리가 끝나면 다음은 this value가 결정된다. <b>this value가 결정되기 이전에 this는 전역 객체를 가리키고 있다가 함수 호출 패턴에 의해 this에 할당되는 값이 결정된다.</b> 전역 코드의 경우, this는 전역 객체를 가리킨다.
<p align="center"><img src="https://poiemaweb.com/img/ec_9.png" width="50%"></p>
<b>전역 컨텍스트(전역 코드)의 경우 변수 객체, 스코프 체인, this 값은 언제나 전역 객체이다.</b>

#### 2. 전역 코드의 실행

```js
	var x = 'xxx';
    
    function foo(){
    	var y = 'yyy';
        
        function bar(){
        	var z = 'zzz';
        	console.log(x + y + z);
        }
        bar();
    }
    
    foo();
```

전역 변수 x에 문자열 'xxx' 할당과 함수 foo의 호출이 실행된다.<br/>
<br/>
#### 2-1. 변수 값의 할당
전역변수 x에 문자열 'xxx'를 할당할 때, 현재 실행 컨텍스트의 스코프 체인이 참조하고 있는 변수 객채를 선두(0)부터 검색하여 변수명에 해당하는 해당하는 프로퍼티가 발견되면 값('xxx')를 할당한다.
<p align="center"><img src="https://poiemaweb.com/img/ec_10.png" width="50%"></p>

#### 2-2. 함수 foo의 실행
전역 코드의 함수 foo가 실행되기 시작하면 새로운 함수 실행 컨텍스트가 생성된다. 함수 foo의 실행 컨텍스트로 컨트롤이 이동하면 전역 코드의 경우와 마찬가지로<b> 1. 스코프 체인의 생성과 초기화, 2. Variable Instantiation 실행, 3. this value 결정</b>이 순차적으로 실행된다.<br/>
단, 이번 실행되는 코드는 함수 코드이기 때문에 <b> 1. 스코프 체인의 생성과 초기화, 2. Variable Instantiation 실행, 3. this value 결정</b>은 전역 코드의 룰이 아닌 함수 코드의 룰이 적용된다.
<br/>
<p align="center"><img src="https://poiemaweb.com/img/ec_11.png" width="50%"></p>

- 스코프 체인의 생성과 초기화 :
함수 코드의 스코프 체인의 생성과 초기화는 우선 Activation Object에 대한 레퍼런스를 스코프 체인의 선두에 설정하는 것으로 시작된다.<br/>
Activation Object는 우선 <b>arguments 프로퍼티의 초기화</b>를 실행하고 그 후, Variable Instantiation가 실행된다. Activation Object는 스펙 상의 개념으로 프로그램이 Activation Object에 직접 접근할 수 없다.(Activation Object의 프로퍼티로의 접근은 가능하다).
<p align="center"><img src="https://poiemaweb.com/img/ec_12.png" width="50%"></p>
그 후, Caller(전역 컨텍스트)의 Scope Chain이 참조하고 있는 객체가 스코프 체인에 push된다. 따라서, 이 경우 함수 foo를 실행한 직후 실행 컨텍스트의 스코프 체인은 Activation Object(함수 foo의 실행으로 만들어진 AO-1)과 전역 객체를 순차적으로 참조하게 된다.<br/>
<p align="center"><img src="https://poiemaweb.com/img/ec_13.png" width="50%"></p>

- Variable Instantiation 실행 :
Function code의 경우, <b>스코프 체인의 생성과 초기화</b>에서 생성된 Activation Object를 Variable Object로서 Variable Instantiation가 실행된다. 이것을 제외하면 전역 코드의 경우와 같은 처리가 실행된다. 즉, 함수 객체를 Variable Object(AO-1)에 바인딩한다. (프로퍼티는 bar, 값은 새로 생성된 Function Object) (bar function object의 [[Scope]] 프로퍼티 값은 AO-1과 Global Object를 참조하는 리스트)<br/>
<p align="center"><img src="https://poiemaweb.com/img/ec_14.png" width="50%"></p>
변수 y를 Variable Object(AO-1)에 설정한다. 이 때 프로퍼티는 y, 값은 undefined이다.
<p align="center"><img src="https://poiemaweb.com/img/ec_15.png" width="50%"></p>
<br/>

-  this value 결정
변수 선언 처리가 끝나면 다음은 this value가 결정된다. this에 할당되는 값은 함수 호출 패턴에 의해 결정된다.<br/>
내부 함수의 경우, this의 value는 전역 객체이다.
<p align="center"><img src="https://poiemaweb.com/img/ec_16.png" width="50%"></p>



### 3. foo 함수 코드의 실행
이제 함수 foo의 코드 블록 내 구문이 실행된다. (변수 y에 문자열 'yyy'의 할당과 함수 bar가 실행된다.)<br/>
#### 3-1. 변수 값의 할당
지역 변수 y에 문자열 'yyy'를 할당할 때, 현재 실행 컨텍스트의 스코프 체인이 참조하고 있는 Variable Object를 선두(0)부터 검색하여 변수명이 해당하는 프로퍼티가 발견되면 값 'yyy' 를 할당한다.<br/>
<p align="center"><img src="https://poiemaweb.com/img/ec_17.png" width="50%"></p>

#### 3-2. 함수 bar의 실행
함수 bar가 실행되기 시작하면 새로운 실행 컨텍스트가 생성된다.
<p align="center"><img src="https://poiemaweb.com/img/ec_18.png" width="50%"></p>
이전 함수 foo의 실행과정과 동일하게 <b>1. 스코프 체인의 생성과 초기화, 2. Variable Instantiation 실행, 3. this value 결정</b>이 순차적으로 실행된다.
<p align="center"><img src="https://poiemaweb.com/img/ec_19.png" width="50%"></p>
이 단계에서 `console.log(x+y+z);` 구문의 실행 결과는 <b>xxxyyyzzz</b>가 된다.<br/>
- x : AO-2에서 x 검색 실패 -> AO-1에서 x 검색 실패 -> GO에서 x 검색 성공 (value : 'xxx')<br/>
- y : AO-2에서 y 검색 실패 -> AO-1에서 y 검색 성공 (value : 'yyy')<br/>
- z : AO-2에서 z 검색 성공 (value : 'zzz')




























