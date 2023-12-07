## 공통점
- template 안에서 변수를 동적으로 만듬
- tempalte 안에서는 `.value`없이 `{{name}}`과 같이 변수명만으로 사용

## 차이점
#### ref
- ref는 string, int, array, object 전부 가능
- .value를 사용해서 값을 변경해야 함
- `name.value = "new name";`
#### reactive
- object나 array에 사용
- 기본자료형에는 사용 불가
- .value를 사용하지 않아도 됨
- `person.name = "new name";`