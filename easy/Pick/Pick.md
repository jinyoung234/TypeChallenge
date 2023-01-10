## 문제

---

`T`에서 `K` 프로퍼티만 선택해 새로운 오브젝트 타입을 만드는 내장 제네릭 `Pick<T, K>`을 이를 사용하지 않고 구현하세요.

### code

```ts
interface Todo {
  title: string;
  description: string;
  completed: boolean;
}

type TodoPreview = MyPick<Todo, "title" | "completed">;

const todo: TodoPreview = {
  title: "Clean room",
  completed: false,
};
```

## 정답 및 풀이 과정

---

### 답

```ts
type MyPick<T, K extends keyof T> = {
  [Key in K]: T[Key];
};
```

### 풀이 과정

> 1. `TodoPreview`가 `MyPick`과 일단 같아야 된다.

- todo에 TodoPreview 대신 MyPick<Todo, ‘title’ | ‘completed’>가 들어가도 적용이 되어야 한다.

> 2. Todo의 type은 `title, description, completed를 key로 하는 객체(interface)`이다.

> 3. MyPick이 통과 하려면 일단 MyPick은 `객체`가 되어야 한다.
>    `key는 Todo 타입인 K, value는 T[K]`

> 4. 내장 지네릭은 `객체 안에 들어올 수 있는 타입을 정하도록 도와주는 개념`이라고 이해했다.
>    `들어오는 타입의 매개변수 화`

> 5. MyPick의 지네릭을 보면 `<T, K>`인데 만약 `{[P in K] : T[P]}`이라고 할 경우, `K가 만약 todo의 key가 아닌 다른 타입의 키`가 오면 `error 발생`한다.

- 그래서 K가 `Todo의 타입`이 될 수 있도록 해야한다.
- 그래서 `K extends keyof T`라고 하여 `K의 key는 T에 있는 key`가 되도록 했다.

## 새롭게 알게 된 내용

---

### 새롭게 알게 된 내용

`Pick<T, K>`

>

- T 타입으로부터 K 프로퍼티만 추출하도록 도와주는 `유틸리티` 타입
- 즉, `T 내에서` Union 타입 `k의 프로퍼티에 대한 타입들을 뽑아내는 것`

Ex)

```ts
interface Event {
  id: string;
  title: string;
  isDone: boolean;
  startDate: string;
}

type BaseInfo = Pick<Event, "id" | "title">;

interface PickedEvent {
  id: string;
  title: string;
}
```

- baseInfo와 PickedEvent는 동일

`keyof`

- TS에서 `어떤 객체의 타입 or 인터페이스의 key들만 뽑아오는 경우` 사용
- `Object.keys(Obj)`와 유사한 개념

```ts
interface Brand {
  Nike: "nike";
  Adidas: "adidas";
  Puma: "puma";
}

let type: keyof Brand;
```

- type은 `'Nike' | 'Adidas' | 'Puma'`라는 Union 타입이 되도록 해야 하는 것

`typeof`

- TS에서 `객체 or Enum`을 `interface`로 만들고 싶을 경우 사용

```ts
enum Brand = {
  Nike = 'nike',
  Adidas = 'adidas',
  Puma = 'puma'
}

let type : typeof Brand
```

- type은 Brand가 interface화 되는 것을 확인할 수 있음

### 질문

```ts
type MyPick2<T, K> = {
  [P in K extends keyof T] : T[P]
}
```

- `'?' expected.(1005)` 에러가 발생하는데 도저히 원인을 모르겠다.
