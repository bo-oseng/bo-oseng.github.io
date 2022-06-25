---
layout: single

title: LinkedList- (2) DoublyLinkedList with python
categories:
  - Data-structure

tag: [DoublyLinkedList, 자료구조, python]

toc: true

---

# 연결리스트

단일 연결리스트는 앞서 [(1) SingleLinkedList](https://bo-oseng.github.io/data-structure/LinkedList-(1)-SingleLinkedList/) 에서 다뤄봤고 이번에는 전방 탐색도 가능한 DoublyLinkedList 이중연결리스트를 정리했다.

[이중 연결리스트 자세한 설명](https://namu.wiki/w/%EC%97%B0%EA%B2%B0%20%EB%A6%AC%EC%8A%A4%ED%8A%B8?from=%EC%97%B0%EA%B2%B0%EB%A6%AC%EC%8A%A4%ED%8A%B8#s-3.2)

## (2) DoublyLinkedList


```python
from typing import Any, Type

class Node:
    def __init__(self, data: Any, prev = None, next = None) -> None:
        self.data = data
        self.prev = prev
        self.next = next


class DoublyLinkedList:
    def __init__(self) -> None:
        self.no = 0
        self.head = self.tail = None
        self.cur = None

    def __len__(self):
        return self.no

    def add_first(self, data: Any):
        self.no += 1
        if self.head is not None:
            ptr = self.head
            self.head = self.cur = Node(data)
            self.head.next = ptr
            ptr.prev = self.head
        else:
            self.head = Node(data)
            self.tail = self.head

    def add_last(self, data: Any):
        self.no += 1
        if self.tail is not None:
            ptr = self.tail
            self.tail = self.cur = Node(data)
            self.tail.prev = ptr
            ptr.next = self.tail
        else:
            self.tail = Node(data)
            self.head = self.tail

    def remove_first(self):
        if self.head is None:
            return False
        else:
            self.head = self.cur = self.head.next
            self.head.prev = None
            self.no -= 1
            return True

    def remove_last(self):
        if self.tail is None:
            return False
        else:
            self.tail = self.cur = self.tail.prev
            self.tail.next = None
            self.no -= 1
            return True

    def print(self):
        ptr = self.head
        while ptr is not None:
            print(f'{ptr.data} ->')
            ptr = ptr.next

    def reverse_print(self):
        ptr = self.tail
        while ptr is not None:
            print(f'{ptr.data} ->')
            ptr = ptr.prev



dlst = DoublyLinkedList()
dlst.add_first(2)
dlst.add_first(3)
dlst.add_first(4)
dlst.add_first(5)
dlst.add_last(6)
dlst.add_last(7)
dlst.add_last(8)
dlst.add_last(9)

dlst.remove_first()
dlst.remove_last()
dlst.reverse_print()
print()
dlst.print()
print()
print(len(dlst))

```

    8 ->
    7 ->
    6 ->
    2 ->
    3 ->
    4 ->
    
    4 ->
    3 ->
    2 ->
    6 ->
    7 ->
    8 ->
    
    6

