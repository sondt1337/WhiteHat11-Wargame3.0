# WhiteHatPlay11v1
Tác giả:  admin-wargame

Lĩnh vực: REVERSE ENGINEERING

Điểm: 32

Đánh giá: ★☆☆☆☆

Given file: [File](/challenge_wargame/re/re1)

# Hướng giải

Mở file bằng IDA, để ý `_main`

![](https://i.imgur.com/CUhZMdd.png)

ở đây có hàm `sub_4010A0`, `sub_401190` và `sub_401440` là đáng chú ý, kiểm tra từng hàm một

## sub_4010A0

![](https://i.imgur.com/cgIeoyN.png)

Tới đây thì có `byte_40EFB0` là không rõ ràng, tìm hiểu sâu vào 

![](https://i.imgur.com/UchJQlL.png)

Để ý kỹ 1 chút ở ngay bên dưới thì có xuất hiện đặc điểm của base85

## sub_401190 

![](https://i.imgur.com/YrMhKuV.png)


Thấy có `byte_410F0C`, kiểm tra thì có 

![](https://i.imgur.com/OQxAf3f.png)

Đến đây là đã xong, dịch base85 bằng cyberchef

![](https://i.imgur.com/lfgcKNR.png)

flag: `Whit3H4t11H4v34N1C3D4yR3VeRs31!`

Bài này chắc là vì lý do chỉ có click&click nên số sao dành tặng cho chall chỉ là 2/5 

