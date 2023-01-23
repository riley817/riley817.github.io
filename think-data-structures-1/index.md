# [Think Data Structures] 자바 배열 리스트 구현하기


## 목표
- 자바 배열을 사용하여 요소를 저장하는 List 인터페이스 구현한다.

## 클래스 정의와 인스턴스 변수, 생성자
```java
public class MyArrayList<T> implements List<T> {
    int size;                    // keeps track of the number of elements
    private T[] array;           // stores the elements

    public MyArrayList() {
        array = (T[]) new Object[10];
        size = 0;
    }
}
```
- `size` 변수는 MyArrayList의 요소 개수를 추적한다.
- `array` 변수는 실제로 그 요소들을 저장하는 배열을 의미한다.
- 생성자는 10개의 요소를 갖는 빈 배열을 생성, size 변수는 0으로 설정한다.

## `add` 메서드 살펴보기
### `boolean add(E e)`
```java
@Override
public boolean add(T element) {
    if(size >= array.length) {
        T[] temp = (T[]) new Object[array.length * 2];
        System.arraycopy(array, 0, temp, 0, array.length);
        array = temp;
    }
    array[size] = element;
    size++;
    return true;
}
```
- 배열에 여분의 공간이 없으면 더 큰 배열을 만들어 복사 한다.
- 리스트 마지막에 요소를 추가한다.
- 리스트의 사이즈를 증가한다.

## `get` 메서드 살펴보기

```java
@Override
public T get(int index) {
    if (index < 0 || index >= size) {
        throw new IndexOutOfBoundsException();
    }
    return array[index];
}
```
- 인덱스가 범위를 벗어나면 `IndexOutOfBoundsException` 예외를 던진다.
- 배열의 인덱스 요소를 읽고 반환한다.

## `set` 메서드 구현하기
### `E set(int index, E element)`
이 목록에 지정된 위치에 있는 요소를 입력받은 요소로 변경한다. 인덱스를 검사하는 코드의 반복은 피한다.

**Parameter**
- `index` - 교체 할 요소의 인덱스
- `element` - 특정한 위치에 저장할 요소

```java
@Override
public T set(int index, T element) {
    T prev = get(index);
    array[index] = element;
    return prev;
}
```
- `get(index)` 메서드를 사용하여 인덱스의 범위를 검사하는 코드의 중복을 피한다.
- 입력받은 요소를 `array` 입력받은 위치에 대입한다.
- 이전 요소를 리턴한다.

## `indexOf` 구현하기
### `int indexOf(Object o)`
- 이 목록에서 지정된 요소가 처음 나타나는 인덱스를 반환한다. 요소가 없으면 -1을 반환
- `(o==null ? get(i)==null : o.equals(get(i)))`

**Parameter**
- `o` - 검색할 요소

**Return**
- 이 목록에서 지정된 요소가 처음 나타나는 인덱스


```java

private boolean equals(Object target, Object element) {
    if (target == null) {
        return element == null;
    } else {
        return target.equals(element);
    }
}

@Override
public int indexOf(Object target) {
    for(int i = 0; i < size; i++) {
        if(equals(target, array[i])) {
            return i;
        }
    }
    return -1;
}
```
- 책에서는 배열의 요소를 대상 값과 비교하여 같으면 true를 반환하는 equals 헬퍼 메서드를 제공. (null 처리 포함)
- 리스트의 사이즈만큼 순회하면서 입력받은 요소와 같은 요소가 있는지 확인한다. 없는 경우 -1

## 특정 위치에 add 하는 메서드 구현
### `void add(int index, E element)`
- 요소를 리스트의 지정한 위치에 삽입. 삽입 된 위치에 있는 다음 요소들을 모두 오른쪽으로 이동. 
- 배열을 더 크게 늘리는 코드는 반복하지 않는다.
- (?? 이건 다시 봐야겠다...)
```java
@Override
public void add(int index, T element) {
    if (index < 0 || index > size) {
        throw new IndexOutOfBoundsException();
    }
    // add the element to get the resizing
    add(element);

    // shift the elements
    for (int i=size-1; i>index; i--) {
        array[i] = array[i-1];
    }
    // put the new one in the right place
    array[index] = element;
}
```

## `remove` 메서드 구현
### `E remove(int index)`
- 지정한 위치에 존재하는 요소를 삭제한다. 삭제된 위치 다음 요소드를 모두 왼쪽으로 이동시킨다.

```java
@Override
public T remove(int index) {
    T prev = get(index);
    for(int i = index + 1; i < size; i++) {
        array[i - 1] = array[i];
    }
    size--;
    return prev;
}
```

- `get(index)` 메서드를 통해 인덱스에 위치한 요소를 삭제할 요소를 가져온다.
- 삭제하는 인덱스 다음 요소를 왼쪽으로 이동한다.
- 리스트의 `size`를 감소시킨다.
- 이전 요소를 리턴한다. 




