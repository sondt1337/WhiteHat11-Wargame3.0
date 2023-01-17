# Corrupt
Tác giả:  admin-wargame

Lĩnh vực: FORENSICS

Điểm: 64

Đánh giá: ★☆☆☆☆

Given file: [File](/challenge_wargame/for/for5)

# Hướng giải

Đầu tiên chúng ta sẽ nhận được 1 file .png mở lên thì thấy không được :3

![image](https://user-images.githubusercontent.com/87920408/212944051-880dfff7-7139-40b2-9173-6ddeb276edd1.png)

kiểm tra hex của nó thì cũng thấy lạ vì không trùng với magic bytes phổ biến nào (các hex đầu)

![image](https://user-images.githubusercontent.com/87920408/212944716-d970804a-c23a-4396-a738-fbdaeeb8b283.png)

Và phía sau đoạn hex đó lại có 1 số đoạn strings quen thuộc như `.png`, `IHDR`, `IDAT` nên mình nghĩ tác giả cố tình chèn 1 vài đoạn strings lạ vào để đánh lừa và thực ra file này ban đầu là 1 ảnh PNG thật, việc của chúng ta bây giờ chỉ cần sửa lại các hex để đưa ra ảnh đúng 

![image](https://user-images.githubusercontent.com/87920408/212945543-9d5c25e9-59c0-4314-b0c8-ff9f9fa4a03a.png)

Ở đây mình sử dụng `bless` để thay đổi các hex trong file ảnh PNG này

![image](https://user-images.githubusercontent.com/87920408/212945730-5541dd33-63fd-4c73-aedc-916d8fb71900.png)

![image](https://user-images.githubusercontent.com/87920408/212945860-5e121166-f32d-47f9-a636-65eb4ff856de.png)

Ở đây có lẽ mình sẽ xoá hết các hex không liên quan đoạn đằng trước `.png` và sau đó đổi `.png` thành `.PNG` bằng cách chuyển bộ số hex từ `89 70 6E 67` thành `89 50 4e 47` tương ứng `.png` --> `.PNG`

Check thấy `IHDR` và `IDAT` đã đúng

Vì tiêu đề PNG có End Of Line cụ thể được nhận dạng trên Linux. Tiêu đề PNG ban đầu của file Corrupt là `89 50 4E 47 0D 1A 0A 1A` và mình sẽ sửa thành `89 50 4E 47 0D 0A 1A 0A`

**Giải thích:** 
- `89` có bit cao được đặt để phát hiện các hệ thống truyền dẫn không hỗ trợ dữ liệu 8 bit và để giảm khả năng tệp văn bản bị hiểu nhầm là PNG hoặc ngược lại.
- `50` `4E` `47` Trong ASCII, các chữ cái PNG, cho phép một người dễ dàng xác định định dạng nếu nó được xem trong trình soạn thảo văn bản.
- `0D` `0A`	Một dòng kiểu DOS kết thúc ( CRLF ) để phát hiện chuyển đổi kết thúc dòng DOS-Unix của dữ liệu.
- `1A` Một byte dừng hiển thị tệp trong DOS khi loại lệnh đã được sử dụng — ký tự cuối tệp.
- `0A` Một dòng kiểu Unix kết thúc ( LF ) để phát hiện chuyển đổi kết thúc dòng Unix-DOS.

Lưu lại và chúng ta sẽ thấy sự khác biệt, bình thường sau bước này mình sẽ check thêm bằng `pngcheck` để xem có lỗi thêm gì không và ở đây may mắn là không lỗi

![image](https://user-images.githubusercontent.com/87920408/212949862-256f1ae0-8538-430c-9542-0b3b6f36d4bb.png)

--> có [ảnh ban đầu](/solutions/Corrupt_full.png):

![image](https://user-images.githubusercontent.com/87920408/212951189-19e1f834-ae9f-4f90-a56e-3cb83c3f5ff4.png)



