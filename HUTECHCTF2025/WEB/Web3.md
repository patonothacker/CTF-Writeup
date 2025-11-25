# U?104D
![alt text](image-29.png)

![alt text](image-18.png)

![alt text](image-19.png)

Đầu tiên ta thấy rằng trang web này có phần upload với các extension được cho phép là ```Allowed: (.png, .jpg, .zip)```, thử upload lên 1 tệp đuôi khác chúng ta sẽ bị:

![alt text](image-20.png)

Chúng ta bị chặn khi upload extension khác và có thể đoán được backend đang sử dụng Whitelist([ds trắng](https://en.wikipedia.org/wiki/Whitelist)) để Filter

Có 1 lổ hỏng liên quan tới vấn đề này là ```File upload``` ([FileUpload la gi?](https://portswigger.net/web-security/file-upload))

### Solution

Hãy để ý 1 tí, khi upload 1 file ảnh phần định dạng file khi chúng ta upload lên là ```Content-Type: image/png``` 
- vì extension ```png``` được phép nên không bị chặn khi upload
![alt text](image-21.png)

Vậy sẽ ra sao nếu ta Upload lên 1 file nào đó bị chặn và chỉnh ```Content-Type``` thành định dạng được cho phép

Mình thử upload lên 1 file ```.txt``` vì nó không nằm trong định dạng```Allowed: (.png, .jpg, .zip)``` nên sẽ bị chặn và mình sẽ thử đổi ```Content-Type: image/png``` để xem như nào:

![alt text](image-22.png)

![alt text](image-23.png)

![alt text](image-24.png)

Mình đã thành công upload lên với cách này, có vẻ như bên trong code backend tin tưởng ```Content-Type``` của user gửi lên nhưng ta có thể thay đổi nó được.

=> lỗ hỏng chính xác ở đây.

Giờ như yêu cầu đề bài ta phải RCE ([rce la gi?](https://www.cloudflare.com/learning/security/what-is-remote-code-execution/)) nó vậy phải làm như nào

Việc khó Google lo
![alt text](image-25.png)

mình sẽ lấy Shell ở([Webshell](https://gist.github.com/joswr1ght/22f40787de19d80d110b37fb79ac3985)) sau đó tiến hành RCE.

Upload lên Script shell này với Extension là ```.php``` và ```Content-Type: image/png``` 
![alt text](image-26.png)

![alt text](image-27.png)

vào file vừa upload
![alt text](image-28.png)

Chúng ta đã RCE thành công vậy bây giờ lấy flag thôi, theo như đề bài nói flag ở thư mục gốc.

![alt text](image-30.png)

![alt text](image-31.png)

Problem solved: ```HUTECHCTF{OMG_YOU_ARE_REAL_HACKER!}```