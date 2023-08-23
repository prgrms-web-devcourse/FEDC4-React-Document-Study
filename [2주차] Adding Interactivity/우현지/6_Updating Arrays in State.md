# Updating Arrays in State

> 자바스크립트에서 배열은 변경 가능하지만 state에 저장할 때는 변경이 불가능한 것으로 취급해야 한다. <br>객체와 마찬가지로, state에 저장된 배열을 업데이트하려면, 새로운 배열을 만들고(또는 기존 배열 복사본을 만듦) 새 배열을 사용하도록 state를 설정해야 한다.

|           | avoid                | prefer                          |
| --------- | -------------------- | ------------------------------- |
| adding    | push, unshift        | concat, [...arr], spread syntax |
| removing  | pop, shift, splice   | filter, slice                   |
| replacing | splice, arr[i] = ... | map                             |
| sorting   | reverse, sort        | 배열을 복사한 다음 처리         |

## Adding to Array(배열에 추가하기)

- push() 는 배열을 변이한다.(avoid 방식)
- `... 배열 전개 구문`을 사용한다.(prefer)
- 추가할 요소를 배열의 끝에 추가하면 push()의 기능을, 배열의 시작 부분에 추가하면 unshift()의 기능을 모두 수행할 수 있다.

## Removing from an array(배열에서 제거하기)

- 배열에서 항목을 제거하는 가장 쉬운 방법은 필터링하은 것이다. => filter 메서드 사용

## Transforming an array(배열 변경하기)

- 배열의 일부 또는 모든 항목을 변경하려면 map()을 사용하여 새로운 배열을 만들수 있다.

## Replacing items in an array(배열에서 항목 교체하기)

- 배열에서 하나 이상의 항목을 바꾸고 싶은 경우(arr[0]='apple')에도 map을 사용하는 것이 좋다.

## Inserting into an array(배열에 삽입하기)

- 배열의 시작, 끝도 아닌 특정 위치에 아이템을 삽입하고 싶을 때, `...`배열 전개 구문과 slice() 메서드를 함께 사용한다.

## Making other changes to an array(배열에 다른 변경사항 적용하기)

- 배열을 반전시키거나 정렬하고 싶을 때, JavaScript의 reverse(), sort()메서드는 원래 배열을 변이하므로 직접 사용할 수 없다.
- 대신 배열을 먼저 복사한 다음 변이한다.
- ...배열 전개 구문을 사용하여 원본 배열을 복사하고 변이메서드를 사용한다.
- 주의사항: 배열을 복사하더라도 배열 내부의 기존 항목을 직접 변이할 수은 없다. 이는 원본 배열의 내부 요소가 같이 변경되기때문이다.
  - 중첩된 state를 업데이트 할 때는 업데이트하려는 지점부터 최상위 수준까지 복사본을 만들어야 한다.
