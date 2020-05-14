---
title: Tìm kiếm mù BFS (Breath First Search) và UCS (Uniform-cost Search)
excerpt_separator: "<!--more-->"
tags:
    - study
    - college
categories:
    - study
comment: true
---
Trong môn **"Nhập môn trí tuệ nhân tạo"** thì tìm kiếm là chương đầu tiên ta được học, nhưng ko chỉ riêng **Trí tuệ nhân tạo** mà hầu như tìm kiếm (search) có mặt ở khắp nơi trong ngành CNTT. Nhằm tìm hiểu sâu hơn về thuật toán, code,.. và *lấp đầy chỗ trống* không may được tạo ra do tính lười học ko chịu nghe giảng ở trên lớp nên một số bài post như này được ra đời.

<!--more-->


# Tìm kiếm

Tìm kiếm theo mình biết là chia làm 2 loại:

* **Tìm kiếm mù (tìm kiếm ko có thông tin)**

* **Tìm kiếm có thông tin**

Ở bài này mình chỉ trình bày về ***tìm kiếm mù***, **BFS** và **UCS (biến thể của BFS)** thôi. Còn tìm kiếm có thông tin thì sẽ có ở một bài viết khác. 

## Tìm kiếm mù

**Các thuật toán bên dưới mặc định là tìm đường đi từ S -> G. Các bạn có thể sửa code để tìm đường đi như ý muốn.**

**Bỏ qua một số thứ râu ria ta vào luôn tổng quan các bước để giải một bài toán tìm kiếm :**

1. Tập hữu hạn các trạng **trạng thái** có thể: Q
2. Tập các trạng thái xuất phát: S⊆Q
3. **Hành động** hay **hàm nối tiếp** hay **toán tử *P(x)***, là tập các trạng thái nhận đuợc từ **trạng thái x** do kết quả thực hiện hành động hay toán tử.
4. Xác định đích:
    - **Tường minh**, cho bởi tập đích G ⊆Q
    - **Không tường minh**, cho bởi một số điều kiện.
5. **Giá thành đường đi**: *Ví dụ:*,tổng khoảng cách, số lượng hành động,...

**Lời giải cho bài toán tìm kiếm** là *chuỗi hành động* cho phép di chuyển từ trạng thái xuất phát đến trạng thái đích.

**Các tiêu chuẩn đánh giá thuật toán tìm kiếm :**

* **Độ phức tạp tính toán:**
    - Khối lượng tính toán cần thực hiện để tìm ra lời giải.
    - Số lượng trạng thái cần xem xét trước khi tìm ra lời giải.
* **Yêu cầu bộ nhớ**
* **Tính đầy đủ**
* **Tính tối ưu :** Nếu bài toán có nhiều lời giải thì thuật toán có thể tìm ra lời giải tốt nhất ko?

> Đọc một hồi hết đoạn trên thì mình nhớ ra lý do vì sao hôm đấy mình lại ko chịu nghe giảng rồi, hóa ra là mình thiếp đi lúc nào ko hay...




## Thuật toán tìm kiếm theo chiều rộng - BFS
{: .notice--info}

**Nguyên tắc:** trong số những nút biên, lựa chọn nút gần gốc nhất để mở rộng (nôm na là đi hết tầng này rồi mới tới tầng sau kiểu kiểu vây...)

![BFS animation]({{'/images/bfs.gif'}}){: .align-center}

Ý tưởng để giải bài toán này là ta sẽ dùng một hàng đợi (queue) để duyệt các nút và phải lưu lại đường đi ngắn nhất.

***Lưu ý*** : Ko thêm những nút đã duyệt rồi hoặc đang có trong queue để tránh bị lặp vô hạn. 

**Ví dụ**

![!BFS problem]({{'/images/bfs_problem.png'}}){: .align-center}

### Cách trình bày lý thuyết

| STT|Nút được mở rộng|Tập biên O (queue)      |
|----|----------------|------------------------|
|  0| S              |   **A<sub>S</sub>**|
|  1|A<sub>S</sub> |**B<sub>A</sub>**, **C<sub>A</sub>** |
|  2 |B<sub>A</sub>|C<sub>A</sub>, **D<sub>B</sub>** |
|  3|C<sub>A</sub>| D<sub>B</sub>, **G<sub>C</sub>**|
|  4|D<sub>B</sub> |G<sub>C</sub>|
|  5|G<sub>C</sub>  | Đích |

**=> Đường đi : S -> A -> C -> G**

