---
title: Tìm kiếm mù DFS (Depth First Search) và IDS (Iterative Deepening Search)
excerpt_separator: "<!--more-->"
tags:
    - study
    - college
categories:
    - study
comment: true
---

Trước mình đã tìm hiểu về **BFS** và biến thể của nó **UCS**, giờ đến **DFS** và biến thể của nó là **IDS**. Cả 2 thuật toán này cùng với **BFS và UCS** đều thuộc trong phần **tìm kiếm mù** nhé!

<!--more-->

# DFS (Depth First Search)
{: .notice--info}

**Nguyên tắc :** Trong số những nút ở tập biên (cái tập mà chúng ta cho các nút vào để xét) chọn nút xa gốc nhất để xét trước (đâm sâu hết nhánh này rồi sang nhánh kia..)

![DFS Animation]({{'/images/study/algo/dfs.gif'}}){: .align-center}

**Ví dụ:**

![!DFS problem]({{'/images/study/algo/dfs_problem.png'}}){: .align-center}

### Cách trình bày lý thuyết

| STT|Nút được mở rộng|Tập biên O (queue)      |
|----|----------------|------------------------|
|  0| S              |   **A<sub>S</sub>**,**B<sub>S</sub>**,|
|  1|A<sub>S</sub> |**D<sub>A</sub>**,**E<sub>A</sub>** ,B<sub>S</sub>|
|  2 |D<sub>A</sub>|**F<sub>D</sub>**, E<sub>A</sub>,B<sub>S</sub> |
|  3|F<sub>D</sub>| **G<sub>F</sub>**, E<sub>A</sub>,B<sub>S</sub>|
|  4|G<sub>F</sub> |Đích|

**=> Đường đi : S -> A -> D -> F -> G**

**Nhận xét về thuật toán**

* **Đầy đủ :** Không. (Trường hợp cây sâu vô hạn hoặc tìm được đích ở một trong những nhánh đầu thì nó sẽ ko duyệt các nhánh khác.)
* **Thời gian:** *(Time Complexity)* Giả sử mỗi nút có b nút con, m : độ sâu tối đa của cây => Thời gian trường hợp xấu nhất là G ko có trong đồ thị = O(b<sup>m</sup>)
* **Bộ nhớ:** *(Space complexity)* : O(bm) tốt hơn so với BFS (do nó chỉ lưu các phần tử của một nhánh nên bộ nhớ nhỏ hơn.)
* **Tối ưu?**  Tất nhiên là **không rồi**. Vì nó ko đầy đủ (ko duyệt hết tất cả các kết quả có thể có nên nó ko biết đâu là đường đi ngắn nhất.)

### Thực hành Coding

**Giải quyết bài toán DFS này có 2 cách giải :**   **Dùng  đệ quy** hoặc **Dùng Stack** . Về cơ bản chúng như nhau, đệ quy thực chất là hàm chồng lên nhau trong  **Stack hệ thống** (**stack overflow** là lỗi tràn ngăn xếp nguyên nhân phổ biến gây ra lỗi này là khi có quá nhiều hàm đệ quy trong stack, đệ quy ko có điểm dừng hay đệ quy quá sâu.) Ở đây để cho gọn code mình sẽ sử dụng **đệ quy**.

```python

class DFS:
    def __init__(self,dict):
        self.dict = dict

        # Cái này để lưu lại đường đi 
        self.father = {}

        # Dùng mảng để lưu lại các node đã kiểm tra
        self.mark = []

    def findPath(self,node):
        if node == 'S':
            print('S -> ',end='')
        else:
            self.findPath(self.father[node])
            if node == 'G':
                print(f'{node}')
            else:
                print(f'{node} -> ',end='')

    def solve(self,node='S'):
        # Nếu node đó là G rồi thì in đường đi thôi
        if node == 'G':
            self.mark.append('G')
            return
        # Nếu ko phải G thì xét từng phần tử của node đó
        for child in self.dict[node]:
            # Chỉ duyệt các node chưa được duyệt
            if child not in self.mark:
                self.father[child]=node
                self.mark.append(child)
                self.solve(child)
        

    def printPath(self):
        # Nếu G chưa được duyệt => ko có đường đi đến G.
        if 'G' not in self.mark:
            print('No path to G!')
        else:
            self.findPath('G')

""" 
Dùng dict để thể hiện đường đi.

dict['A'] = ['B','C'] => từ A có thể đi đến B và C.

"""
dict = {
    'S': ['A','B'],
    'A': ['D','E'],
    'B': ['C','D'],
    'C': ['H','G'],
    'D': ['F'],
    'E': ['F'],
    'F': ['G'],
    'H': ['G']
}

dfs = DFS(dict)

dfs.solve()
dfs.printPath()

```

