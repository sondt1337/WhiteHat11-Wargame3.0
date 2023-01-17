# flagcheck
Tác giả:  admin-wargame

Lĩnh vực: REVERSE ENGINEERING

Điểm: 64

Đánh giá: ★★★☆☆

Given file: [File](/challenge_wargame/re/re7)

# Hướng giải

Check main: 

![image](https://user-images.githubusercontent.com/87920408/212787344-f09c6b93-049e-43f3-a31f-082960e8d06f.png)

Qua check hàm main thì dễ thấy hàm này đơn giản là cho 1 dãy ký tự đầu vào, sau đó chúng ta phải nhập flag và hàm này sẽ check từng ký tự rồi xor với số thứ tự của chúng và cộng thêm 1 

![image](https://user-images.githubusercontent.com/87920408/212787564-e6cb90ed-c0ff-4e6e-9cb7-3324e6e8b09d.png)

Và sau đó so sánh bộ ký tự mới được in ra và bộ ký tự ban đầu cho, vậy nên đến đây có thể viết script để có được flag

code:
```python
encode_flag = "Vjjp`Nf|roqSua}Ow}aKg%H{q{wpxpxE~mLTX"
for i in range(len(encode_flag)):
    print(chr(ord(encode_flag[i]) ^ (i + 1)), end ="")
```

flag: `WhiteHat{ez_xor_for_r3_challenge_Oop}`
