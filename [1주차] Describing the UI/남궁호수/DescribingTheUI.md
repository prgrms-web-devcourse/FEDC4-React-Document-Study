- 시작

class 컴포넌트도 존재하지만, 공식문서에서 함수형 컴포넌트를 권장하고 있습니다.

왜 함수형 컴포넌트를 쓰게 되었을까요?
과거와 다르게 React v16부터, useState를 통해 state를 관리할 수 있게 되었고, useEffect를 통해 lifeCycle을 관리할 수 있게 되었기 때문입니다!

<br>
<br>
<br>
<br>
<br>
<br>

한 파일에서는 단 하나의 default export만 사용할 수 있지만 named export는 여러 번 사용할 수 있습니다.

예시.
function Profile() {
return (
<img
      src="https://i.imgur.com/QIrZWGIs.jpg"
      alt="Alan L. Hart"
    />
);
}
function Profile2() {
return (
<img
      src="https://i.imgur.com/QIrZWGIs.jpg"
      alt="Alan L. Hart"
    />
);
}

export default function Gallery() {
return (
<section>
<h1>Amazing scientists</h1>
<Profile />
<Profile />
<Profile />
</section>
);
}

--->다른 컴포넌트에서,
import { Profile, Profile2 } from './Gallery.js';
import Gallery from './Gallery.js';

<br>
<br>
<br>
<br>
<br>

컴포넌트의 이름은 항상 대문자로 시작합니다.
<br>
<br>
<br>
<br>
<br>

함수 컴포넌트는 JSX 마크업을 반환합니다.

<br>
<br>
<br>
<br>
<br>
<br>

Web이 더욱 인터랙티브해지면서, 로직이 내용을 결정하는 경우가 많아졌습니다. 그래서 JavaScript가 HTML을 담당하게 되었습니다! 이것이 바로 React에서 렌더링 로직과 마크업이 같은 위치에 함께 있게 된 이유입니다. 즉, 컴포넌트에서 말입니다.

![image](https://github.com/prgrms-web-devcourse/FEDC4-React-Document-Study/assets/124763142/ab2f3b84-77b2-43af-a124-94d5419aa70a)

<br>
<br>
<br>
<br>
<br>
<br>

JSX에서는 태그를 명시적으로 닫아야 합니다. <img>처럼 자체적으로 닫아주는 태그는 반드시 <img /> 형태로 작성해야 하며, <li>oranges와 같은 래핑 태그도 <li>oranges</li> 형태로 작성해야 합니다.

<br>
<br>
<br>
<br>
<br>
<br>

react jsx의 어트리뷰트가 헷갈릴떄가 있었는데, 변환기를 통해 해결이 가능합니다! https://transform.tools/html-to-jsx

기존 마크업에서 모든 어트리뷰트를 변환하는 것은 지루할 수 있습니다. 변환기를 사용하여 기존 HTML과 SVG를 JSX로 변환하는 것을 추천합니다. 변환기는 매우 유용하지만 그래도 JSX를 혼자서 편안하게 작성할 수 있도록 어트리뷰트를 어떻게 쓰는지 이해하는 것도 중요합니다!

+tmi로,
js에서 만든 예시 데이터를 json으로 바꾸고 싶을 때 쓰면 좋겠다는 생각이 들었습니다.
https://transform.tools/js-object-to-json

<br>
<br>
<br>
<br>
<br>
<br>

중괄호를 사용하면 JavaScript 논리와 변수를 마크업으로 가져올 수 있습니다.

<br>
<br>
<br>
<br>
<br>
<br>

props값으로 객체 주기.
export default function Profile() {
return (
<Avatar
person={{ name: 'Lin Lanying', imageId: '1bX5QH6' }}
size={100}
/>
);
}

<br>
<br>
<br>
<br>
<br>
<br>

props 객체 spread 뿌려주기
function Profile({ person, size, isSepia, thickBorder }) {
return (
<div className="card">
<Avatar
        person={person}
        size={size}
        isSepia={isSepia}
        thickBorder={thickBorder}
      />
</div>
);
}

------>
function Profile(props) {
return (
<div className="card">
<Avatar {...props} />
</div>
);
}

<br>
<br>
<br>
<br>
<br>
<br>

JSX 태그 내에 콘텐츠를 중첩하면, 부모 컴포넌트는 해당 콘텐츠를 children이라는 prop으로 받을 것입니다. 예를 들어, 아래의 Card 컴포넌트는 <Avatar />로 설정된 children prop을 받아 이를 래퍼 div에 렌더링 할 것입니다.

import Avatar from './Avatar.js';

function Card({ children }) {
return (
<div className="card">
{children}
</div>
);
}

export default function Profile() {
return (
<Card>
<Avatar
size={100}
person={{
          name: 'Katsuko Saruhashi',
          imageId: 'YfeOqp2'
        }}
/>
</Card>
);
}

<br>
<br>
<br>
<br>
<br>
<br>

uuid 패키지를 통해, 리스트 데이터 키 만들어주기
https://www.npmjs.com/package/uuid

key를 잘 선택하면 React가 정확히 무슨 일이 일어났는지 추론하고 DOM 트리에 올바르게 업데이트 하는데 도움이 됩니다. key는 배열 내 위치보다 더 많은 정보를 제공합니다. 재정렬로 인해 *위치*가 변경되더라도 key는 React가 생명주기 내내 해당 항목을 식별할 수 있게 해줍니다.

index를 key로 하면 안되는 이유!
실제로 key를 전혀 지정하지 않으면 React는 인덱스를 사용합니다. 하지만 항목이 삽입되거나 삭제하거나 배열의 순서가 바뀌면 시간이 지남에 따라 항목을 렌더링하는 순서가 변경됩니다. 인덱스를 key로 사용하면 종종 미묘하고 혼란스러운 버그가 발생합니다
