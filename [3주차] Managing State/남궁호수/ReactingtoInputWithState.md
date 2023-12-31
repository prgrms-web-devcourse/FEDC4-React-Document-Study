# input에 대한 반응 (state)

react는 직접 UI를 조작하지 않는다. 표시할 내용을 선언하면 react가 UI를 업데이트할 방법을 알아낸다.

### 선언적으로 UI 생각하기
1. 컴포넌트의 다양한 시각적 상태를 식별한다.
2. 상태 변화를 촉발하는 요소를 파악한다.
3. useState를 사용하여 메모리의 상태를 표현한다.
4. 필요없는 state 변수를 제거한다.
5. 이벤트 헨들러를 연결하여 state를 설정한다.

### 상태 변경을 촉발하는 요인
사람의 입력: 버튼 클릭, 필드 입력, 링크 이동
컴퓨터의 입력: 네트워크에서 응답 도착, 시간 초과, 이미지 로딩

### 컴포넌트를 만들 때, 생각해둘 것.
1. 모든 시각적 상태를 식별한다. 
2. 사람과 컴퓨터의 상태 변화를 생각한다.
3. useState로 상태를 관리한다.
4. 필요없는 state는 제거한다.
5. 이벤트 헨들러를 통해 state를 설정한다.