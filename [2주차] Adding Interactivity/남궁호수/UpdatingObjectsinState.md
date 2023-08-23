## 객체 state 업데이트

JS에서는 숫자, 문자, 불리언과 같은 원시 자료형 값을 변경할 수 없다.
react state에서는 숫자, 문자, 불리언과 같이 불변하는 것처럼 객체를 취급해야 한다.
객체는 직접 변경이 가능하지만, 객체를 변경하지 않고 교체해야 한다.

```jsx
import { useState } from 'react';
export default function MovingDot() {
  const [position, setPosition] = useState({
    x: 0,
    y: 0
  });
  return (
    <div
      onPointerMove={e => {
        setPosition({
          x: e.clientX,
          y: e.clientY
        });
        {/* onPointerMove={e => {
            position.x = e.clientX;
            position.y = e.clientY;
            이렇게 작성하면 안됨.
}} */}
      }}
      style={{
        position: 'relative',
        width: '100vw',
        height: '100vh',
      }}>
      <div style={{
        position: 'absolute',
        backgroundColor: 'red',
        borderRadius: '50%',
        transform: `translate(${position.x}px, ${position.y}px)`,
        left: -10,
        top: -10,
        width: 20,
        height: 20,
      }} />
    </div>
  );
}
```

***
새 객체를 생성하고 set 설정자 함수에 전달해야 한다.
이때, 모든 속성을 개별적으로 복사하지 않고, ... 객체 전개 구문을 사용한다.
```jsx
setPerson({
  ...person, // Copy the old fields
             // 이전 필드를 복사합니다.
  firstName: e.target.value // But override this one
                            // 단, first name만 덮어씌웁니다. 
});
```
```jsx
import { useState } from 'react';

export default function Form() {
  const [person, setPerson] = useState({
    firstName: 'Barbara',
    lastName: 'Hepworth',
    email: 'bhepworth@sculpture.com'
  });

  function handleFirstNameChange(e) {
    setPerson({
      ...person,
      firstName: e.target.value
    });
  }

  function handleLastNameChange(e) {
    setPerson({
      ...person,
      lastName: e.target.value
    });
  }

  function handleEmailChange(e) {
    setPerson({
      ...person,
      email: e.target.value
    });
  }

  return (
    <>
      <label>
        First name:
        <input
          value={person.firstName}
          onChange={handleFirstNameChange}
        />
      </label>
      <label>
        Last name:
        <input
          value={person.lastName}
          onChange={handleLastNameChange}
        />
      </label>
      <label>
        Email:
        <input
          value={person.email}
          onChange={handleEmailChange}
        />
      </label>
      <p>
        {person.firstName}{' '}
        {person.lastName}{' '}
        ({person.email})
      </p>
    </>
  );
}
```
 주의해야 할 점으로는, ... 전개구문은 '얕은' 구문으로, 한단계 깊이만 복사할 수 있다.

 ``jsx
 export default function Form() {
  const [person, setPerson] = useState({
    firstName: 'Barbara',
    lastName: 'Hepworth',
    email: 'bhepworth@sculpture.com'
  });

  function handleChange(e) {
    setPerson({
      ...person,
      [e.target.name]: e.target.value
    });
  }

  return (
    <>
      <label>
        First name:
        <input
          name="firstName"
          value={person.firstName}
          onChange={handleChange}
        />
      </label>
      <label>
        Last name:
        <input
          name="lastName"
          value={person.lastName}
          onChange={handleChange}
        />
      </label>
      <label>
        Email:
        <input
          name="email"
          value={person.email}
          onChange={handleChange}
        />
      </label>
      <p>
        {person.firstName}{' '}
        {person.lastName}{' '}
        ({person.email})
      </p>
    </>
  );
}
```
다음과 같이, 객체 내에서 [] 중괄호를 사용하여 동적이름을 가진 프로퍼티를 지정할 수 있다.
e.target.name은 input DOM 요소에 지정된 name 속성을 참조한다.

***
중첩된 객체 구조를 변경하려면 어떻게 해야할까? state는 불변으로 취급해야 한다. 변경이 아닌 복사를 통해.
```jsx
const [person, setPerson] = useState({
  name: 'Niki de Saint Phalle',
  artwork: {
    title: 'Blue Nana',
    city: 'Hamburg',
    image: 'https://i.imgur.com/Sd1AgUOm.jpg',
  }
});

//첫번째 방법
const nextArtwork = { ...person.artwork, city: 'New Delhi' };
const nextPerson = { ...person, artwork: nextArtwork };
setPerson(nextPerson);

