# ES6+ 문법정리


## Variable

es5까지 변수를 담는 키워드는 var뿐이였지만 호이스팅과 스코프관련하여 문제점이 많아 es6에 let과 const가 추가되었다.<br/>
각각의 키워드의 차이점은 다음과 같다.

### 1.var
* 함수 레벨 스코프: var로 선언된 변수는 함수 레벨 스코프를 가지며, 함수 외부에서 선언된 경우 전역 변수가 된다.<br/>
* 호이스팅: 변수가 선언된 위치와 상관 없이 참조,호출을 해도 참조에러가 발생하지 않는다.(선언,할당 동시화)<br/>
* 재선언 가능: 동일한 스코프 내에서 var로 선언된 변수를 재선언할 수 있습니다.<br/>

```javascript
function example() {
  console.log(a); // undefined
  var a = 10;
  console.log(a); // 10
}

function varExample() {
  var funcs = [];
  for (var i = 0; i < 3; i++) {
    funcs.push(function() {
      console.log(i);
    });
  }
  return funcs;
}

var resVar = varExample();
resVar[0](); // 출력: 3
resVar[1](); // 출력: 3
resVar[2](); // 출력: 3
```

### 2.let
* 블록 레벨 스코프: let으로 선언된 변수는 블록 레벨 스코프를 가집니다. 즉, 중괄호 {} 안에서만 접근이 가능하다.<br/>
* 호이스팅에 대한 보호: let으로 선언된 변수도 호이스팅이 발생하지만, 변수 선언 전에 접근하려고 하면 ReferenceError가 발생한다.<br/>
* 재선언 불가능: 동일한 스코프 내에서 let으로 선언된 변수를 재선언할 수 없다.<br/>

```javascript
let 나이;
let 나이;//에러

let variable = 'a'; 
console.log(variable); //a
variable = 'b'; 
console.log(variable); //b

if ( 1 == 1 ){
    let 이름 = 'Kim';
    console.log(이름);
  }
console.log(이름); // ReferenceError

function letExample() {
  var funcs = [];
  for (let i = 0; i < 3; i++) {
    funcs.push(function() {
      console.log(i);
    });
  }
  return funcs;
}

var resLet = letExample();
resLet[0](); // 출력: 0
resLet[1](); // 출력: 1
resLet[2](); // 출력: 2
```

### 3.const
* 블록 레벨 스코프: const도 let과 마찬가지로 블록 레벨 스코프를 가진다.<br/>
* 재할당 불가능: const로 선언된 변수는 한 번 할당되면 재할당할 수 없다. 단, 객체 또는 배열과 같은 참조 타입 변수의 경우, 내부 속성은 변경할 수 있다.<br/>
* 재선언 불가능: let과 마찬가지로, 동일한 스코프 내에서 const로 선언된 변수를 재선언할 수 없다.<br/>
* 초기화 필수: const로 변수를 선언할 때 반드시 초기값을 지정해야 한다.<br/>

```javascript
const c = 30;
c = 40; // TypeError: Assignment to constant variable.

{
  const d = { value: 50 };
  d.value = 60; // 가능
  console.log(d.value); // 60
}

function constExample() {
  var funcs = [];
  for (const i = 0; i < 3; i++) {
    funcs.push(function() {
      console.log(i);
    });
  }
  return funcs;
}

var resConst = constExample();
resConst[0](); // 출력: 0
resConst[1](); // 출력: 1
resConst[2](); // 출력: 2
```

## ArrowFunction
기존 함수(함수 선언, 함수 표현)과 화살표 함수(arrow function)의 주요 차이점은 this의 바인딩 방식, arguments 객체의 유무, 그리고 문법적인 간결함이다.

### this 바인딩
```javascript
//일반함수
function NormalFunction() {
  this.value = 42;
  this.getValue = function() {
    return this.value;
  };
}

const normalInstance = new NormalFunction();
console.log(normalInstance.getValue()); // 출력: 42

const getValue = normalInstance.getValue;
console.log(getValue()); // 출력: undefined (or Error in strict mode)
```
```javascript
//화살표함수
function ArrowFunction() {
  this.value = 42;
  this.getValue = () => {
    return this.value;
  };
}

const arrowInstance = new ArrowFunction();
console.log(arrowInstance.getValue()); // 출력: 42

const getArrowValue = arrowInstance.getValue;
console.log(getArrowValue()); // 출력: 42
```

### arguments 객체 유무
```javascript
//일반함수
function normalFunction() {
  console.log(arguments);
}

normalFunction(1, 2, 3); // 출력: Arguments(3) [1, 2, 3, callee: (...), Symbol(Symbol.iterator): ƒ]

```
```javascript
//화살표함수
const arrowFunction = () => {
  console.log(arguments);
};

arrowFunction(1, 2, 3); // 출력: Error - arguments is not defined

```

#### arguments 
arguments 객체는 함수 내부에서 접근 가능한 지역 변수로, 해당 함수에 전달된 모든 인수를 담고 있는 유사 배열 객체이다.

