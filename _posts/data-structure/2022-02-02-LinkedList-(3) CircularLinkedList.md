---
layout: single

title: LinkedList- (3) CircularLinkedList with python
categories:
  - Data-structure

tag: [CircularLinkedList, 자료구조, python]

toc: true

---
# 연결리스트

단일 연결리스트 그리고 이중 연결리스트에 이어서 원형 연결리스트를 다루어보았다.

[원형 연결 리스트](https://namu.wiki/w/%EC%97%B0%EA%B2%B0%20%EB%A6%AC%EC%8A%A4%ED%8A%B8?from=%EC%97%B0%EA%B2%B0%EB%A6%AC%EC%8A%A4%ED%8A%B8#s-3.3)


+ [(1) SingleLinkedList](https://bo-oseng.github.io/data-structure/LinkedList-(1)-SingleLinkedList/)
+ [(2) DoublyLinkedList](https://bo-oseng.github.io/data-structure/LinkedList-(2)-DoublyLikedList/)


```python
from typing import Any,Type

# 노드 클래스 선언
class Node:
     
    def __init__(self,data:Any,next=None):

        self.data = data
        self.next = next

# 원형링크드리스트 클래스 선언
class CircularLinkedList:

     # 시작 head, 현재 cur, 갯수 no 선언
    def __init__(self):

        self.head = None
        self.cur = None
        self.no = 0


     # dunder 함수로 len구현
    def __len__(self):

        return self.no


     # 원하는 데이터를 찾는 함수
    def search(self,data:Any)->int:

         # data의 인덱스를 나타낼 변수
        cnt = 0
   
         # self.head에 있을경우
        p = self.head
        if p.data == data:
            self.cur = p   
            return cnt

         # head 부터 시작해서 노드가 self.head가 될때까지 next
        while p.next is not self.head:

            # 스캔중 data 발견시 return cnt 
            if p.data == data:
                self.cur = p    
                return cnt
            p = p.next
            cnt+=1
   
         # 검색 실패시 return -1
        return -1


     # 머리에 노드추가 함수
    def add_first(self,data:Any)->None:

         # self.head가 비어있을 경우
        if self.head is not None:
            p = self.head
            while p.next is not self.head:
                p = p.next
            self.head = self.cur = p.next = Node(data,self.head)

         # self.head.next를 self.head로 업데이트
        else:
            self.head = self.cur = Node(data,self.head)
            self.head.next = self.head
        self.no+=1


     # 꼬리 노드추가 함수 
    def add_last(self,data:Any)->None:

         # 리스트가 비었다면 머리추가와 동일
        if self.head is None:
            self.add_first(data)

        else:

             # p.next가 self.head 즉 마지막 노드까지 스캔
            p = self.head
            while p.next is not self.head:
                p = p.next

             # 마지막노드 p의 next에 data를 data로 갖는 노드 추가
             # cur, no 업데이트
            p.next = self.cur = Node(data)
            p.next.next = self.head
            self.no+=1


     # 머리노드 삭제 함수
    def remove_first(self)->bool:

         # 리스트가 비었다면 삭제 실패 False 반환
        if self.head is None:
            return False

         # head가 None이 아니라면 head를 삭제
         # cur, no 업데이트 후 True 반환            
        else:
            if self.head.next is self.head:
                self.head = self.cur = None
            else:
                self.head = self.cur = self.head.next
            self.no-=1
            return True


     # 꼬리노드 삭제 함수
    def remove_last(self)->bool:

         # 리스트가 비었다면 false 반환
        if self.head is None:
            return False

         # p.next가 self.head 즉 마지막 노드까지 스캔
         # 단. pre에 이전 p를 저장하며 진행
        else:
            p = self.head
            pre = self.head
            while p.next is not self.head:
                pre = p
                p = p.next
    
             # 마지막 노드 p의 전 노드 pre의 next를 self.head으로 업데이트
            pre.next = self.head
            self.cur = pre
            self.no-=1
            return True

     # 노드x를 삭제하는 함수
    def remove(self,x:Node)->None:
  
         # 리스트가 비었다면 false 반환  
        if self.head is None:
            return False
        else:
            p = self.head

             # p의 next와 x가 일치할때 까지 진행
            while p.next is not x:
                p = p.next

                 # 마지막 노드까지 없다면 false 반환
                if p.next is self.head:
                    return False

             # p의 next가 x이므로 p.next에 x의 next 대입
            p.next = x.next
            self.cur = p
            self.no-=1

    def remove_cur_nod(self)->None:
        self.remove(self.cur)

    def print_cur_node(self)->None:

        if self.cur is None: print('None')
        else: print(self.cur.data)

    def print(self)->None:
        p = self.head
        if p.next is self.head:
            print(p.data)
        else:
            while p.next is not self.head:
                print(p.data)
                p = p.next

    def clear(self)->None:

        while self.head is not None:
            self.remove_first()
        self.cur = None
        self.no

x = CircularLinkedList()

x.add_first(1)
x.remove_first()
x.add_first(2)
x.remove_first
x.add_first(3)
x.add_first(4)
x.add_last(5)
x.add_last(6)

x.print()
```

    4
    3
    2
    5

