# Typescript

> 타입스크립트에 대해서 공부를 하면서 정리를 하려고 합니다.

타입스크립트는 자바스크립트의 superset이라고 생각하면 됩니다. 타입스크립트는 자바스크립트 위에 있으며 작성되는 모든 코드는 자바스크립트로 변환됩니다.



## 왜 사용할까?

> 타입스크립트에는 자바스크립트에는 없는 규칙들이 있습니다.

Javascript는 엄격한 규칙이 없고, 사용하기 쉽고(?), 개발자가 원하는 방향으로 수정하기가 쉽기 때문에 많이 사용되어 왔습니다.

하지만 이러한 점들이 규모가 큰 프로젝트나 협업을 하게 될 경우 장점이 단점이 될 수 있습니다. 

그렇기 때문에 타입스크립트를 사용할 수 있습니다. 예측이 가능하고 읽기가 쉽고 엄격하기 때문입니다.



## Setup

> 타입스크립트를 사용하기 위해 먼저 해야할 일들이 몇 가지 있습니다.

차근 차근 하나씩 알아보겠습니다.



#### step 1.

프로젝트를 만들었다면 `yarn init` 명령어를 통해 package.json을 만들어 줍니다. 그리고  `yarn add typescript` 명령어를 통해 타입스크립트를 설치해 줍니다. 만약에 타입스크립트가 모두 필요하다면 `yarn global add typescript`를 사용하면 됩니다. 



#### step 2.

tsconfig.json 파일을 생성해 줍니다. 이 파일에서는 타입스크립트에게 어떻게 js로 변환하는지 알려주고 몇몇 옵션을 주게 됩니다.

```js
// tsconfig.json
{
    "compilerOPtions": {
        // node.js를 평범하게 사용하고 다양한 것들을 import, export할 수 있도록
        "module": "commonjs",
        // 어떤 버전의 js로 컴파일 되고 싶은지
        "target": "ES2015",
        // sourcemap 처리를 하고 싶은지
        "sourceMap": true
    },
    // 어떤 파일들이 컴파일 과정에 포함되는지 알려주며, 컴파일 과정에서 포함할 파일의 배열을 적으면 됨.
    "include": ["index.ts"]
}
```

 그리고 index.ts라는 파일을 생성하여 다음 코드를 작성하겠습니다. 

```js
// index.ts
alert("hello");
```



#### step 3.

이후에 할 일은 index.ts 파일을 index.js로 컴파일 하는 과정이 필요합니다. 터미널에서 `tsc` 명령어를 통해 index.ts 파일에 있는 코드를 컴파일해서 index.js와 index.js.map을 만들어 줍니다. 하지만 tsc를 매번 사용하면 번거롭기 때문에 yarn start를 사용하면 바로 타입스크립트가 컴파일 되서 실행이 되도록 만들어 주도록 합시다. 

```js
// package.json

{
    ...
    // 추가로 적어줄 것들
    "script": {
    	"start": "node index.js",
    	"prestart": "tsc"
}
}
```

이렇게 하고 나서 `yarn start` 명령어를 사용하면 node에서 index.js가 실행됩니다. 하지만 그 전에 ts 파일을 컴파일 해줘야 합니다. prestart에 tsc를 넣어줌으로서 yarn은 prestart를 먼저 실행하고 start를 실행하게 됩니다. 위와 같이 하는 이유는 Node.js는 타입스크립트를 모르기 떄문에 js로 컴파일해주는 작업이 필요하기 때문입니다.



## Typescript 맛보기

> 타입 스크립트는 type이라는 말이 있듯이 어떤 종류의 변수와 데이터인지 설정을 코드를 통해 해줘야 합니다. Swift와 비슷하게 사용할 거 같은데 한 번 알아보겠습니다.

간단한 코드를 작성해보겠습니다.

```typescript
// index.ts
const name = "Yongjai";
const age = 25;
const gender = "male";

const myInfo = (name, age, gender) => {
    console.log("Hi!, My name is ${name}. I'm ${age} years old and ${gender}.")
}

myInfo(name, age);

// 타입스크립트 규칙으로 이 파일이 모듈된다는 것을 알려준다는 걸 명시.
export {};
```

위 코드가 그냥  Javascript였다면 컴파일 에러 없이 그냥 동작하겠지만 타입스크립트이기 때문에 에러가 발생합니다.



이유는 myInfo에서 3개의 파라미터를 넘겨주기로 햇는데 두 개만 넘겨줬기 때문입니다. 

```typescript
// ?를 이용해서 해결을 할 수도 있습니다.
const myInfo = (name, age, gender?) => {
    console.log("Hi!, My name is ${name}. I'm ${age} years old and ${gender}.")
}

/*
gender 뒤에 ?를 붙여주는 방법입니다. swift에서는 옵셔널이라고 불리는데 타입스크립트에서 정확한 명칭은 모르겠습니다. 아마 사용하는 방식은 비슷할 것이라 생각이 됩니다. !도 있지 않을까요?
언젠가 자세하게 다룰 일이 있을거라 생각이 되어 이런 것이 있다라는 정도로만 하고 넘어갈까 합니다.
*/
```



위 코드를 타입스크립트에 어울리는 코드로 좀 바꿔보겠습니다.

```typescript
const myInfo = (name: string, age: number, gender: string): void => {
    console.log("Hi!, My name is ${name}. I'm ${age} years old and ${gender}.");
}
myInfo("Yongjai", "25", "male");

const myInfo1 = (name: string, age: number, gender: string): void => {
    return "Hi!, My name is ${name}. I'm ${age} years old and ${gender}.";
}
myInfo1("Yongjai", 25, "male");

export {};
```

이 코드를 동작시키면 어떻게 될까요?

에러가 발생합니다.

```typescript
myInfo("Yongjai", "25", "male");
```

위 코드는 age를 number로 받을 것을 명시했지만 스트링으로 받고 있기 때문에 에러가 발생합니다.



```typescript
myInfo1("Yongjai", "25", "male");
```

위 코드는 myInfo1에서 void를 이용해 리턴값이 없게 설정했는데 리턴을 하고 있기 때문에 에러가 발생합니다. 



```typescript
const myInfo = (name: string, age: number, gender: string): void => {
    console.log("Hi!, My name is ${name}. I'm ${age} years old and ${gender}.");
}
myInfo("Yongjai", 25, "male");

const myInfo1 = (name: string, age: number, gender: string): string => {
    return "Hi!, My name is ${name}. I'm ${age} years old and ${gender}.";
}
myInfo1("Yongjai", 25, "male");

export {};
```

위와 같이 수정하게 되면 코드가 정상적으로 동작하게 됩니다.