### 문법적 간결함
```javascript
//일반함수
function normalFunction(a, b) {
  return a + b;
}
```
```javascript
//화살표함수
const arrowFunction = (a, b) => a + b;
```

### 중괄호와 return문 생략

#### 한 줄 표현식:
* 화살표 함수의 본문이 한 줄의 표현식으로 구성된 경우, 중괄호와 return문을 생략할 수 있다.
  
```javascript
// 중괄호와 return 문을 생략하고, 한 줄 표현식을 사용
const sum = (a, b) => a + b;
```
#### 단일 인수:
* 화살표 함수가 하나의 인수만 받는 경우, 괄호를 생략할 수 있다.

```javascript

const getObject = () =>{
 const fooName = "bar"
 return { foo: 'fooName }
}

// 객체 리터럴을 반환하는 화살표 함수: 소괄호 사용
const getObject = () => ({ foo: 'bar' });

```

## Template Literals

템플릿 리터럴은 ES6에서 도입된 새로운 문자열 표기법이다. 백틱(`)으로 문자열을 감싸고, 내부에 ${} 구문을 사용하여 변수나 표현식을 직접 삽입할 수 있다. 이렇게 함으로써 문자열 연결 작업이 훨씬 간편하고 가독성 높은 코드를 작성할 수 있다.
```javascript
  // ES5
function es5() {
	return '안녕' + name + '너의 나이는' + age + '살 이다!'; 
}

console.log(es5('영희', 22));// 출력 => 안녕 영희 너의 나이는 22살 이다!
}

```

```javascript
// ES6
const es6 = (name, age) => {
	return `안녕 ${name}, 너의 나이는 ${age}살 이다!`; 
};

console.log(es6('영희', 22));// 출력 => 안녕 영희, 너의 나이는 22살 이다!
```

## Tagged Template Literals

태그드 템플릿 리터럴은 템플릿 리터럴을 확장한 형태로, 템플릿 리터럴 앞에 함수 이름(태그)을 붙여 사용한다. 이 태그 함수는 템플릿 리터럴의 문자열과 표현식을 개별적인 인수로 받아 처리할 수 있어, 문자열 생성을 더욱 유연하고 강력하게 제어할 수 있다. 태그 함수를 이용해 문자열 생성 과정에 로직을 추가하거나 문자열 포맷팅 등 다양한 활용이 가능하다.
```javascript
function tagged(strings, ...values) {
  console.log(strings);
  console.log(values);
  return strings.reduce((result, string, i) => `${result}${string}${values[i] || ''}`, '');
}

const name = "Alice";
const age = 25;

const result = tagged`Hello, my name is ${name} and I am ${age} years old.`;
console.log(result); // 출력: Hello, my name is Alice and I am 25 years old.
```

## Spread Opertor

Spread Operator (...)는 배열이나 객체를 개별 요소로 확장하거나 분해할 수 있는 ES6의 기능이다. 이 연산자는 주로 배열의 복사, 병합, 함수 인자 전달 등에서 유용하게 사용된다. 간결하게 여러 값들을 그룹화하거나 펼칠 때 사용하며 코드의 가독성과 유지보수에 도움을 준다.
```javascript
// 배열
let array2= ['hello', 'world'];
console.log(array2); //['hello', 'world']
console.log(...array2); //hello world

// 변수
let letter= 'hello';
console.log(letter); //hello
console.log(...letter);//h e l l o 

// deep copy
var a = [1,2,3];
var b = [4,5];
var c = [...a, ...b];

c = [1,2,3,4,5]// (deep copy할떄 유용)

//등호로 복사를 하면 a와 b 변수는 [1,2,3]을 각각 따로 하나씩 가진게 아니라 값 공유가 일어난다
var a = [1,2,3];
var b = a;

// 객체 property copy
let o1 = { a : 1, b : 2};
let o2 = { a : 3, ...o1 };
console.log(o2);

// 이렇게 a라는 값이 중복이 발생하면  뒤에 오는 a로 적용
// 출력해보면 a : 1 이라는 자료가 담겨짐


// 함수 파라미터 넣을떄
function add(a,b,c){
   console.log(a + b + c)
}
let array= [10, 20, 30];
add(...array);  //요즘방식
add.apply(undefined, array); //옛날방식// add(array[0], array[1], array[2]); 
```

## Default parameters

Default parameters는 함수의 매개변수에 기본값을 할당하는 ES6의 기능이다. 이 기능을 사용하면 함수 호출 시 명시적인 인자가 전달되지 않아도, 정의된 기본값을 통해 오류 없이 함수가 실행될 수 있다.
```javascript
function showMessage( message, from='unkown' ){
  console.log(`${message} by ${from}`);
}
showMessage('hi')

function temp(){
  return 10 
}

