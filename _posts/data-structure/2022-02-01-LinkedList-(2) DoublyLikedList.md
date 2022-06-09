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
from typing import Any,Type

# 노드 클래스 선언
class Node:
     
    def __init__(self,data:Any,next:Node=None,prev:Node=None)->None:
         
         # 전방 탐색을 위해 prev필드 추가
        self.data = data
        self.next = next
        self.prev = prev

 # 이중연결리스트 클래스 선언
class DoublyLinkedList:

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
        
         # head 부터 시작해서 노드가 none이 될때까지 next
        p = self.head
        while p is not None:

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
        
        self.head = Node(data,self.head)
        self.no+=1
  
         # head.next가 존재 할시 prev가 self.head를 가르키도록 할당
        if self.head.next is not None:
            self.head.next.prev = self.head


     # 꼬리 노드추가 함수 
    def add_last(self,data:Any)->None:

         # 리스트가 비었다면 머리추가와 동일
        if self.head is None:
            self.add_first(data)

        else:

             # p.next가 None 즉 마지막 노드까지 스캔
            p = self.head
            while p.next is not None:
                p = p.next

             # 마지막노드 p의 next에 data를 data로 prev를 p로 갖는 노드 추가
             # cur, no 업데이트
            p.next = self.cur = Node(data,None,p)
            self.no+=1


     # 머리노드 삭제 함수
    def remove_first(self)->Any:

         # 리스트가 비었다면 삭제 실패 False 반환
        if self.head is None:
            return False

         # head가 None이 아니라면 head를 삭제
         # cur, no 업데이트 후 True 반환            
        else:
            self.head = self.cur = self.head.next
            self.head.prev = None
            self.no-=1
            return True


     # 꼬리노드 삭제 함수
    def remove_last(self)->bool:

         # 리스트가 비었다면 false 반환
        if self.head is None:
            return False

         # p.next가 none 즉 마지막 노드까지 스캔
         # 단. pre에 이전 p를 저장하며 진행
        else:
            p = self.head
            pre = self.head
            while p.next is not None:
                pre = p
                p = p.next
    
             # 마지막 노드 p의 전 노드 pre의 next를 None으로 업데이트
            pre.next = None

             # p의 prev에 None 할당
            p.prev = None
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
            pre = self.head

             # p의 next와 x가 일치할때 까지 진행
            while p.next is not x:
                pre = p
                p = p.next

                 # 마지막 노드까지 없다면 false 반환
                if p is None:
                    return False

             # p의 next가 x이므로 p.next에 x의 next 대입
            p.next = x.next

             # p의 next가 None이 아니라면 prev에 pre 할당
            if p.next is not None:
                p.next.prev = pre
            self.cur = p
            self.no-=1

    def remove_cur_node(self)->None:
        self.remove(self.cur)

    def print_cur_node(self)->None:

        if self.cur is None: print('None')
        else: print(self.cur.data)

    def print(self)->None:
       
        p=self.head
        while p is not None:
            print(p.data)
            p=p.next

    def print_reverse(self)->None:
        
        p=self.head
        while p.next is not None:
            p=p.next
        while p is not None:
            print(p.data)
            p=p.prev

    def clear(self)->None:

        while self.head is not None:
            self.remove_first()
        self.cur = None
        self.no


x=DoublyLinkedList()
x.add_first(1)
x.add_first(2)
x.add_first(3)
x.add_last(4)
x.add_last(5)
x.search(1)
x.remove_cur_node()
x.add_last(9)
x.add_last(10)
x.remove_last()
x.add_first(8)
x.add_first(7)
x.remove_first()

x.print()
print(f'len:{len(x)}')
```

    8
    3
    2
    4
    5
    9
    len:6

