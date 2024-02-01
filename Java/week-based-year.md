## YearMonth 포맷 오류
### 문제
생성 기간 정보를 `YearMonth` 타입으로 정의했습니다.
예를 들어 "200108"와 같은 문자열을 입력받으면 2001년 08월을 의미하는 `YearMonth`로 변환해야 합니다.
YearMonth 타입으로 저장하기 위해 `DateTimeFormatter.ofPattern("YYYYMM")`을 사용했더니 `DateTimeParseException`이 발생했습니다.
<br>
### 문제 원인
DateTimeFormatter 사용법을 정확히 숙지하지 못해 발생한 문제였습니다.
 'Y'는 `week based year`을 의미하고, 'y'는 `year of era`를 의미한다고 합니다.
`year of era`는 달력 년도와 같다고 생각하면 되고, `week based year`은 주를 기준으로 년도를 선택한다고 합니다.
예를 들어 오늘이 `2024년 1월 1일 화요일`이라면 `week based year`에서는 어제인 `2023년 12월 31일 월요일`도 2024년 첫째주로 여겨`2023년 12월 31일`도 2024년으로 나타내게 됩니다.
문자열 데이터가 년도와 월만 가지기 때문에 `week based year`에서는 년도를 알 수 없어서 발생한 문제로 생각됩니다. 

<br>
### 해결 방법
년월을 지정하는 패턴을 "YYYYMM"에서 "yyyyMM"으로 변경해 오류를 해결할 수 있었습니다.