function add(a, b = temp() ){
  console.log(a + b)
}
```

## Rest parameters

Rest parameters는 함수에 전달된 인자들을 배열로 모으는 구문이다. 이 기능은 함수가 가변 개수의 인자를 받을 수 있게 해준다. Rest parameters는 매개변수 명 앞에 '...'를 사용하여 표시된다.
장점으로는 파라미터가 몇개인지 함수안에서 신경쓰지 않아도 된다.
```javascript
function printAll( ...args ) {   //배열형태로 파라미터가 전달 -> ['seok' , 'woo' , 'jjang']
  for(let i =0; i<args.length; i++){
      console.log(arg[i]);
  }
  }
  printAll('seok','woo','jjang')
  // 출력:
  // seok
  // woo
  // jjang
  
  // args.forEach((arg) => console.log(arg)) 또는
  // for(const arg of args) {
  //    console.log(arg);
  // }
  // 표현도 가능하다 
  
  function fun2(a, b ,...parameter){
    console.log(parameter)
  }
  fun2(1,2,3,4,5,6,7); //[3,4,5,6,7]
  
  //function fun2(a, ...parameter, b){} //에러 :rest parameter는 항상 맨뒤에 써야됨
  //function fun2(a, ...parameter, ...parameter2){} //에러:2개 이상 사용할 수 없습니다. 
}
```

## Destructing

"Destructuring"은 ES6에서 도입된 문법으로, 배열이나 객체의 속성을 해체하여 그 값을 개별 변수에 할당할 수 있는 JavaScript 표현식이다. 이를 통해 코드를 더욱 간결하고 가독성 좋게 작성할 수 있다.
```javascript
function printAll( ...args ) {   //배열형태로 파라미터가 전달 -> ['seok' , 'woo' , 'jjang']
  for(let i =0; i<args.length; i++){
      console.log(arg[i]);
  }
  }
  printAll('seok','woo','jjang')
  // 출력:
  // seok
  // woo
  // jjang
  
  // args.forEach((arg) => console.log(arg)) 또는
  // for(const arg of args) {
  //    console.log(arg);
  // }
  // 표현도 가능하다 
  
  function fun2(a, b ,...parameter){
    console.log(parameter)
  }
  fun2(1,2,3,4,5,6,7); //[3,4,5,6,7]
  
  //function fun2(a, ...parameter, b){} //에러 :rest parameter는 항상 맨뒤에 써야됨
  //function fun2(a, ...parameter, ...parameter2){} //에러:2개 이상 사용할 수 없습니다. 
}
```

### 주의점
구조 분해 할당(destructuring assignment)을 사용할 때, 객체의 메서드에서 this가 참조하는 것이 바뀔 수 있다. 일반적으로, 객체의 메서드 내에서 this는 그 객체 자신을 참조한다. 그러나 구조 분해를 사용하여 메서드를 별도의 변수로 추출할 경우, this는 더 이상 원래의 객체를 참조하지 않게 된다. 이는 this의 값이 실행 컨텍스트에 의존하는 JavaScript의 특성 때문이다.

```javascript
const obj = {
  value: 42,
  method() {
    console.log(this.value);
  }
};

// 정상적인 객체 메서드 호출: this는 obj를 참조
obj.method(); // 출력: 42

// 구조 분해 후 메서드 호출: this는 undefined를 참조 (strict mode에서)
const { method } = obj;
method(); // 출력: TypeError: Cannot read properties of undefined (reading 'value')
```

### 해결책
구조 분해 할당의 this 참조의 문제점은 원래의 객체를 참조하도록 this를 바인딩하거나, 화살표 함수를 사용하는 방법으로 문제를 해결할 수 있습니다

#### Function.prototype.bind를 사용하여 this 바인딩

```javascript
const obj = {
  value: 42,
  method() {
    console.log(this.value);
  }
};

const { method } = obj;
const boundMethod = method.bind(obj);

boundMethod(); // 출력: 42
```

#### Function.prototype.bind를 사용하여 this 바인딩

```javascript
const obj = {
  value: 42,
  method() {
    console.log(this.value);
  }
};

const method = () => obj.method();

method(); // 출력: 42
```

## ES Modules
"Import"와 "Export"는 ES6(ES2015)에서 도입된 모듈 시스템이다, JavaScript 코드를 여러 파일로 분할하고 관리할 수 있게 해준다. 이 모듈 시스템은 CommonJS 모듈 시스템과 여러 차이점이 있는데
다음과 같이 정리 할 수 있다.

### 정적 vs. 동적 로딩:
* ES Modules (ESM): import와 export 문은 정적으로 분석된다. 즉, 모듈 의존성은 코드가 실행되기 전에 알려져 있어야 한다.
* CommonJS: require() 함수를 통해 동적으로 모듈을 로드할 수 있다. 이를 통해 런타임에 모듈 의존성을 변경할 수 있다.

```javascript
// ES Modules (정적 로딩)

// myfile.mjs
export const name = "ES Modules";

// main.mjs
import { name } from './myfile.mjs';
console.log(name); // "ES Modules"

