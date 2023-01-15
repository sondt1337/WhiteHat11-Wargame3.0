# WhiteHatPlay11v1
Tác giả:  admin-wargame

Lĩnh vực: REVERSE ENGINEERING

Điểm: 32

Đánh giá: ★☆☆☆☆

Given file: [File](/challenge_wargame/re/re1)

# Hướng giải

Nhìn file tải về là biết nó liên quan đến python, là 1 người chơi rev chắc hẳn sẽ biết đến pyc (đã từng được mình nhắc đến trong chall của BlueHens)

![](https://i.imgur.com/yb488Vi.png)

Sau một hồi loay hoay thì mình tìm được tool để decompile file này là [pyinstxtractor](https://github.com/extremecoders-re/pyinstxtractor)

Sau khi chạy pyinstxtractor:

![](https://i.imgur.com/JVBREYP.png)

Và ra file quewridg.pyc, sử dụng [Uncompyle6](https://github.com/rocky/python-uncompyle6) hoặc [Decompiler Tools](https://www.decompiler.com/)

Code:
```py
    # uncompyle6 version 3.7.4
# Python bytecode 3.7 (3394)
# Decompiled from: Python 2.7.17 (default, Sep 30 2020, 13:38:04) 
# [GCC 7.5.0]
# Warning: this version of Python has problems handling the Python 3 "byte" type in constants properly.

# Embedded file name: quewridg.py
import base64, os, time
str1 = []
str2 = []
k = None
h = None

def re():
    global h
    global k
    s = 'VOhEdHV0YIRVVLF0S9'
    x = '92Mp5GXI5XV79DMO1F'
    for i in range(len(s)):
        if i % 2 != 0:
            str1.append(s[i])
            str2.append(x[i])
        else:
            str1.append(x[i])
            str2.append(s[i])

    k = ''.join(str1)
    h = ''.join(str2)


def write_1():
    with open(os.environ['USERPROFILE'] + str(base64.b64decode('XEFwcERhdGFcTG9jYWxcVGVtcFw='), 'utf-8') + k + str(base64.b64decode('LnR4dA=='), 'utf-8'), 'w') as (f):
        f.write('YmFuIGNvIHRoYXkgY29uIGJhY2ggdHVvYyBrZXUga2hvbmc=')
    with open(os.environ['USERPROFILE'] + str(base64.b64decode('XEFwcERhdGFcUm9hbWluZ1w='), 'utf-8') + h + str(base64.b64decode('LnR4dA=='), 'utf-8'), 'w') as (f):
        f.write('dGltIHRodSB4ZW0=')


def content():
    banner = ".__   __.  __    ______   ___   .___________.____    _  _    .___  ___. \n|  \\ |  | /_ |  /      | / _ \\  |           |___ \\  | || |   |   \\/   | \n|   \\|  |  | | |  ,----'| | | | `---|  |----` __) | | || |_  |  \\  /  | \n|  . `  |  | | |  |     | | | |     |  |     |__ <  |__   _| |  |\\/|  | \n|  |\\   |  | | |  `----.| |_| |     |  |     ___) |    | |   |  |  |  | \n|__| \\__|  |_|  \\______| \\___/      |__|    |____/     |_|   |__|  |__|"
    print(banner)
    print('=======================================================================================')
    print('                      https://www.youtube.com/shorts/t1u-h4rlNSY                       ')
    print('=======================================================================================')
    time.sleep(5)


if __name__ == '__main__':
    content()
    re()
    write_1()
```

Đọc code thì có thể thấy ở hàm `write_1()` chỉ là sử dụng base64 để đánh lừa, khi biên dịch ra thực chất chỉ là tạo file và điền vào đó vài cụm từ vô nghĩa với mục đích chúng ta cần, hàm `content()`

Vậy ở đây chắc chỉ dùng được hàm `re()`

ta thấy h và k đã được trải qua nhiều lệnh nhưng lại không để làm gì nên sẽ in ra xem nó sẽ làm gì 

Khi in ra thì

h = V2hpdGVIYXR7VDFOSF

k = 9OME5HX05IVV9LM019

Dịch thử base64 

![](https://i.imgur.com/bpciw0o.png)

flag: `WhiteHat{T1NH_N0NG_NHU_K3M}`

Có lẽ tác giả ra đến đây bí ý tưởng :)) chứ không thể xàm như này được 
