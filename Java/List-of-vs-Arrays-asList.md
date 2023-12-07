### List.of vs Arrays.asList
- Arrays.asList는 불변, List.of는 가변
- Arrays.asList는 Null을 허용, List.of는 Null을 허용하지 않음
- Arrays.asList는 원본을 참조, List.of는 값을 기반으로 독립적인 객체를 생성(참조 X)
- 둘 다 크기는 변경할 수 없다. 크기를 바꾸려면 Collections을 생성해 값을 옮겨야한다.
- 크기가 크고 동적일 경우 Arrays.of를 사용하고, 작고 변경되지 않는 경우 List.of를 사용