// --------------------------------------------------------

// CommonJS (동적 로딩)
// myfile.js
exports.name = "CommonJS";

// main.js
const loadModule = () => {
  const { name } = require('./myfile.js');
  console.log(name); // "CommonJS"
};
loadModule();
```

### 네임스페이스:
* ES Modules: ESM은 명시적인 네임스페이스를 갖고 있으며, 이름이 있는(named) 및 기본(default) export를 지원합니다.
* CommonJS: 모든 exports는 단일 module.exports 객체에 추가됩니다.

```javascript
// ES Modules (명시적 네임스페이스)

// lib.mjs
export const foo = "foo";
export const bar = "bar";

// main.mjs
import * as lib from './lib.mjs';
console.log(lib.foo, lib.bar); // "foo bar"

// --------------------------------------------------------

// CommonJS (단일 module.exports 객체)
// lib.js
exports.foo = "foo";
exports.bar = "bar";

// main.js
const lib = require('./lib.js');
console.log(lib.foo, lib.bar); // "foo bar"

```

### 실행 시점:
ES Modules: 모듈은 한 번만 실행되며, 결과는 캐시된다. 같은 모듈을 여러 번 import해도, 모듈 코드는 한 번만 실행된다.
CommonJS: require 함수를 통해 모듈을 여러 번 로드할 때마다 모듈 코드가 실행된다.

```javascript
// ES Modules (한 번만 실행)

// counter.mjs
let counter = 0;
counter++;
export default counter;

// main.mjs
import counter1 from './counter.mjs';
import counter2 from './counter.mjs';
console.log(counter1, counter2); // 1 1

// --------------------------------------------------------
// CommonJS (여러 번 로드할 때마다 실행)

// counter.js
let counter = 0;
counter++;
module.exports = counter;

// main.js
const counter1 = require('./counter.js');
const counter2 = require('./counter.js');
console.log(counter1, counter2); // 1 2


```
### 사용 문법:
* ES Modules: ESM은 명시적인 네임스페이스를 갖고 있으며, 이름이 있는(named) 및 기본(default) export를 지원한다.
* CommonJS: 모든 exports는 단일 module.exports 객체에 추가된다.

### 환경 지원:
* ES Modules: 최신 브라우저 및 Node.js(v13.2.0 이후)에서 지원된다.
* CommonJS: Node.js에서 널리 사용된다. 브라우저에서는 추가 도구 없이 직접 지원되지 않는다.

## Promise
Promise는 JavaScript에서 비동기 연산을 더 효율적이고 간결하게 처리할 수 있게 돕는 객체이다. Promise는 보통 네트워크 요청 같은 비동기 연산의 성공 또는 실패와 그 결과값을 나타낸다. Promise는 "pending(대기)", "fulfilled(이행)", "rejected(거부)" 중 하나의 상태를 가진다. Promise 체인을 통해 여러 비동기 연산을 순차적으로 연결할 수 있으며, 이를 통해 콜백 지옥을 피할 수 있다.

```javascript
//콜백으로 인한 비동기처리

function fetchData(callback) {
  setTimeout(() => {
    callback('Data fetched!');
  }, 1000);
}

fetchData((data) => {
  console.log(data); // 'Data fetched!'
});
```

```javascript
//프로미스로 인한 비동기처리

function fetchData() {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve('Data fetched!');
    }, 1000);
  });
}

fetchData().then((data) => {
  console.log(data); // 'Data fetched!'
});
```

### Promise.all:
여러 개의 Promise를 병렬로 실행하고, 모든 Promise가 성공적으로 이행되면 결과값들의 배열을 반환한다.

```javascript
Promise.all([promise1, promise2])
  .then((results) => {
    console.log(results); // [result1, result2]
  })
  .catch((error) => {
    console.error(error);
  });
```

### Promise.allSettled:
여러 개의 Promise를 병렬로 실행하고, 모든 Promise의 결과(성공 또는 실패)가 반환될 때까지 기다린다.

```javascript
Promise.allSettled([promise1, promise2])
  .then((results) => {
    console.log(results); // [{status: 'fulfilled', value: result1}, {status: 'rejected', reason: error2}]
  });
```

### Promise.race:
여러 개의 프로미스 중에서 가장 먼저 완료되는(즉, 이행되거나 거부되는) 프로미스의 결과값 또는 이유를 반환한다. 이 메서드는 주어진 프로미스들 중 하나라도 완료되면 즉시 완료된다.

```javascript
const promise1 = new Promise((resolve, reject) => {
    setTimeout(resolve, 500, 'First promise');
});

const promise2 = new Promise((resolve, reject) => {
    setTimeout(resolve, 1000, 'Second promise');
});

Promise.race([promise1, promise2])
    .then((value) => {
        console.log(value); // 'First promise'
    })
    .catch((error) => {
        console.error(error);
    });
