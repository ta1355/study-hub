# Dataverse 관계형 모델 & Power Apps 요약

## 1. 관계 유형

| 관계 유형                     | 특징                                                           | 예시 (Contoso)                                          | Power Fx 접근 방식                                                                                                               |
| ----------------------------- | -------------------------------------------------------------- | ------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------- |
| **일대다(1:N) / 다대일(N:1)** | 부모(기본) 테이블의 한 행이 자식(관련) 테이블의 여러 행과 연결 | 위치 1 : N 책상<br>책상 N : 1 위치<br>사용자 1 : N 예약 | 점 표기법으로 자식→부모 또는 부모→자식 탐색<br>`ThisItem.Location.Address`<br>`Filter(Location.Selected.Desks, Status="Active")` |
| **다대다(N:N)**               | 교차(숨김) 테이블이 두 테이블의 여러 행을 서로 연결            | 책상 N : N 책상 기능<br>사용자 N : N 즐겨찾는 책상      | `ThisItem.'Desk Features'`<br>`Concat(ThisItem.'Desk Features', Name, ",")`<br>Relate()/Unrelate() 사용                          |

## 2. 캔버스 앱에서 관계 사용하는 기본 패턴

- **부모→자식(일대다)**

```powerfx
Filter(LocationDropdown.Selected.Desks, Status='Status (Desks)'.Active)
```

또는

```powerfx
LocationDropdown.Selected.Desks
```

- **자식→부모(다대일)**

```powerfx
ThisItem.Location.Address
```

- **다단계 탐색**

```powerfx
ThisItem.Location.'Primary Contact'.'Full Name'
```

## 3. 관련 행 추가·수정

- **Choices() 함수** : 조회 열에 가능한 값 제공

```powerfx
Filter(Choices([@Desks].contoso_Location), Status="Active")
```

- **Patch() 함수** : 행과 부모 행 연결

```powerfx
Patch(Desks, ThisItem, {Location:LocationDropdown.Selected})
```

- **Relate()/Unrelate() 함수** : 관계 추가/해제

```powerfx
Relate(LocationDropdown.Selected.Desks, ThisItem)
Unrelate(LocationDropdown.Selected.Desks, ThisItem)
```

## 4. 다대다 관계 폼 처리 팁

- 양식 자동 생성 시 다대다 필드는 바로 작동하지 않음 → **카드 잠금 해제 후 Items 속성을 Choices()로 수정**
- 제출 시 Update 속성 지우고, OnSelect에 다음 순서로 수식 작성:
  1. 콤보 상자 선택값을 컬렉션으로 저장
  2. SubmitForm() 호출
  3. 저장한 컬렉션으로 Relate() 호출해 연결 생성

## 5. 설계 시 고려사항

- 관계 동작(삭제·할당·공유 시)을 올바르게 설정해야 고아 레코드 방지 가능
- 다대다 관계로는 **추가 속성 저장 불가** → 필요하면 교차 테이블 직접 생성 후 두 N:1 관계로 설계

## 6. 실습 예제 (Contoso)

- **예약 갤러리**: 현재 로그인 사용자 예약만 표시

```powerfx
LookUp(
    Users,
    'Primary Email'=currentUserEmail
).'Reservations (contoso_Reservation_ReservedFor_SystemUser)'
```

- **즐겨찾는 책상**: 사용자–데스크 N:N 관계 생성 후 Relate()로 즐겨찾기 추가

```powerfx
Relate(UserGallery.Selected.'Favorite Desks', DeskGallery.Selected)
```