// 두번째 방법
개인적으로 두번째가 나을 것 같다.
setPerson({
  ...person, // Copy other fields
  artwork: { // but replace the artwork
             // 대체할 artwork를 제외한 다른 필드를 복사합니다.
    ...person.artwork, // with the same one
    city: 'New Delhi' // but in New Delhi!
                      // New Delhi'는 덮어씌운 채로 나머지 artwork 필드를 복사합니다!
  }
});

```
city를 업데이트 하려면 다음과 같이 객체 복사를 통해 state를 변경해야 한다.

***
```jsx
let obj1 = {
  title: 'Blue Nana',
  city: 'Hamburg',
  image: 'https://i.imgur.com/Sd1AgUOm.jpg',
};

let obj2 = {
  name: 'Niki de Saint Phalle',
  artwork: obj1
};

let obj3 = {
  name: 'Copycat',
  artwork: obj1
};
```
객체 중첩은 실제로 중첩된 것이 아니라, 프로퍼티를 사용하여 서로를 가리키는 별도의 객체라고 볼 수 있다.

***
state가 깊게 중첩된 경우는 Immer를 사용할 수 있다.
변이 구문을 사용하여 작성해도 자동으로 사본을 생성해주는 라이브러리이다. 
```jsx
updatePerson(draft => {
  draft.artwork.city = 'Lagos';
});
```
다음과 같이 state의 불변성을 해치는 것처럼 보이는 코드를 Immer 라이브러리로 쉽게 작성할 수 있다. 자세한 내용은 Immer 관련 내용을 보는게 좋을 듯 하다.
Immer에서 제공하는 draft는 프록시라는 특수한 유형의 객체라고 한다. 사용자가 수행하는 작업을 기록하기 때문에 , 원하는 만큼 수정이 가능하다. draft의 어떤 부분이 변경되었는지 파악하고 편집내용이 포함된 새로운 객체를 생성한다고 한다.

```jsx
import { useImmer } from 'use-immer';

export default function Form() {
  const [person, updatePerson] = useImmer({
    name: 'Niki de Saint Phalle',
    artwork: {
      title: 'Blue Nana',
      city: 'Hamburg',
      image: 'https://i.imgur.com/Sd1AgUOm.jpg',
    }
  });

  function handleNameChange(e) {
    updatePerson(draft => {
      draft.name = e.target.value;
    });
  }

  function handleTitleChange(e) {
    updatePerson(draft => {
      draft.artwork.title = e.target.value;
    });
  }

  function handleCityChange(e) {
    updatePerson(draft => {
      draft.artwork.city = e.target.value;
    });
  }

  function handleImageChange(e) {
    updatePerson(draft => {
      draft.artwork.image = e.target.value;
    });
  }

  return (
    <>
      <label>
        Name:
        <input
          value={person.name}
          onChange={handleNameChange}
        />
      </label>
      <label>
        Title:
        <input
          value={person.artwork.title}
          onChange={handleTitleChange}
        />
      </label>
      <label>
        City:
        <input
          value={person.artwork.city}
          onChange={handleCityChange}
        />
      </label>
      <label>
        Image:
        <input
          value={person.artwork.image}
          onChange={handleImageChange}
        />
      </label>
      <p>
        <i>{person.artwork.title}</i>
        {' by '}
        {person.name}
        <br />
        (located in {person.artwork.city})
      </p>
      <img 
        src={person.artwork.image} 
        alt={person.artwork.title}
      />
    </>
  );
}
``` 
Immer 사용 예시. use-immer을 설치하고, useState 대신 useImmer을 사용하면 된다.
 state에 중첩이 있고 객체를 복사하면 코드가 반복되는 경우 업데이트 핸들러를 간결하게 유지하는 데 Immer는 좋은 방법이다.

 ***
 리액트에서 state변이를 권장하지 않는 이유는?
 1. 디버깅
 2. 리액트의 최적화
    - 이전 프로퍼티나 state가 다음 프로퍼티나 state가 서로 동일한 경우 작업을 건너뛰는 작업을
    state를 변이하지 않는다면 비교 확인이 빠릅니다.
 3. state가 변이 된다면 state에 의존하는 리액트 기능 사용이 어렵다.
 
정리
1. 리액트의 모든 state는 불변으로 취급 (객체는 새로운 버전을 생성)
2. 객체 전개 구문을 사용하여 객체 사본 생성이 가능 (한 단계의 깊이만 수정하는 얕은 구문)
3. state의 객체 깊이가 꽤 깊을 경우는 Immer를 사용