```

### Promise.any:
주어진 Promise 반복 가능 객체 중에서 하나라도 이행(fulfill)되면 그 결과 값을 이행하는 새로운 Promise를 반환한다. 만약 모든 Promise 객체가 거부(reject)된다면, Promise.any() 는 AggregateError를 포함한 거부 상태의 Promise를 반환한다. 이 메서드는 여러 개의 비동기 작업 중 하나만 성공해도 되는 경우 유용하다.

```javascript
const promise1 = Promise.reject('Fail 1');
const promise2 = new Promise((resolve) => setTimeout(resolve, 100, 'Success'));
const promise3 = Promise.reject('Fail 3');

Promise.any([promise1, promise2, promise3])
  .then((value) => console.log(value))  // 출력: 'Success'
  .catch((error) => console.log(error));
```

#### 참고: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/all

## Async/Await
async/await는 JavaScript에서 비동기 코드를 작성하는 더 현대적이고 깔끔한 방법이다. async 함수는 항상 프로미스를 반환하며, await 키워드는 프로미스가 해결될 때까지 함수의 실행을 일시 중지한다. 
이를 통해 비동기 코드를 동기 코드처럼 보이게 하여 읽기 쉽고 관리하기 쉬운 코드를 작성할 수 있다. <br/>

다음은 콜백에서 시작하여 프로미스를 거쳐 async/await까지의 개선 흐름을 보여주는 코드 예제이다

```javascript
//콜백
function fetchData(callback) {
  setTimeout(() => {
    callback('Callback: Data fetched');
  }, 1000);
}

fetchData((result) => {
  console.log(result);
});
```
```javascript
//프로미스
function fetchData() {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve('Promise: Data fetched');
    }, 1000);
  });
}

fetchData().then((result) => {
  console.log(result);
}).catch((error) => {
  console.error(error);
});
```
```javascript
//async/await
async function fetchData() {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve('Async/Await: Data fetched');
    }, 1000);
  });
}

async function main() {
  try {
    const result = await fetchData();
    console.log(result);
  } catch (error) {
    console.error(error);
  }
}

main();
```

## s(dotAll) 플래그
s(dotAll) 플래그는 정규식에서 점(.) 메타문자가 개행 문자도 인식할 수 있게 해준다. 기존에는 점(.)이 개행 문자를 인식하지 못했기 때문에 여러 줄의 문자열을 매칭할 때 제약이 있었다.

```javascript
/foo.bar/.test('foo\nbar'); // false

/foo.bar/s.test('foo\nbar'); // true
```

## Array.prototype.methods

### Array.prototype.flat()  
Array.prototype.flat()는 배열의 깊이를 줄여준다. 지정된 깊이만큼 배열을 평평하게 만들 수 있다.

```javascript
// ES2019 이전의 고전 코드
var arr1 = [1, [2, [3, [4]], 5]];
var flatArr1 = arr1.reduce((acc, val) => acc.concat(val), []);
console.log(flatArr1); // [1, 2, [3, [4]], 5]

// ES2019에서 추가된 코드
var arr2 = [1, [2, [3, [4]], 5]];
console.log(arr2.flat(1)); // [1, 2, [3, [4]], 5]
```

### Array.prototype.flatMap()
Array.prototype.flatMap()은 각 배열 요소에 함수를 적용한 후, 결과를 새 배열로 평탄화한다.

```javascript
// ES2019 이전의 고전 코드
var arr3 = [1, 2, 3, 4];
var flatMapArr3 = arr3.map(x => [x * 2]).reduce((acc, val) => acc.concat(val), []);
console.log(flatMapArr3); // [2, 4, 6, 8]

// ES2019에서 추가된 코드
var arr4 = [1, 2, 3, 4];
console.log(arr4.flatMap(x => [x * 2])); // [2, 4, 6, 8]
```

### Array.prototype.includes()
배열이 특정 요소를 포함하고 있는지 판별한다.

 ```javascript
console.log([1, 2, 3].includes(2)) // true
```

### Array.prototype.find()
배열에서 특정 조건을 만족하는 첫 번째 요소의 값을 반환한다. 만족하는 요소가 없으면 undefined를 반환한다.

 ```javascript
console.log([1, 2, 3].find(x => x > 1)) // 2
```

### Array.prototype.findIndex():
배열에서 특정 조건을 만족하는 첫 번째 요소의 인덱스를 반환한다. 만족하는 요소가 없으면 -1을 반환한다.

 ```javascript
console.log([1, 2, 3].findIndex(x => x > 1)) // 1
```

### Array.prototype.fill()
배열의 시작 인덱스부터 끝 인덱스까지 특정 값으로 채운다.

 ```javascript
const array1 = [1, 2, 3, 4];

