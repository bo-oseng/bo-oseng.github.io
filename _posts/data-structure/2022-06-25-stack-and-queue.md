---
layout: single

title: 스택과 큐 연결리스트로 구현 with python

categories:
  - Data-structure

tag: [LinkedList, 연결리스트, 스택, 큐, python]

toc: true

---
## 스택과 큐
스택과 큐를 [연결리스트]()를 활용해 직접 구현해 보았다.
## 스택
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


## 큐
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

