---
layout: single

title: CS50 2019 강의보충

categories:
  - CS50

tag: [C, C언어, 포인터, 배열포인터, 포인터 배열, 이중포인터]

toc: true

---

## CS50 강의 외 보충
>배경
>
>CS50 강의를 듣고 난뒤에 부족했던 부분을 보충학습한 내용입니다.  

### 다양한 포인터   

#### Void 포인터
+ 타입을 명시하지 않은 포인터입니다. 모든 타입을 가르킬 수 있으나 포인터 연산이나 메모리 참조와 같은 작업은 할 수 없습니다. 사용할 때에는 사용하고자 하는 타입으로 명시적 선언 후에 사용해야 합니다.   
  
#### NULL 포인터
+ 0이나 NULL을 대입하여 초기화한 아무것도 가리키지 않는 포인터라는 의미입니다.   
  
#### 이중 포인터
+ 이중 포인터(포인터의 포인터): 포인터 변수의 주소를 가르키는 변수를 이중 포인터라고 합니다.
    ```c
    int num = 10;              // 변수 선언

    int* ptr_num = &num;       // 포인터 선언

    int** pptr_num = &ptr_num; // 포인터의 포인터 선언  

    printf(num); // 10
 
    printf(*ptr_num); //10

    printf(**pptr_num);  //10
    ```
  이중 포인터는 다차원 배열에서 활용할 수 있습니다.    


### 포인터 배열
#### 포인터 배열(Array of pointers)
포인터들을 원소로 가지는 배열을 의미힙나다.<br>
만약 포인터를 원소로 가지는 배열이 있고, 각 배열의 원소인 포인터들이 또 다른 배열을 가리킨다면 해당 포인터 배열은 다차원 배열의 형태입니다.
#### 다차원 배열
배열의 요소로 배열을 가지는 배열입니다.
```c
type var_name[row_len][col_len]
```
같이 선언하며, 메모리는 입체적으로 존재하는게 아닌 선형적인 공간이므로 사실상 col개의 변수가 row개 만큼 반복되는 선형 구조로 저장됩니다. 그래서 같은 주소의 메모리를 var_name[r][c]로도 접근 할 수 있고, var_name[r*c]로도 접근할 수 있습니다.
<center>
  <img src="http://www.tcpschool.com/lectures/img_c_twodimensional_array.png" width="80%">
</center>
<center>
  <a href="http://www.tcpschool.com/c/c_array_twoDimensional"> 출처: tcpschool</a>
</center>

#### 배열 포인터
베열을 가르킬 수 있는 포인터를 의미합니다. 이전 글에서 살펴 보았듯, 배열은 포인터로도 접근이 가능하며 이때 배열의 이름 자체는 배열이 시작하는 원소의 주소를 가지고 있습니다. 배열의 이름이 갖는 주소를 저장한 포인터를 배열 포인터라고 합니다.

<center>
  <img src="https://user-images.githubusercontent.com/94548914/175514285-45da13c9-4e79-4a71-a1dd-f30c6150d6e4.png" width="80%">
</center>
<center>
  <a href="http://www.tcpschool.com/c/c_pointerArray_arrayPointer"> 출처: tcpschool</a>
</center>

```c
int arr[2][3] =             // 배열의 선언

{

    {10, 20, 30},

    {40, 50, 60}

};

int (*pArr)[3] = arr;       // 배열 포인터의 선언  

 

printf("%d\n", arr[1][1]);  // 배열 이름으로 참조

printf("%d\n", pArr[1][1]); // 배열 포인터로 참조
```