// Fill with 0 from position 2 until position 4
console.log(array1.fill(0, 2, 4)); //  [1, 2, 0, 0]
```

### Array.prototype.at()
 배열에서 지정한 인덱스에 있는 요소를 반환한다. 이 메서드의 특징은 음수 인덱스를 받아 배열의 끝에서부터 역순으로 요소를 찾을 수 있다는 것이다. JavaScript에서 배열은 0부터 시작하는 인덱스를 사용하므로, 이 기능은 배열의 끝부터 쉽게 요소에 접근할 수 있는 방법을 제공한며 인덱스가 범위를 벗어날 경우 undefined를 반환한다.

 ```javascript
const arrays = ['a', 'b', 'c', 'd'];

// 양수 인덱스 사용
console.log(arrays.at(0)); // 'a'
console.log(arrays.at(1)); // 'b'

// 음수 인덱스 사용
console.log(arrays.at(-1)); // 'd'
console.log(arrays.at(-2)); // 'c'

// 범위를 벗어난 인덱스
console.log(arrays.at(4)); // undefined
console.log(arrays.at(-5)); // undefined
```

#### 참고: [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/all](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)

## String.prototype.methods

### String.prototype.replaceAll()
문자열 내에서 일치하는 모든 패턴 또는 문자열을 찾아서 새 문자열로 교체한다. 이 메서드는 기존의 String.prototype.replace() 메서드와 유사하나, replace() 메서드는 정규식을 사용하지 않으면 첫 번째 일치 항목만 교체하는 반면, replaceAll() 메서드는 문자열 내의 모든 일치 항목을 교체한다.

```javascript
const str = 'I like my dog, my dog is very cute.';
const newStr = str.replace('dog', 'cat');
console.log(newStr);
// 출력: "I like my cat, my dog is very cute."
```

```javascript
const str = 'I like my dog, my dog is very cute.';
const newStr = str.replaceAll('dog', 'cat');
console.log(newStr);
// 출력: "I like my cat, my cat is very cute."
```

### String.prototype.includes()
문자열이 특정 문자열을 포함하고 있는지 판별한다.

```javascript
console.log('Hello World'.includes('World')) // true
```

### String.prototype.repeat()
문자열을 지정된 횟수만큼 반복하여 새 문자열을 생성한다.

 ```javascript
console.log('ab'.repeat(3)) //'ababab'
```

### String.prototype.startsWith()
문자열이 특정 문자열로 시작하는지 판별한다.

 ```javascript
console.log('Hello'.startsWith('Hel')) // true
```

### String.prototype.endsWith()
문자열이 특정 문자열로 끝나는지 판별한다.

 ```javascript
console.log('Hello'.endsWith('lo')) // true
```

### String.prototype.padStart()
현재 문자열을 다른 문자열로 패딩하여 지정된 길이를 달성한다. 패딩은 현재 문자열의 시작(왼쪽)에 적용된다.

 ```javascript
console.log('5'.padStart(2, '0')); // '05'
```

### String.prototype.padEnd():
현재 문자열을 다른 문자열로 패딩하여 지정된 길이를 달성한다. 패딩은 현재 문자열의 끝(오른쪽)에 적용된다.

 ```javascript
console.log('5'.padEnd(2, '0')) // '50'
```

#### 참고: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String

## Symbol
Symbol은 ES6에서 도입된 새로운 원시 데이터 타입이다. 심볼은 고유하고 변경 불가능한 데이터 유형으로, 주로 객체 프로퍼티의 키로 사용되어 이름 충돌의 가능성을 없애준다.. 심볼은 Symbol() 함수를 호출하여 생성하며, 선택적으로 설명 문자열을 전달할 수 있다.심볼을 키로 사용하면 다른 어떤 키와도 충돌하지 않으므로 객체 프로퍼티 키로 사용할 때 이름 충돌의 우려가 없다. 또한, 심볼은 고유하므로 같은 설명을 가진 두 심볼이라도 서로 다르게 취급한다.

 ```javascript
const uniqueSymbol1 = Symbol('description1');
const uniqueSymbol2 = Symbol('description2');

let object = {
  [uniqueSymbol1]: "value1",
  [uniqueSymbol2]: "value2",
};

console.log(object[uniqueSymbol1]); // "value1"
console.log(object[uniqueSymbol2]); // "value2"

// 기존 객체 프로퍼티 키와 충돌이 발생하지 않아, 더욱 안전한 코드 작성이 가능하다.
```

## BigInt 
BigInt는 ES2020에서 도입된 자바스크립트의 내장 객체로, 아주 큰 정수를 나타내는 데 사용된다. 기존의 Number 타입이 표현할 수 있는 정수 범위는 -2^53에서 2^53 - 1로 제한되지만, BigInt를 사용하면 이 범위를 넘어 아무리 큰 정수라도 정확하게 표현할 수 있다.

BigInt를 생성하려면 정수 뒤에 n을 붙이거나, BigInt 함수를 사용하면 된다.

```javascript
const bigIntFromLiteral = 1234567890123456789012345678901234567890n;
const bigIntFromFunction = BigInt("1234567890123456789012345678901234567890");

