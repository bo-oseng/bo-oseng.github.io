---
layout: single

title: 스택, 큐, 덱 연결리스트로 구현 with python

categories:
  - Data-structure

tag: [LinkedList, 연결리스트, 스택, 큐, 덱, python, ADT]

toc: true

---
## 스택, 큐, 덱 ADT(Abstract Data Type) 연결리스트로 구현
대표적이 세가지 ADT 자료구조를 연결리스트를 활용해 직접 구현해 보았다.
## 스택(Stack)
스택이란 First In Last Out의 구조를 가지느 자료형으로 아래에서부터 차례로 접시를 쌓고 빼는걸 생각하면 된다.
<center>
  <img src="https://w.namu.la/s/434fe12ff0f55012d5037b34e7fcad2f5bac49952d2b4e102d9ca91b0acfb89f0cb89593b00cdc183e58b1b36fd8bfd0e07630fdbf1be695864e3446f80603b68f5825dc403a3cd3c25d33f3ce756716e499fe7bc84fee098434761aae28023e4906159d5f4de59ab8e9afd35ef65929" width="50%">
</center>
<center>
  <p>출처: 파이썬 알고리즘 인터뷰</p>
</center>

스택의 대표적인 메소드인 push, pop, peek, len, is_empty, print_stk을 구현 해보자. 해당 메소드들은 단일연결리스트로 쉽게 구현이 가능하다.   
[단일연결리스트](https://bo-oseng.github.io/data-structure/LinkedList-(1)-SingleLinkedList/)   
<br>  

```python
from typing import Any, Type


class Node:
    def __init__(self, data: Any, next: 'Node' = None) -> None:
        self.data = data
        self.next = next


class Stak:
    def __init__(self) -> None:
        self.no = 0
        self.head = None
        self.cur = None

    def __len__(self) -> int:
        return self.no

    def is_empty(self):
        return self.no <= 0

    def push(self, data: Any) -> None:
        p = self.cur = Node(data, None)
        if self.is_empty():
            self.head = p
        else:
            ptr = self.head
            while ptr.next is not None:
                ptr = ptr.next
            ptr.next = p
        self.no += 1

    def pop(self) -> Any:
        if self.is_empty():
            print("스택이 비어있습니다.")
            return -1
        else:
            ptr = prev = self.cur = self.head
            while ptr.next is not None:
                prev = self.cur = ptr
                ptr = ptr.next
            prev.next = None
            self.no -= 1
            print(f'pop: {ptr.data}')
            return ptr.data

    def peek(self) -> Any:
        if self.is_empty():
            print("peek: Empty")
            return
        else:
            print(f'peek: {self.cur.data}')

    def print_stk(self):
        ptr = self.head
        while ptr is not None:
            print(f'{ptr.data} -> ')
            ptr = ptr.next


s = Stak()
s.push(6)
s.push(5)
s.push(4)
s.push(3)
s.push(2)
s.push(1)
s.push(0)
s.pop()
s.pop()
s.pop()
print()
s.print_stk()
print()
s.peek()
print()
print(len(s))
print()
s.pop()
s.pop()
s.pop()
s.pop()
s.pop()
print()
s.peek()



```

    pop: 0
    pop: 1
    pop: 2
    
    6 -> 
    5 -> 
    4 -> 
    3 -> 
    
    peek: 3
    
    4
    
    pop: 3
    pop: 4
    pop: 5
    pop: 6
    스택이 비어있습니다.
    
    peek: Empty


## 큐(Queue)
큐란 First In First Out의 자료구조로 차례대로 줄을 서는 상황을 생각하면 된다.  
<center>
  <img src="https://upload.wikimedia.org/wikipedia/commons/5/52/Data_Queue.svg" width="50%">
</center>
<center>
  <p>출처: 위키피디아</p>
</center>
큐의 대표적 메소드인 enqueue, dequeue, peek_rear, peek_front, is_empty, len, print_queue를 구현해보자.
스택과 마찬가지로 연결리스트를 활용하면 쉽게 구현할 수 있다.   

[단일연결리스트](https://bo-oseng.github.io/data-structure/LinkedList-(1)-SingleLinkedList/)   
<br>


```python
from typing import Any, Type


class Node:
    def __init__(self, data: Any, next: 'Node' = None):
        self.data = data
        self.next = next


class Queue:
    def __init__(self):
        self.no = 0
        self.head = None
        self.rear = None
        self.front = None

    def __len__(self) -> int:
        return self.no

    def is_empty(self) -> bool:
        return self.no <= 0

    def enqueue(self, data: Any) -> None:
        ptr = self.head
        self.head = self.rear = Node(data, ptr)
        self.no += 1

    def dequeue(self) -> None:
        if self.is_empty():
            print("큐가 비었습니다.")
        else:
            ptr = prev = self.front =self.head
            while ptr.next is not None:
                prev = self.front = ptr
                ptr = ptr.next
            prev.next = None
            self.no -= 1
            print(f'dequeue: {ptr.data}')
            return ptr.data

    def peek_rear(self) -> None:
        if self.is_empty():
            print("Empty")
        else:
            print(f'rear: {self.rear.data}')

    def peek_front(self) -> None:
        if self.is_empty():
            print("Empty")
        else:
            print(f'rear: {self.front.data}')

    def print_queue(self) -> None:
        tmp = []
        ptr = self.head
        while ptr is not None:
            tmp.append(ptr.data)
            ptr = ptr.next
        for t in tmp:
            print(f'{t} ->')



q = Queue()
q.enqueue(0)
q.enqueue(1)
q.enqueue(2)
q.enqueue(3)
q.enqueue(4)
q.print_queue()
print()
q.dequeue()
q.dequeue()
q.dequeue()
print()
q.print_queue()
print()
q.peek_rear()
q.peek_front()
print(f'len: {len(q)}')
q.dequeue()
q.dequeue()
q.dequeue()
q.peek_rear()
q.peek_front()



```

    4 ->
    3 ->
    2 ->
    1 ->
    0 ->
    
    dequeue: 0
    dequeue: 1
    dequeue: 2
    
    4 ->
    3 ->
    
    rear: 4
    rear: 3
    len: 2
    dequeue: 3
    dequeue: 4
    큐가 비었습니다.
    Empty
    Empty

## 덱(Dequeue)
덱은 Double Ended Queue의 약자로 기존의 큐를 개선해 양쪽으로 자료의 출입이 모두 가능한 형태의 자료구조이다.
<center>
  <img src="https://w.namu.la/s/f609d0b148138acfee6cadf749d09f694f71740915afcf7a8e0eca7469601caaad5c05e60600ec9a77ed02b27b73d8bea06b55aa2f7eae97d54af2a99004105136d5364296bf049be150abbacb6e9735476116d6ee384501fc6794e24bd3ed0752f2ec2b57ab773079384d63dd08a1ef" width="50%">
</center>
<center>
  <p>출처: 파이썬 알고리즘 인터뷰</p>
</center>
덱은 스태과 큐나 달리 양 방향성이 있어서 이중 연결리스트로 구현하는것이 가장 잘 어울린다. 하지만 이중연결리스트 자체가 단일연결리스트보다 고려해야할 것이 조금 더 많고, 노드 자체가 차지하는 메모리가 조금더 많다. 하지만 탐색과 자료 추가삭제에 더 빠른 성능을 기대할 수 있다는 장점이 있다. 덱의 대표적 메소드인 appendleft, append, popleft, pop, len, get_rear, get_front, is_empty를 구현 해보자.    

[이중연결리스트](https://bo-oseng.github.io/data-structure/LinkedList-(2)-DoublyLikedList/)   
   
<br>


```python
from typing import Any

# appendleft, append, popleft, pop, len, get_rear, get_front, is_empty 구현


class Node:
    def __init__(self, data: Any, prev: "Node" = None, next: "Node" = None) -> None:
        self.data = data
        self.prev = prev
        self.next = next

class Dequeue:
    def __init__(self) -> None:
        self.no = 0
        self.head = self.tail = None
        self.rear = None
        self.front = None

    def __len__(self) -> int:
        print(f"len: {self.no}")
        return self.no

    def is_empty(self):
        return (self.no <= 0) and (self.head is None)

    def appendleft(self, data: Any) -> None:
        ptr = self.head
        p = self.rear = Node(data, None, ptr)
        self.head = p

        if self.is_empty():
            self.tail = self.head
        else:
            ptr.prev = p

        self.no += 1

    def append(self, data: Any) -> None:
        ptr = self.tail
        p = self.front = Node(data, ptr, None)
        self.tail = p

        if self.is_empty():
            self.head = self. tail
        else:
            ptr.next = p

        self.no += 1

    def popleft(self) -> Any:
        if self.is_empty():
            print("덱이 비었습니다.")
            print()
            return -1
        else:
            data = self.head.data
            self.head.next.prev = None
            self.head = self.rear = self.head.next
            self.no -= 1
            print(f'popleft: {data}')
            print()
            return data

    def pop(self) -> Any:
        if self.is_empty():
            print("덱이 비었습니다.")
            print()
            return -1
        else:
            data = self.tail.data
            self.tail.prev.next = None
            self.tail = self.front = self.tail.prev
            self.no -= 1
            print(f'pop: {data}')
            print()
            return data

    def get_rear(self) -> None:
        print(f"rear: {self.rear.data}")
        print()

    def get_front(self) -> None:
        print(f"front: {self.front.data}")
        print()

    def print_dequeue(self, reverse = False) -> None:
        if not reverse:
            ptr = self.head
            while ptr is not None:
                print(f'{ptr.data} -> ')
                ptr = ptr.next
            print("End")
            print()
        else:
            ptr = self.tail
            while ptr is not None:
                print(f'{ptr.data} <- ')
                ptr = ptr.prev
            print("End reverse")
            print()


d = Dequeue()
d.append(0)
d.append(1)
d.append(2)
d.appendleft(3)
d.appendleft(4)
d.appendleft(5)
d.print_dequeue()
d.popleft()
d.pop()
d.get_rear()
d.get_front()
len(d)
d.print_dequeue()
d.print_dequeue(True)



```

    5 -> 
    4 -> 
    3 -> 
    0 -> 
    1 -> 
    2 -> 
    End
    
    popleft: 5
    
    pop: 2
    
    rear: 4
    
    front: 1
    
    len: 4
    4 -> 
    3 -> 
    0 -> 
    1 -> 
    End
    
    1 <- 
    0 <- 
    3 <- 
    4 <- 
    End reverse
    

