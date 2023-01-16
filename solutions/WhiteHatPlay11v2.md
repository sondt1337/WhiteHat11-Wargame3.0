# WhiteHatPlay11v2
Tác giả:  admin-wargame

Lĩnh vực: REVERSE ENGINEERING

Điểm: 256

Đánh giá: ★☆☆☆☆

Given file: [File](/challenge_wargame/re/re2)

# Hướng giải

Mở file .dll nhận được bằng IDA 

## Hàm WhiteHat()

1 số điều cần chú ý ở hàm đặc biệt này:

**biến local:**

![](https://i.imgur.com/NVovlKm.png)

**check biến vào:**

![](https://i.imgur.com/QpjDskX.png)

**LABEL_6:**

![](https://i.imgur.com/ImI1A1I.png)

![](https://i.imgur.com/AQZT4Nu.png)

**LABEL_41**

![](https://i.imgur.com/i7FLJjA.png)


## Chức năng 

Tại **LABEL_6:** 
- `strncpy_s(Destination, 260u, Str, 36u);` và `v4 = Destination;` sẽ hiểu là biến `v4 = Destination` sẽ lấy 36 ký tự đầu của Str

- Một phần nữa là `QDIwMjI=` base64 là @2022 sẽ là 5 ký tự cuối của flag

Hàm check 36 ký tự đầu của input và đối chiếu với flag:

![](https://i.imgur.com/RMMRTpR.png)

mặt khác lại có các byte tương ứng để đối chiếu

![](https://i.imgur.com/uh8BuFQ.png)

Theo mình hiểu code sẽ là qua hàm lấy từng ký tự của input xor với 

Để ý call ngay trước 5 byte cuối sẽ là hàm `sub_10001EC0` xử lý 36 byte đầu của input 

![](https://i.imgur.com/aJZkT0O.png)

Hàm này có công dụng gọi 36 byte đầu của input và mỗi ký tự khi gọi sẽ được xor với 0x11 (^ 0x11) và sau đó cộng với 11 

Viết script:

```py
decode = [0x70, 0x83, 0x84, 0x51, 0x70, 0x30, 0x64, 0x2D, 0x2C, 0x4D, 0x2B, 0x2B, 0x2D, 0x72, 0x2B, 0x68, 0x82, 0x83, 0x68, 0x30, 0x6F, 0x2C, 0x53, 0x7F, 0x88, 0x88, 0x2B, 0x51, 0x7F, 0x87, 0x2D, 0x4E, 0x6E, 0x2D, 0x5E, 0x87]

for i in range(0, len(decode)):
    print(chr((decode[i] - 11) ^ 0x11), end ="")

```

Nhưng có vẻ chưa ổn lắm vì bị ngược (sau 4 byte nó tự đổi)

Whit3H4t --> tihWt4H3

Script Final:
```py
decode = [0x70, 0x83, 0x84, 0x51, 0x70, 0x30, 0x64, 0x2D, 0x2C, 0x4D, 0x2B, 0x2B, 0x2D, 0x72, 0x2B, 0x68, 0x82,
            0x83, 0x68, 0x30, 0x6F, 0x2C, 0x53, 0x7F, 0x88, 0x88, 0x2B, 0x51, 0x7F, 0x87, 0x2D, 0x4E, 0x6E, 0x2D, 0x5E, 0x87]
x = []
for i in range(0, len(decode)):
    x += (chr((decode[i] - 11) ^ 0x11)) # dịch các ký tự
for i in range(0, len(decode), 4): # đổi chỗ các ký tự

    tmp1 = x[i]
    x[i] = x[i+3]
    x[i+3] = tmp1 # Đổi thứ tự 1, 4 --> 4, 1

    tmp2 = x[i+1]
    x[i+1] = x[i+2]
    x[i+2] = tmp2 # đổi thứ tự 2, 3 --> 3, 2
print(*x, sep = "")
```

flag: `WhiteHat{Whit3H4t11S0L1v34LifeY0uW1llR3memB3r}`

**Nhận xét**: Bài này mình thấy khá hay nhưng sao lại ít sao thế nhỉ? (1/5 sao)