console.log(bigIntFromLiteral); // 1234567890123456789012345678901234567890n
console.log(bigIntFromFunction); // 1234567890123456789012345678901234567890n
```

## Dynamic import
동적 import는 스크립트 실행 중에 필요에 따라 모듈을 로드할 수 있게 해주는 기능이다. 이는 모듈의 조건부 로딩이 가능해지며, 초기 로딩 시간을 줄일 수 있다. 동적 import는 프로미스를 반환하므로 then 또는 await 키워드를 사용해 처리할 수 있다.
해당 기능으로 프론트엔드에서는 Code Splitting 기능이나 Lazy Loading 같은 기능을 추가할 수 있게 되었다.

### 정적 import
이전에는 모듈을 로드할 때 정적 import를 사용했다. 모듈이 애플리케이션 시작 시에 무조건 로드되며, 로드되지 않아도 실행 중에 로드할 수 없다.

```javascript
import module from './path/to/module.js';

if (condition1 && condition2) {
  module.doSomething();
}
```

### 동적 import
요즘 코드에서는 import() 함수를 사용하여 모듈을 동적으로 로드할 수 있다. 이 방식은 필요할 때만 모듈을 로드할 수 있게 해주며, 더 효율적인 코드 분할과 레이지 로딩을 가능하게 한다.

```javascript
if (condition1 && condition2) {
  import('./path/to/module.js').then(module => {
    module.doSomething();
  });
}

// 또는 async 함수 내에서 await 사용
if (condition1 && condition2) {
  const module = await import('./path/to/module.js');
  module.doSomething();
}
```

## Optional Chaining
옵셔널 체이닝(?.)은 객체의 속성에 접근할 때, 해당 속성이나 중간 체인에 null이나 undefined가 있으면 오류를 발생시키지 않고 undefined를 반환해주는 연산자이다.

```javascript
//기존 코드
const adventurer = {
  name: 'Alice',
  cat: {
    name: 'Dinah'
  }
};

let catName;
if (adventurer.cat && adventurer.cat.name) {
  catName = adventurer.cat.name;
} else {
  catName = undefined;
}
console.log(catName);
// 'Dinah'

let dogName;
if (adventurer.dog && adventurer.dog.name) {
  dogName = adventurer.dog.name;
} else {
  dogName = undefined;
}
console.log(dogName);
// undefined

```

```javascript
//Optional Chaining
const adventurer = {
  name: 'Alice',
  cat: {
    name: 'Dinah'
  }
};

let catName;
if (adventurer.cat && adventurer.cat.name) {
  catName = adventurer.cat.name;
} else {
  catName = undefined;
}
console.log(catName);
// 'Dinah'

let dogName;
if (adventurer.dog && adventurer.dog.name) {
  dogName = adventurer.dog.name;
} else {
  dogName = undefined;
}
console.log(dogName);
// undefined
```

##  Null coalescing operator(null 병합 연산자)
Null 병합 연산자(??)는 논리 연산자로, 왼쪽 피연산자가 null 또는 undefined인 경우 오른쪽 피연산자를 반환한다. 만약 왼쪽 피연산자가 null 또는 undefined가 아니라면, 왼쪽 피연산자를 그대로 반환한다. 이 연산자는 왼쪽 피연산자가 falsy한 값을 가지더라도 (예: 0, false, ''), 그 값을 유효한 값으로 인식하고 반환한다.

#### 기존 코드 (OR 연산자 사용)
기존에는 OR(||) 연산자를 사용하여 기본 값을 할당할 수 있었다. 하지만 OR 연산자는 왼쪽 피연산자가 모든 falsy한 값을 (예: 0, false, '') 만나면 오른쪽 피연산자를 반환하므로, falsy하지만 유효한 값들을 기본 값으로 설정할 수 없었다.
```javascript
//Optional Chaining
const foo = null || 'default string';
console.log(foo);
// expected output: "default string"

const baz = 0 || 42;
console.log(baz);
// expected output: 42
```

#### ES2020 코드 (Null 병합 연산자 사용)
Null 병합 연산자를 사용하면, 오직 null이나 undefined인 경우에만 오른쪽 피연산자를 반환하게 돤다. 이러한 특성 덕분에, falsy하지만 유효한 값들 (예: 0, false, '')을 기본 값으로 설정할 수 있게 되었다.
```javascript
const foo = null ?? 'default string';
console.log(foo);
// expected output: "default string"

const baz = 0 ?? 42;
console.log(baz);
// expected output: 0
```

## globalThis
globalThis는 ES2020에서 도입된 새로운 전역 객체로, 어느 환경에서든 일관된 방법으로 전역 객체에 접근할 수 있게 해준다. 이전에는 JavaScript가 실행되는 환경에 따라 전역 객체에 접근하는 변수가 달랐기 때문에, 여러 환경에서 작동하는 코드를 작성하는 것이 복잡다. globalThis의 도입으로 이러한 문제가 해결되었다.

```javascript
// 브라우저 환경에서
console.log(globalThis === window); // true

// Node.js 환경에서
console.log(globalThis === global); // true

