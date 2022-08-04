# LinkedList

</br>

## 연결 리스트(Linked List)

### 연결 리스트란
연결 리스트는 데이터를 감싼 노드를 포인터로 연결해서 공간적인 효율성을 극대화시킨 자료 구조.

### 연결 리스트의 특징
- 연속적인 메모리 위치에 저장되지 않는 선형 데이터 구조.
- 각 노드는 데이터 필드와 다음 노드에 대한 참조를 포함하는 노드로 구성
- 삽입과 삭제가 O(1)이 걸리며 탐색에는 O(n)이 걸림.  
- prev 포인터와 next 포인터로 앞과 뒤의 노드를 연결시킨 것이 연결 리스트. 맨 앞에 있는 노드를 헤드(head)라고 함.
- 연결 리스트는 싱글 연결 리스트, 이중 연결 리스트, 원형 이중 연결 리스트가 있음.
    - `싱글 연결 리스트` : next 포인터만 가짐.
    - `이중 연결 리스트` : next 포인터와 prev 포인터를 가짐.
    - `원형 이중 연결 리스트` : 이중 연결 리스트와 같지만 마지막 노드의 next 포인터가 헤드 노드를 가리키는 것을 말함.

### 연결 리스트의 장점과 단점
배열은 비슷한 유형의 선형 데이터를 저장하는데 사용할 수 있지만 제한 사항이 있음 <br/>
(1) 배열의 크기가 고정되어 있어 미리 요소의 수에 대해 할당을 받아야 함 <br/>
(2) 새로운 요소를 삽입하는 것은 비용이 많이 듬 (공간을 만들고, 기존 요소 전부 이동) <br/><br/>
연결 리스트는 동적 크기 할당이 가능하고, 삽입/삭제가 용이하다는 장점이 있음.<br/>
단점으로는 임의접근이 불가능하며 첫번째 노드부터 순차적으로 요소에 접근해야 하고(이진 검색 수행 불가능), 다음 노드에 대한 참조를 저장할 여분의 메모리 공간이 각 원소에 필요하다는 점이 있음.


### 연결 리스트의 시간복잡도
|경우|접근(read)|탐색(search)|삽입(insert)|식제(delete)|
|:---|:---:|:---:|:---:|:---:|
|평균|$O(n)$|$O(n)$|$O(1)$|$O(1)$|
|최악|$O(n)$|$O(n)$|$O(1)$|$O(1)$|

### 연결 리스트의 구현
```java
package Structure;

public interface IList<T> {

    void add(T t);

    void insert(int index, T t);

    void clear();

    boolean delete(T t);

    boolean deleteByIndex(int index);

    T get(int index);

    int indexOf(T t);

    boolean isEmpty();

    boolean contains(T t);

    int size();
}
package Structure;

public class MyLinkedList<T> implements IList<T> {

    private int size;
    private Node head;

    public MyLinkedList() {
        this.size = 0;
        this.head = new Node(null); // dummy head node
    }

    @Override
    public void add(T t) {
        Node curr = this.head;
        while (curr.next != null){
            curr = curr.next; // curr의 next가 null이라는 건 뒤가 없다는 것
        }
        Node node = new Node(t);
        curr.next = node;
        this.size++;
    }

    @Override
    public void insert(int index, T t) {
        if (index > this.size || index < 0) { // index와 size가 같으면 add랑 같음
            throw new IndexOutOfBoundsException();
        }
        Node prev = this.head;
        Node curr = prev.next;

        int i = 0;
        while (i++ < index) {
            prev = prev.next;
            curr = curr.next;
        }

        Node node = new Node(t, curr);
        prev.next = node;
        this.size++;
    }

    @Override
    public void clear() {
        this.size = 0;
        this.head.next = null; // next가 없으면 찾아갈 방법이 없으니 쓰레기 수집기가 수거
    }

    @Override
    public boolean delete(T t) { // 데이터가 일치하면 삭제
        Node prev = this.head;
        Node curr = prev.next;
        while (curr != null) {
            if (curr.data.equals(t)) {
                prev.next = curr.next;
                curr.next = null;
                this.size--;
                return true;
            }
            prev = prev.next;
            curr = curr.next;
        }
        return false;
    }

    @Override
    public boolean deleteByIndex(int index) {
        if (index >= this.size || index < 0) { 
            throw new IndexOutOfBoundsException();
        }
        Node prev = this.head;
        Node curr = prev.next;

        int i = 0;
        while (i++ < index) {
            prev = prev.next;
            curr = curr.next;
        }

        prev.next = curr.next;
        curr.next = null;
        this.size--;
        return true;
    }

    @Override
    public T get(int index) {
        if (index >= this.size || index < 0) {
            throw new IndexOutOfBoundsException();
        }
        Node curr = this.head.next; // 여기가 첫 데이터
        int i = 0; // 노드의 변경이 있을 때만 prev 필요
        while (i++ < index) {
            curr = curr.next;
        }
        return curr.data;
    }

    @Override
    public int indexOf(T t) {
        Node curr = this.head.next;
        int index = 0;
        while (curr != null) {
            if (t.equals(curr.data)) {
                return index;
            }
            curr = curr.next;
            index++;
        }
        return -1;
    }

    @Override
    public boolean isEmpty() {
        return this.head.next == null;
    }

    @Override
    public boolean contains(T t) {
        Node curr = head.next;
        while (curr != null) {
            if (t.equals(curr.data)) {
                return true;
            }
            curr = curr.next;
        }
        return false;
    }

    @Override
    public int size() {
        return this.size; // 하나씩 뒤지면 O(N)의 시간 소요
    }

    private class Node {
        T data;
        Node next;

        Node(T data) {
            this.data = data;
        }

        Node(T data, Node next) {
            this.data = data;
            this.next = next;
        }
    }
}

//출처 : https://ish0301.tistory.com/entry/8%EC%A3%BC%EC%B0%A8-%EC%97%B0%EA%B2%B0-%EB%A6%AC%EC%8A%A4%ED%8A%B8Linked-List-%EC%9E%90%EB%B0%94%EB%A1%9C-%EA%B5%AC%ED%98%84
```

<br/>

## LinkedList와 ArrayList 비교
ArrayList는 데이터들이 순서대로 늘어선 배열의 형식을 취하고 있지만, LinkedList는 자료의 주소값으로 서로 연결된 형식을 취하고 있음.

### ArrayList
- 원하는 데이터에 무작위로 접근할 수 있음.
- 리스트의 크기가 제한되어 있으며, 리스트의 크기를 재조정하는 것은 많은 연산이 필요.
- 데이터의 추가/삭제를 위해서는 임시 배열을 생성하고 복제하고 있어 시간이 오래 걸림.

### LinkedList
- 리스트의 크기에 영향 없이 데이터 추가할 수 있음.
- 데이터를 추가하기 위해 새로운 노드를 생성하여 연결하므로 추가/삭제 연산이 빠름.
- 무작위 접근이 불가능하며, 순차 접근만이 가능함.
<br/>

따라서 데이터의 검색이 주가 되는 경우에는 ArrayList를 사용하는 게 좋다.<br/>
데이터의 삽입, 삭제가 빈번하다면 ArrayList보다는 LinkedList를 사용하는 편이 낫다.