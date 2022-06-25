---
layout: single

title: 스택과 큐 연결리스트로 구현 with python
categories:
  - Data-structure

tag: [LinkedList, 스택, 큐, python]

toc: true

---
스택


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


큐


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

