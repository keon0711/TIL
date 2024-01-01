## toArray
List를 Array로 바꿀 때는 아래와 같이 구현한다.
```java
String[] stringArray = StringList.toArray(new String[0]);
```
toArray 메서드로 전달되는 `new String[0]`은 새롭게 생성될 String[]의 크기를 정하기 위해 존재한다.
아래는 ArrayList에 구현된 toArray 메서드이다.
```java
public <T> T[] toArray(T[] a) {  
    if (a.length < size)  
        // Make a new array of a's runtime type, but my contents:  
        return (T[]) Arrays.copyOf(elementData, size, a.getClass());  
    System.arraycopy(elementData, 0, a, 0, size);  
    if (a.length > size)  
        a[size] = null;  
    return a;  
}
```
`size`는 리스트의 길이로, 만약 원본 리스트 길이가 매개변수 a의 크기보다 더 크다면 원본 리스트 크기를 가진 배열을 생성한다. 그러나 인자로 넘어간 배열의 사이즈가 더 크다면 해당 배열과 같은 크기를 가진 배열을 생성한다.

## 결론
- List의 toArray 메서드의 인자는 배열의 타입과 길이를 결정한다.
- 원본과 같은 크기로 만들고 싶다면 `new String[0]`을 하면 된다.
- 만약 메서드에 아무것도 전달하지 않는다면 같은 크기의 `Object` 배열이 만들어진다.