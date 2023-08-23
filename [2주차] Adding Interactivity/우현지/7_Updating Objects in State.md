# Updating Objects in State

> React state에 있는 객체를 직접 변이해서는 안된다. 대신 객체를 업데이트하려면 새 객체를 생성하고(혹은 기존 객체의 복사본을 만들고), 해당 복사본을 사용하도록 state를 설정해야 한다.

- 기술적으로 객체자체의 내용을 변경하는 것은 가능하다. 이를 `변이(mutation)`라고 한다.

  position.x = 5;

- React state의 객체는 기술적으로 변이할 수 있지만, 숫자, 불리언, 무자열과 같이 불변하는 것처럼 취급해야한다.
- state에 넣는 모든 JavaScript 객체를 읽기 전용으로 취급해야 한다.

## Copying objects with the spread syntax (전개 구문을 사용하여 객체 복사하기)

- `...` 객체 전개 구문을 사용하여 객체의 모든 속성을 복사 할 수 있다.
- `...` 전개 구문은 얕은 구문으로, 한 단계 깊이만 복사한다는점에 유의해야 한다.

## Updating a nested object(중첩된 객체 업데이트 하기)

- 중첩된 객체의 속성을 변이할려면 중첩된 객체를 새로 생성하여 중첩된 객체의 속성을 변이하고 중첩된 객체를 가리키는 새로운 객체를 생성해야 한다.

person 객체에 중첩된 artwork 객체의 city 속성을 변이하려고 할때

```js
person.artwork.city = "New Delhi";
```

```js
const nextArtwork = {
  ...person.artwork,
  city: "New Delhi",
};
const nextPerson = {
  ...person,
  artwork: nextArtwork,
};
setPerson(nextPerson);
```

- 단일 함수 호출로 작성 할 수 있다.

```js
setPerson({
  ...person,
  artwork: {
    ...person.artwork,
    city: "New Delhi",
  },
});
```

## Write concise update logic with Immer(Immer로 간결한 업데이트 로직 작성)

- Immer는 변이 구문을 사용하여 작성하더라고 자동으로 사본을 생성해주는 편리한 인기 라이브러리이다.

```

**Immer 사용방법**

1. npm install use-immer
2. import {useImmer} from 'use-immer'
```