**Nhận xét về thuật toán**

* **Đầy đủ :** Có *(nó duyệt qua các nút trong từng hàng, nếu số nút trong hàng là hữu hạn thì đầy đủ.)*
* **Thời gian:** *(Time Complexity)* Giả sử mỗi nút có b nút con, d : độ sâu của cây => Thời gian duyệt xong : 1+b+b<sup>2</sup>+b<sup>3</sup>+...+b<sup>d</sup> = O(b<sup>d</sup>)
* **Bộ nhớ:** *(Space complexity)* O(b<sup>d</sup>)
* **Tối ưu?** Có (nếu không xét đến khoảng cách giữa các nút hay khoảng cách các nút bằng nhau thì nó sẽ cho kết quả tốt nhất.)

### Thực hành Coding

Ta đã biết biểu diễn một **đồ thị (graph)** bằng **ma trận (matrix)** nhưng ở đây mình sẽ dùng **Dictionary** trong **Python** để biểu diễn đồ thị trên. **Here is the code** :

```python
class BFS:
    def __init__(self,dict):
        self.dict = dict
        # Cái này để lưu lại đường đi 
        self.father = {}
        # Dùng mảng để lưu lại các node đã kiểm tra
        self.mark = []
        # Hàng đợi để xét các node.
        self.queue = []

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
        # Thêm phần tử đầu vào queue để bắt đầu vòng lặp
        self.queue.append('S')
        # Duyệt đến khi nào ko còn node nào trong queue thì thôi.
        while len(self.queue):
            # Lấy phần từ đầu ra xét
            node = self.queue.pop(0)
            # Nếu là G rồi thì in ra đường đi thôi.
            if node == 'G':
                break
            else:
                # Thêm các node con của node đang xét
                for child in self.dict[node]:
                    # Chỉ thêm các node chưa được duyệt
                    if child not in self.mark and child not in self.queue:
                        self.father[child] = node
                        self.queue.append(child)
                        self.mark.append(child)

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
    'S': ['A'],
    'A': ['B','C'],
    'B': ['D'],
    'C': ['D','G'],
    'D': ['G']
}

BFS = BFS(dict)

BFS.solve()


```



#### Kết quả khi chạy code

![BFS Solved!]({{'/images/solve_bfs.png'}}){:. align-center}


## Thuật toán duyệt theo giá thành thống nhất (Uniform-cost Search)
{: .notice--info}

Đây là một biến thể của **BFS** cơ chế hoạt động cũng như **BFS** xét hết hàng này đến hàng tiếp, nhưng tại sao ta lại cần một biến thể của **BFS** ở đây? Như đã nói ở phần trên **BFS** chỉ tối ưu khi ta không quan tâm đến khoảng cách các nút hoặc khoảng cách các nút bằng nhau. 

**Ví dụ** : Nếu bạn tìm đường từ nhà đến trường bạn phải đi qua một vài trạm xe buýt, nếu bạn ~~**không quan tâm khoảng cách**~~  giữa các trạm xe buýt thì **BFS** sẽ làm tốt nhiệm vụ của nó. (**BFS** sẽ tìm cho bạn đường mà bạn phải đổi tuyến ít nhất nhưng nó ko phải đường đi ngắn nhất vì dù đi qua ít trạm nhưng khoảng cách giữa 2 trạm lại dài hơn so với con đường khác có 3 trạm xe.) => **UCS** ra đời để tìm cho bạn đường đi ngắn nhất.

**BFS -> đường đi ít trạm xe nhất (đổi xe buýt ít nhất.)**

**UCS -> đường đi ngắn nhất (có thể phải đổi xe buýt nhiều hơn.)**

**Về cơ bản thì UCS giống như  BFS sẽ duyệt các node theo hàng nhưng vì có thêm giá (cost) giữa các nút nên nó có một số thay đổi :**

* Trong ***queue*** lấy node có giá thấp nhất để xét trước.
* Nếu gặp một nút đã xét rồi nếu nút đó có giá nhỏ hơn lần xét trước ta thêm lại nút đó vào **queue**.
* Nếu nút đó đang có trong ***queue*** thì phải so sánh giá rồi cập nhật nút đó với giá thấp nhất. 


**Ví dụ:** Tiếp tục với bài toán của **BFS** nhưng giờ ta có khoảng cách giữa cách đường đi.

![UCS Problem]({{'/images/ucs_problem.png'}}){: .align-center}

### Cách trình bày lý thuyết

