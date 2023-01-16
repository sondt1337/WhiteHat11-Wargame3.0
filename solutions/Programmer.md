# Programmer
Tác giả:  admin-wargame

Lĩnh vực: CRYPTOGRAPHY

Điểm: 64

Đánh giá: ★★★☆☆

Given file: [File](/challenge_wargame/crypto/Programming.py)
# Hướng giải


```python
from secret import flag

def cal_flag(flag):
	output=[]
	for i in range(len(flag)):
	    temp = ord(flag[i])**17%3233
	    output.append(temp)
	print(output)

if __name__ == '__main__':
	cal_flag(flag)

#[604, 2170, 3179, 884, 1313, 3000, 1632, 884, 855, 3179, 119, 1632, 2271, 119, 612, 2412, 2185, 2923, 2412, 1632, 2271, 2271, 1313, 2412, 119, 3179, 119, 2170, 1632, 2578, 1313, 119, 2235, 2185, 119, 745, 3179, 1369, 1313, 1516]
```

Đọc kỹ đề có thể thấy từ việc ta có 1 flag cho sẵn của đề –> ta sẽ biến đổi từng phần tử flag qua hàm `cal_flag` bằng cách với mỗi ký tự của flag vào sẽ được mũ 17 lên và lấy phần dư với 3233

Ta tiến hành viết lại code để chuyển đổi từng số trong output trở lại thành flag

Do mình cũng không biết có cách nào hay hơn nên mình chỉ đơn giản là bruteforce cái encode đó về lại thành flag với cấu trúc lệnh duy nhất là `**17%3233` (với mỗi ký tự thuộc khoảng (0, 127) sẽ cho qua lệnh để xem nó có phải là ký tự của encode tương ứng không)

code: 
```python
encode = [604, 2170, 3179, 884, 1313, 3000, 1632, 884, 855, 3179, 119, 1632, 2271, 119, 612, 2412, 2185, 2923, 2412,
          1632, 2271, 2271, 1313, 2412, 119, 3179, 119, 2170, 1632, 2578, 1313, 119, 2235, 2185, 119, 745, 3179, 1369, 1313, 1516]

for i in encode:
    for x in range(0, 127):
        if ((x**17 % 3233) == i):
            print(chr(x), end="")
```

flag: `WhiteHat{i_am_programmer_i_have_no_life}`