// 웹 워커 환경에서
console.log(globalThis === self); // true

// globalThis를 사용하여 전역 변수 설정
globalThis.myGlobalVar = 42;
console.log(myGlobalVar); // 42

// globalThis를 사용하여 전역 변수 가져오기
console.log(globalThis.myGlobalVar); // 42
```

## 논리 연산자와 할당 표현식
??=, &&=, ||= 는 각각 nullish 병합 할당, 논리적 AND 할당, 논리적 OR 할당 연산자이다. 이 연산자들은 특정 조건이 충족될 때만 할당 연산을 수행한다.

### Nullish 병합 할당 (??=)
* a.speed ??= 25;
* 왼쪽 피연산자(a.speed)가 null 또는 undefined일 때만 오른쪽 피연산자(25)를 할당합니다.

```javascript
const a = { duration: 50, title: ''};

// a.duration은 이미 값이 있으므로 (50) 오른쪽 표현식은 무시
a.duration ??= 10;
console.log(a.duration);
// 출력: 50
```

### Nullish 병합 할당 (??=)
* a.speed ??= 25;
* 왼쪽 피연산자(a.speed)가 null 또는 undefined일 때만 오른쪽 피연산자(25)를 할당

```javascript
const a = { duration: 50, title: ''};

// a.duration은 이미 값이 있으므로 (50) 오른쪽 표현식은 무시
a.duration ??= 10;
console.log(a.duration);
// 출력: 50
```

### 논리적 AND 할당 (&&=)
* a.duration &&= 60;
* 왼쪽 피연산자(a.duration)가 참(Truthy)일 때만 오른쪽 피연산자(60)를 할당

```javascript
// a.duration은 참 값이므로, 오른쪽 표현식(60)이 할당
a.duration &&= 60;
console.log(a.duration);// 출력: 60
```

### 논리적 OR 할당 (||=)
* a.title ||= 'Logical or Assignment'
* 왼쪽 피연산자(a.title)가 거짓(Falsy)일 때만 오른쪽 피연산자('Logical or Assignment')를 할당

```javascript
const a = { duration: 50, title: ''};

// a.title은 빈 문자열('')이므로 거짓 값이므로, 오른쪽 표현식('Logical or Assignment')이 할당
a.title ||= 'Logical or Assignment';
console.log(a.title);
// 출력: 'Logical or Assignment'
```

## 숫자 구분 기호
숫자 구분 기호는 긴 숫자를 보다 명확하게 표현할 수 있게 돕는다. 이 기능은 큰 숫자를 표현할 때 자릿수를 쉽게 파악할 수 있게 해주며, 코드의 가독성을 향상시킨다. 언더바(_)를 사용하여 숫자 자릿수를 구분할 수 있다.

```javascript
const largeNumber = 10_000_000_000;
console.log(largeNumber);
// 출력: 10000000000

const price = 99_99.99;
console.log(price);
// 출력: 9999.99
```

## Shorthand property names
객체의 속성을 정의할 때, 속성 이름과 변수 이름이 같다면, 변수 이름만을 사용하여 객체 속성을 간단히 정의할 수 있는 ES6(ES2015)의 기능이다. 이 기능을 사용하면 코드가 더 간결하고 읽기 쉬워진다.

```javascript
const name = 'Ellie';
const age = '18';

//기존코드
const ellie = {
  name: name,
  age: age,
};

//Shorthand property names
const ellie2 = {
  name,
  age,
};
```

## Class Field Declarations
이전에는 클래스 필드를 생성자 내에서만 선언할 수 있었지만, 이제 클래스 본문에서 직접 필드를 선언할 수 있다. 이를 통해 코드가 더 깔끔해지고 가독성이 향상된다.

```javascript
class Hello {
  fields = 0;
  title;
}

const helloInstance = new Hello();
console.log(helloInstance.fields); // 0
console.log(helloInstance.title); // undefined
```

## Static 
static 키워드를 사용하여 클래스에 정적 메서드와 정적 필드를 추가할 수 있다. 이러한 정적 멤버들은 클래스 자체에 속하며 인스턴스에는 속하지 않는다.

```javascript
class Hello {
  name = 'world';
  static title = 'here';
  static getTitle() { return this.title; }
}

console.log(new Hello().name); // 'world'
console.log(new Hello().title); // undefined
console.log(Hello.title); // 'here'
console.log(Hello.getTitle()); // 'here'
```

## Private 키워드와 # 표기
클래스 내에서 # 표기법을 사용하여 private 필드와 메서드를 선언할 수 있다. 이러한 private 멤버들은 클래스 외부에서 접근할 수 없다.

```javascript
class ClassWithPrivateField {
  #privateField;

  constructor() {
    this.#privateField = 42;
  }

  getPrivateField() {
    return this.#privateField;
  }
}

const instance = new ClassWithPrivateField();
console.log(instance.privateField) // undefined
console.log(instance.getPrivateField()); // 42

```