| STT|Nút được mở rộng|Tập biên O (queue)      |
|----|----------------|------------------------|
|  0| S              |   **A<sub>S</sub>(30)**|
|  1|A<sub>S</sub> |**B<sub>A</sub>(50)**, **C<sub>A</sub>(60)** |
|  2 |B<sub>A</sub>|C<sub>A</sub>(60), **D<sub>B</sub>(60)** |
|  3|C<sub>A</sub>| D<sub>B</sub>(60), **G<sub>C</sub>(95)**|
|  4|D<sub>B</sub> |G<sub>D</sub>(75)**(*)**|
|  5|G<sub>D</sub>  | Đích |

<b>( * ) Lưu ý :</b>  Xét **D<sub>B</sub>** có đường đến nút **G**, mà nút **G** đã ở trong ***queue*** rồi ta so sánh giá (cost) với **G** trong ***queue***. Đuờng đi qua **D -> G** (tổng là 75) nhỏ hơn so với đường đi qua **C -> G** (95) nên ta cập nhật giá (cost) và nút cha của **G**.

**=> Đường đi : S -> A -> B -> D -> G**

### Thực hành Coding

**Code dựa trên BFS nhưng vì thêm giá thành nên nó hơi phức tạp hơi chút. Here is the code:**

```python
class UCS:
    def __init__(self,dict):
        self.dict = dict
        # Cái này để lưu lại đường đi 
        self.father = {}
        # Dùng mảng để lưu lại các node đã kiểm tra
        self.mark = []
        # Hàng đợi để xét các node.
        self.queue = []
        # Cập nhật giá (cost) cho các nút.
        self.cost = {}

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
        # Thêm phần tử đầu vào queue để bắt đầu vòng lặp
        self.queue.append('S')
        self.cost['S'] = 0
        # Duyệt đến khi nào ko còn node nào trong queue thì thôi.
        while len(self.queue):
            # Tìm phần tử có giá nhỏ nhất
            node = self.queue[0]
            for ele in self.queue:
                if self.cost[ele] < self.cost[node]:
                    node = ele
            # Bỏ phần tử đó ra khỏi queue
            self.queue.remove(node)
            # Nếu là G rồi thì in ra đường đi thôi.
            if node == 'G':
                break
            else:
                # Thêm các node con của node đang xét
                for child in self.dict[node]:
                    # Nếu node đó chưa được duyệt
                    if child not in self.mark:
                        # Thêm vào queue cập nhật nút cha và giá
                        self.queue.append(child)
                        self.father[child]=node
                        self.cost[child] = self.cost[node] + self.dict[node][child]
                        self.mark.append(child)
                    else:
                        # Nếu nút đã xét rồi thì kiểm tra xem giá của nó có nhỏ hơn ko?
                        if self.cost[node] + self.dict[node][child] < self.cost[child]:
                            # Giá nhỏ hơn và ko nằm trong queue thì thêm nó vào queue
                            if child not in self.queue:
                                self.queue.append(child)
                            # Cập nhật lại giá và nút cha cho nút đó
                            self.cost[child] = self.cost[node] + self.dict[node][child] 
                            self.father[child] = node

        # Nếu G chưa được duyệt => ko có đường đi đến G.
        if 'G' not in self.mark:
            print('No path to G!')
        else:
            self.findPath('G')
            print(f'Minimum Price : {self.cost["G"]}')

""" 
Dùng dict để thể hiện đường đi.

dict['A'] = {'B':20,'C':30} => chi phí đi từ A đi đến B là 20 và đi đến C là 30.
"""


dict = {
    'S': {'A':30},
    'A': {'B':20,'C':30},
    'B': {'D':10},
    'C': {'D':15,'G':35},
    'D': {'G':15}
}

ucs = UCS(dict)

ucs.solve()

```


#### Đây là kết quả khi chạy :

![UCS Solved]({{'/images/solve_ucs.png'}}){: .align-center}

#### Đây là góc nhìn của mình (sinh viên) về môn học , nên không có ý dạy bảo ai cả.
{: .notice--warning}

#### Phần Coding là do mình tự thêm vào (tài liệu "Nhập môn AI" ko có code), trong môn "Toán Rời Rạc" biễu diễn graph bằng ma trận các node là số (trông khá là rối) nên mình sử dụng Dictionary (các node là chữ) cho trực quan. Code là do mình tự code nên có thể có sai sót! Báo cho mình biết nếu bạn tìm thấy lỗi sai, mình sẽ sửa sớm nhất có thể.
{: .notice--success}