**Kết quả khi chạy code sẽ ra:**

![DFS Solved!]({{'/images/study/algo/solve_dfs.png'}}){:. align-center}

## Thuật toán duyệt sâu dần (Interative Deepening Search)
{: .notice--info}

**IDS** cũng có thể gọi là **IDDFS (Interative Deepening Depth First Search)** : một biến thể của **DFS**, nó sinh ra để giải quyết các vấn đề mà **DFS** ko giải quyết được **tính đầy đủ**, **tối ưu (đầy đủ thì chắc sẽ phải tối ưu rồi)** và khắc phục lỗi khi duyệt phải nhánh sâu vô hạn của **DFS**. Bằng cách duyệt các nút ko quá độ sâu cho trước, đi xuống dần dần.

**Ví dụ: Sử dụng ví dụ của DFS để ta thấy sự khác nhau.**

![!IDS problem]({{'/images/study/algo/dfs_problem.png'}}){: .align-center}

### Cách trình bày lý thuyết

| STT|Nút được mở rộng|Tập biên O (queue)      |
|----|----------------|------------------------|
|                   depth=0                         |
|0| S||
|                   depth=1                         |
|0| S|**A<sub>S</sub>**,**B<sub>S</sub>**|
|1|A<sub>S</sub>|B<sub>S</sub>|
|2|B<sub>S</sub>||
|                   depth=2                         |
|0| S|**A<sub>S</sub>**,**B<sub>S</sub>**|
| 1|A<sub>S</sub> |**D<sub>A</sub>**,**E<sub>A</sub>** ,B<sub>S</sub>|
|2|D<sub>A</sub> |E<sub>A</sub> ,B<sub>S</sub>|
|3|E<sub>A</sub>| B<sub>S</sub>|
|4|B<sub>S</sub>|**C<sub>B</sub>**,**D<sub>B</sub>**|
|5|C<sub>B</sub>|D<sub>B</sub>|
|6|D<sub>B</sub>||
|                   depth=3                         |
|0| S|**A<sub>S</sub>**,**B<sub>S</sub>**|
| 1|A<sub>S</sub> |**D<sub>A</sub>**,**E<sub>A</sub>** ,B<sub>S</sub>|
|2|D<sub>A</sub> |**F<sub>D</sub>**,E<sub>A</sub> ,B<sub>S</sub>|
|3|F<sub>D</sub>|E<sub>A</sub> ,B<sub>S</sub>|
|4|E<sub>A</sub>| B<sub>S</sub>|
|5|B<sub>S</sub>|**C<sub>B</sub>**|
|5|C<sub>B</sub>|**G<sub>C</sub>**,**H<sub>C</sub>**|
|6|G<sub>C</sub> |Đích|

**=> Đường đi : S -> B -> C -> G**

**Nhận xét về thuật toán**

* **Đầy đủ :** Có. 
* **Thời gian:** *(Time Complexity)* Giả sử mỗi nút có b nút con, d : độ sâu từ gốc đến G => O(b<sup>d</sup>)
* **Bộ nhớ:** *(Space complexity)* : O(bd).
* **Tối ưu?**  Có. Vì nó duyệt lần lượt hết nên sẽ tìm ra đường đi ngắn nhất.


### Thực hành Coding

