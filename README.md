# FizzBuzz-python
The   FizzBuzz   problem was presented as the lowest level of comprehension required to illustrate adequacy.

Write a program that prints the integers from 1 to 100.

But:
- For multiples of three print “Fizz” instead of the number
- For the multiples of five print “Buzz”.
- For numbers which are multiples of both three and five print “FizzBuzz”.



Đề bài: 
Viết một chương trình in ra màn hình các số từ 1 đến 100. Tuy nhiên: 
- Nếu số đó chia hết cho 3, in ra "Fizz" thay cho số 
- Nếu số đó chia hết cho 5, in ra "Buzz" thay cho số 
- Nếu số đó chia hết cho cả 3 và 5 thì in ra "FizzBuzz"

------
Một bài toán tưởng như vô hại và khá dễ dàng chỉ với vòng lặp for và vài câu lệnh if.

Tuy nhiên giải thuật bên trong nó lại "phức tạp" một cách khó chịu.
Bài toán này được khá nhiều công ty sử dụng để test đầu vào với các ứng viên. Và khá nhiều ứng viên "non tay" đã chấp nhận bỏ cuộc chỉ ngay sau 5-10'.

Bài toán này có một số điều thú vị ẩn dấu bên trong.
1. Đầu tiên là vấn đề liên quan đến logic đọc hiểu

Theo logic thông thường thì chúng ta phần lớn sẽ viết code như này:

```Python
def fizz_buzz():
    for index in range(1, 101):
        if index % 3 == 0:
            print("FIZZ")
        elif index % 5 == 0:
            print("BUZZ")
        elif index % 3 and index % 5 == 0:
            print("FIZZ BUZZ")
        else:
            print(index)
```

Và chắc mẩm, rung chân rung đùi là đã đúng. Khi chạy thì kết quả sẽ ra.....

```
1
2
FIZZ
4
BUZZ
FIZZ
7
8
FIZZ
BUZZ
11
FIZZ
13
14
FIZZ
16
17
FIZZ
```

Nhìn kết quả, có lẽ chúng ta sẽ cười cười vì đã làm đúng rồi, ơ nhưng mà.... có gì đó sai sai. 

Sau số 14 là 15, 15 chia hết cho 3 và 5, nhưng sao nó lại ra FIZZ nhỉ ?

15 chia hết cho 3 --> in ra FIZZ và... dừng lại (huhu)

> Nếu một ứng viên không đủ bình tĩnh, thì đây sẽ là điểm dừng của ứng viên. 
> Vò đầu, bứt tai vì...không hiểu mình sai ở đâu.

Một cái "trap" nhỏ nhỏ, xinh xinh nhỉ :D

Thôi, quay lại bài toán, code đúng sẽ như này: 

```Python
def fizz_buzz_simple():
    for index in range(1, 101):
        if index % 5 == 0 and index % 3 == 0:
            print("FIZZ BUZZ")
        elif index % 3 == 0:
            print("FIZZ")
        elif index % 5 == 0:
            print("BUZZ")
        else:
            print(index)
```

Kết quả đã đúng:
```
1
2
Fizz
4
Buzz
Fizz
7
8
Fizz
Buzz
11
Fizz
13
14
FizzBuzz
16
17
Fizz
19
Buzz
Fizz
22
```

2. Vấn đề tiếp theo là tối ưu bài toán.

Sau khi có đáp án simple thì đến tối ưu bài toán. Câu hỏi thường gặp là "bạn có cách nào tối ưu bài toán này không?"

> Câu hỏi này sẽ là mở đầu cho câu hỏi liên quan đến độ phức tạp thuật toán...

Các ứng viên thường sẽ nhìn quanh nhìn quẩn bài toán và ... 'em không biết'

Thực ra thì khá đơn giản `index % 5 == 0 and index % 3 == 0`, ở đây có từ khoá `and` -> một lần làm phép toán logic. Có cách nào bỏ từ khoá `and` này không ? 

`3 * 5 = 15` --> ta có thể thay thế `index % 5 == 0 and index % 3 == 0` thành `index % 15 == 0`

```Python
def fizz_buzz_optimal():
    for index in range(1, 101):
        if index % 15 == 0:
            print("FIZZ BUZZ")
        elif index % 3 == 0:
            print("FIZZ")
        elif index % 5 == 0:
            print("BUZZ")
        else:
            print(index)
```

Thử thời gian thực hiện của 2 phép toán.

```Python
import timeit
start_simple = timeit.default_timer()
fizz_buzz_simple()
stop_simple = timeit.default_timer()

start_optimal = timeit.default_timer()
fizz_buzz_optimal()
stop_optimal = timeit.default_timer()


print('Time-simple: ', stop_simple - start_simple)  
print('Time-optimal: ', stop_optimal - start_optimal)  

```


>Time-simple:  0.002665010999862716

>Time-optimal:  0.0029439630000069883

WHAT???? Sao optimal lại nhiều hơn Simple được nhỉ 


Note: Lần chạy đầu tiên có thể optimal sẽ lâu hơn cả simple, lần chạy tiếp theo sẽ thấy sự khác biệt

>Time-simple:  0.0028098370000861905
>Time-optimal:  0.002432777000194619


Vậy tăng thêm vòng lặp cho phép toán để xem có gì thay đổi không nhé.

```Python
import timeit
start_simple = timeit.default_timer()
for _ in range(0, 1000):
    fizz_buzz_simple()
stop_simple = timeit.default_timer()

start_optimal = timeit.default_timer()
for _ in range(0, 1000):
    fizz_buzz_optimal()
stop_optimal = timeit.default_timer()
```

```Python
print('Time-simple: ', stop_simple - start_simple)  
print('Time-optimal: ', stop_optimal - start_optimal)  

```

>Time-simple:  10.763847636000037

>Time-optimal:  10.718760332999864

Sự khác biệt đã lớn hơn rồi.


3. One-line code
   Trong python-code có điều thú vị liên quan đến one-line code. Và dưới đây là 1line với bài toán fizzbuzz

```Python
for i in range(1,101): print("Fizz"*(i%3==0) + "Buzz"*(i%5==0) or i)
```


Thời gian chạy của cách làm này khá ấn tượng.

>Time-simple:  0.003149918999952206

>Time-optimal:  0.003027181999982531

>Time-one-line:  0.0025488549999863608


Để lý giải cho thời gian chạy này, chúng ta có thể tưởng tượng như sau:
- theo như cách làm ở simple & optimal, chúng ta dùng đến 4 lần so sánh điều kiện (if, else) để đưa ra kết quả. 
- Ở đây, chúng ta dùng 2 lần so sánh điều kiện (if, else), 1 lần dùng toán tử `or` và 1 lần dùng toán tử `+`

--> Có thể là việc dùng `or` và `+` nhanh hơn việc dùng so sánh if, else chăng ?


