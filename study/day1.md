# Power Fx 기초 정리

---

## `ThisItem`

- 현재 데이터 컨텍스트(예: 갤러리 내 각 항목)에서 항목을 참조
- 속성 접근 가능: `ThisItem.Name`, `ThisItem.Photo` 등
- 예:
  ```
  ThisItem.Name
  ```

---

## `If`

조건문 함수 (삼항 연산자와 비슷)

형식: If(조건, 참일 때 결과, 거짓일 때 결과)

예:

```
If(ThisItem.Name = "yoo", Color.Black, Color.White)
```

---

## `Filter`

테이블에서 조건에 맞는 데이터만 필터링

SQL의 WHERE 절과 비슷한 역할

예:

```
Filter(Machines, 'Machine Type ID' = 'Machine Type Gallery'.Selected.'Type ID')
```

---

## `Collect`

컬렉션(리스트) 생성 또는 데이터 추가

임시 테이블 형태로 앱 내에서 데이터 저장 가능

예:

```
Collect(CompareList, ThisItem)
```

---

## `First`

테이블에서 첫 번째 레코드 반환

반환값은 단일 레코드(Record)

예:

```
First(MyCollection)
```

특정 필드 접근:

```
First(MyCollection).Name
```

---

## 문자열 연결 (&)

동적 값과 문자열 합칠 때 사용

예:

```
User().FullName & " 고객님 주문이 완료되었습니다!"
```

조건을 활용한 속성 설정

예:

```
If(CountRows(CompareList) > 0, DisplayMode.Edit, DisplayMode.Disabled)
```

---

## `OnSelect`

버튼 클릭 등 이벤트 발생 시 실행되는 함수

React의 onClick과 유사함

---

## `Clear`

컬렉션의 데이터를 초기화(모두 삭제)

예:

```
Clear(CompareList)
```

---

## `Navigate`

화면 전환 함수

React의 라우터 이동과 유사

앞에는 이동할 화면, 뒤에는 이팩트(없으면 ScreenTransition.None )

예:

```
Navigate('Compare Screen', ScreenTransition.None)
```

---

## 추가 팁

UpdateContext: 화면 내 로컬 변수 설정

Set: 전역 변수 설정

LookUp: 조건에 맞는 첫 번째 레코드 검색 (Filter 후 First 조합 대신 사용 가능)

Text 함수: 숫자, 날짜 등 포맷팅 가능

컬렉션은 앱 실행 중 메모리에만 존재하며 종료 시 사라짐
