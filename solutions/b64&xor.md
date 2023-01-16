# b64&xor
Tác giả:  admin-wargame

Lĩnh vực: CRYPTOGRAPHY

Điểm: 128

Đánh giá: ★★★☆☆

Given file: [File](/challenge_wargame/crypto/b64&xor.md)
# Hướng giải

Đề bài cho 1 đoạn base64 dịch sang thì thành 1 đoạn không hề có nghĩa gì lắm 

![image](https://user-images.githubusercontent.com/87920408/212610428-1e796fe0-6324-4608-bbd1-2e24c01ff014.png)

Mà đề bài lại là xor&b64 nhưng lại chẳng cho thứ cần xor là gì nên mình nghĩ sẽ check thử với format của đề bài: `WhiteHat{`

và sau vài lần thử thì mình đã thành công bằng việc chuyển đoạn cipher ban đầu từ base64 decode ra về byte rồi xor với các byte của format đề 

```python
import base64

if __name__ == "__main__":
    coded_string = "NFwcKxN4DxMGLVxFABUAADgQFgAqMgNRMQ89PAAbNyldXg4iF1xBJik="
    format_flag  = "WhiteHat{"
    flag = []
    y = bytes(format_flag, 'utf-8')
    x = base64.b64decode(coded_string)
    for i in range(0,9):
        j = 0 
        print((chr(x[i] ^ y[i])), end ="")
```
Đoạn này ra tương ứng với `c4u_v0ng}`, mình thử thêm nhiều `WhiteHat{` nữa vào format flag và nó trả về như này 

![image](https://user-images.githubusercontent.com/87920408/212610967-27eaa92b-70d2-467f-b71c-26d75f5aea6b.png)

Không có ý nghĩa gì lắm nên trở lại thì mình chợt nghĩ ra, nếu xor từ `WhiteHat{` thành được `c4u_v0ng}` vậy thì từ `c4u_v0ng}` cũng về được `WhiteHat{`
và mục tiêu bây giờ chỉ cần kiếm cái đoạn đằng sau cái `WhiteHat{` đó bằng cách mình thêm rất nhiều cụm `c4u_v0ng}` vào format_flag

```python
import base64

if __name__ == "__main__":
    coded_string = "NFwcKxN4DxMGLVxFABUAADgQFgAqMgNRMQ89PAAbNyldXg4iF1xBJik="
    format_flag  = "c4u_v0ng}c4u_v0ng}c4u_v0ng}c4u_v0ng}c4u_v0ng}c4u_v0ng}c4u_v0ng}c4u_v0ng}"
    flag = []
    y = bytes(format_flag, 'utf-8')
    x = base64.b64decode(coded_string)
    for i in range(0,41):
        j = 0 
        print((chr(x[i] ^ y[i])), end ="")
```

flag: `WhiteHat{Nh0_c0n_mu4_mua_h@_4nh_m0i_th4y_c4u_v0ng}`
