# Queue

</br>

## 큐(Queue)
큐(queue)는 선형 자로구조의 일종으로, 먼저 집어넣은 데이터가 먼저 나오는 성질(FIFO, First In First Out)을 지닌 자료 구조.<br/>
큐의 사용 예시로는 프린터의 인쇄 대기, 콜센터 고객 대기 시간 등이 있음.

### 큐의 특징
- 나중에 집어넣은 데이터가 먼저 나오는 스택과는 반대되는 개념을 가짐. 먼저 들어간 원소가 먼저 나오는 구조.
- CPU 작업을 기다리는 프로세스, 스레드 행렬 또는 네트워크 접속을 기다리는 행렬, 너비 우선 탐색(BFS), 캐시 등에 사용됨.
- 삽입 및 삭제에 O(1), 탐색에 O(n)이 걸림.

### 큐의 연산
- offer(x) (=add(x)) : x를 큐의 끝부분에 추가 (Enqueue)
- poll() (=remove()) : 큐의 첫번째 항목을 제거 (Dequeue)
- peek() : 큐의 가장 처음(front)에 있는 데이터를 확인
- isEmpty() : 큐가 비어있는지 확인

### 큐의 시간복잡도
|경우|접근(read)|탐색(search)|삽입(insert)|식제(delete)|
|:---|:---:|:---:|:---:|:---:|
|평균|$O(n)$|$O(n)$|$O(1)$|$O(1)$|
|최악|$O(n)$|$O(n)$|$O(1)$|$O(1)$|

### 큐의 구현
Array로 구현하면 poll 연산 이후 객체를 앞당기는 작업이 필요함. 하지만 List로 구현하면 객체 1개만 제거하면 되므로 삽입 및 삭제가 용이한 LinkedList(연결 리스트)로 구현하는 것이 좋음.

```java
public class MyQueue {
  private static class QueueNode {
    private T data;
    private QueueNode next;

    public QueueNode(T data) {
      this.data = data;
    }
  }

  private QueueNode first;
  private QueueNode last;

  public void add(T item) {
    QueueNode t = new QueueNode(item);

    if (last != null) last.next = t;
    last = t;
    if (first == null) first = last;
  }

  public T remove() {
    if (first == null) throw new NoSuchElementException();
    T data = first.data;
    first = first.next;

    if (first == null) last = null;
    return data;
  }

  public T peek() {
    if (first == null) throw new NoSuchElementException();
    return first.data;
  }

  public boolean isEmpty() {
    return first == null;
  }
}

// 출처 : https://gmlwjd9405.github.io/2018/08/02/data-structure-queue.html
```

### 원형 큐
- 배열을 이용해 큐를 구현하면 데이터를 삭제할때 모든 데이터를 한 칸씩 앞으로 이동해야하기 때문에
비효율적.
- 배열을 이용해 큐를 효과적으로 구현하기 위해 원형 큐의 구조로 구현 가능.
- 원형 큐는 선형 큐의 한계점을 해결하기 위한 자료구조이며, 배열의 마지막 인덱스에서 다음 인덱스로 넘어갈 때 '(index+1) % 배열의 사이즈'를 이용하여 OutOfBoundsException이 일어나지 않고 인덱스 0으로 순환되는 구조를 가짐.

### 원형 큐의 구현
```java
public class Queue {

    private final int MAX_SIZE = 5;
    private int front, rear;
    private int[] queue;

    public Queue2() {
        front = rear = 0;
        queue = new int[MAX_SIZE];
    }

    // front와 rear이 같은 위치에 있다면 큐가 비어있다는 뜻이다.
    private boolean isEmpty() {
        return front == rear;
    }
    // rear이 front의 바로 전 위치에 있다면 큐가 가득찼다는 뜻이다.
    private boolean isFull() {
        return (rear+1) % MAX_SIZE == front;
    }
    // 데이터가 들어갈 땐 rear만 움직인다. 
    // rear의 위치는 최근에 들어온 데이터의 위치이다. 
    // 즉, 새로운 데이터가 들어오기 위해 먼저 이동해야한다.
    public void enqueue(int data) {
        // 큐가 가득차면 들어갈 수 없다 -> 리턴
        if (isFull())
            System.out.println("It's FULL!!!");
            // 큐에 자리가 있다면 데이터를 넣는다.
        else {
            rear = (++rear) % MAX_SIZE; //원형 큐이기 때문에 순환한다.
            queue[rear] = data;
        }
    }
    // 데이터가 나갈 땐 front만 움직인다. 
    public int dequeue() {
        int preIndex;
        // 큐가 비어있다면 데이터를 뺄 수 없다. 나는 데이터가 없다는 신호로 -1을 리턴했다(임시 정의)
        if (isEmpty())
            return -1;
            // 큐에 데이터가 있다면 데이터를 뺀다. front 이동(증가)
        else {
            preIndex = front;
            front = (++front) % MAX_SIZE;
        }
        return queue[preIndex];
    }

    public void display() {
        System.out.println("FRONT :"+front+" REAR :"+rear);
        System.out.print("QUEUE DATA: ");
        for(int index = front + 1; index != (rear+1) % MAX_SIZE; index = (index+1) % MAX_SIZE)
            System.out.print(queue[index] + " ");
        System.out.println();
        System.out.println();
    }
}

// 출처 : https://mailmail.tistory.com/41
```