```python

class IDS:
    def __init__(self,dict):
        self.dict = dict

        # Cái này để lưu lại đường đi 
        self.father = {}

        # Dùng dict lưu độ sâu các nút.
        self.depth = {'S':0}

        # Độ sâu cho phép tránh bị duyệt vô hạn một nhánh
        self.curDepth = 0
        self.notFound = True

        # Kiểm tra xem còn nút con nào ko để duyệt.
        self.remaining = True

    def findPath(self,node):
        if node == 'S':
            print('S -> ',end='')
        else:
            self.findPath(self.father[node])
            if node == 'G':
                print(f'{node}')
            else:
                print(f'{node} -> ',end='')

    def solve(self):
        # Chạy tới khi tìm thấy hoặc hết node để duyệt
        while self.notFound and self.remaining:
            self.remaining = False
            self.dfs('S')
            self.curDepth+=1


    def dfs(self,node='S'):
        # Tìm được G thì in ra thôi
        if node == 'G':
            self.notFound = False
            return

        # Duyệt từng node con
        for child in self.dict[node]:
            # Cập nhật nếu nút đó được duyệt rồi nhưng với độ sâu sâu hơn hiện tại.
            if child not in self.depth or self.depth[child] > self.depth[node]+1:
                self.depth[child] = self.depth[node]+1
                self.father[child]=node
 
            # Nếu chưa chạm độ sâu giới hạn tiếp tục đào sâu
            if self.depth[child] <= self.curDepth:
                self.dfs(child)
            
            # Vẫn còn nút có độ sâu lớn hơn độ sâu giới hạn
            if self.depth[child] > self.curDepth:
                # Ta vẫn tiếp tục phải đào.
                self.remaining = True


    def printPath(self):
        # Nếu G chưa được duyệt => ko có đường đi đến G.
        if self.notFound:
            print('No path to G!')
        else:
            self.findPath('G')

""" 
Dùng dict để thể hiện đường đi.

dict['A'] = ['B','C'] => từ A có thể đi đến B và C.

"""
dict = {
    'S': ['A','B'],
    'A': ['D','E'],
    'B': ['C','D'],
    'C': ['H','G'],
    'D': ['F'],
    'E': ['F'],
    'F': ['G'],
    'H': ['G']
}

ids = IDS(dict)

ids.solve()
ids.printPath()


```

#### Đây là kết quả khi chạy :

![UCS Solved]({{'/images/study/algo/solve_ids.png'}}){: .align-center}


***Rõ ràng cùng một bài toán trên nhưng IDS cho kết quả tối ưu hơn (ko quan tâm tới khoảng cách giữa các nút như BFS) và lưu ý rằng trong IDS vẫn thêm lại nhưng nút đã duyệt rồi, còn DFS thì ko (DFS chỉ thêm lại các nút nằm trong Stack nhưng chưa được duyệt) vì nó làm thay đổi nghiệm của bài toán.***

**Trước mình cũng thắc mắc :** ***"Sao IDS có vẻ giống BFS thế nhỉ?"*** Nhưng tìm hiểu kĩ hơn thì thấy rằng **IDS** dựa trên **DFS** nên nó chỉ lưu trữ số node trên một nhánh thôi => Space Complexity ( độ phức tạp bộ nhớ) =  O(bd), còn Time Complexity (thời gian chạy) = O(b<sup>d</sup>) giống như **BFS**, trông có vẻ **IDS** chạy lâu hơn (vì có thêm vòng lặp lồng nhau) nhưng mà ta thấy các đỉnh cành xa gốc thì số lần lặp lại sẽ ít hơn, nút gốc sẽ lặp lại d+1 lần, còn nút ở độ sâu d sẽ chỉ lặp lại một lần => (d+1) + db + (d-1)b<sup>2</sup>+...+b<sup>d</sup> = O(b<sup>d</sup>).

## Tổng kết 4 thuật toán trong tìm kiếm mù
{: .notice--info}

|  |  BFS              | UCS |DFS|IDS|
|-----------------|----|-----|---|--|
| Đầy đủ?         | Có  | Có | Không                                                          | Có  |
| Tối ưu?         | Có  | Có                                                | Không           | Có  |
| Time Complexity| O(b<sup>d</sup>)        | O(<sup>C/e</sup>)| O(b<sup>m</sup>) | O(b<sup>d</sup>) |
| Space Complexity|O(b<sup>d</sup>)   |  O(<sup>C/e</sup>)| O(bm) |O(bd)|

**Từ đây ta có kết luận :**
* Dùng **BFS** khi độ phân nhánh b nhỏ.
* Dùng **DFS** khi biết trước độ sâu tối đa (m) khi đó bạn có thể chỉnh sửa code để tìm ra tất cả các đường đi.
* **UCS** khi có đề cập giá thành đường đi.
* **IDS** khi độ sâu tối đa (m) lớn.


