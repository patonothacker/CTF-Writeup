# U?104D
<img width="1189" height="459" alt="image-29-1" src="https://github.com/user-attachments/assets/757d42c3-5c63-46b7-ab88-2e3ab3381669" />


<img width="2543" height="1100" alt="image-18" src="https://github.com/user-attachments/assets/2b1bdd3a-2b9b-402c-8269-d9da68fdae77" />


<img width="2154" height="1149" alt="image-19" src="https://github.com/user-attachments/assets/9d4f3c8c-723c-426d-8308-90e1ef3afcb9" />


Đầu tiên ta thấy rằng trang web này có phần upload với các extension được cho phép là ```Allowed: (.png, .jpg, .zip)```, thử upload lên 1 tệp đuôi khác chúng ta sẽ bị:

<img width="1025" height="171" alt="image-20" src="https://github.com/user-attachments/assets/54e669b7-390c-4040-a31c-08e7469cd858" />


Chúng ta bị chặn khi upload extension khác và có thể đoán được backend đang sử dụng Whitelist([ds trắng](https://en.wikipedia.org/wiki/Whitelist)) để Filter

Có 1 lổ hỏng liên quan tới vấn đề này là ```File upload``` ([FileUpload la gi?](https://portswigger.net/web-security/file-upload))

### Solution

Hãy để ý 1 tí, khi upload 1 file ảnh phần định dạng file khi chúng ta upload lên là ```Content-Type: image/png``` 
- vì extension ```png``` được phép nên không bị chặn khi upload



Vậy sẽ ra sao nếu ta Upload lên 1 file nào đó bị chặn và chỉnh ```Content-Type``` thành định dạng được cho phép

Mình thử upload lên 1 file ```.txt``` vì nó không nằm trong định dạng```Allowed: (.png, .jpg, .zip)``` nên sẽ bị chặn và mình sẽ thử đổi ```Content-Type: image/png``` để xem như nào:

<img width="1088" height="834" alt="image-22" src="https://github.com/user-attachments/assets/e6750bc3-e8ea-4b46-b0d0-c7ac6a5eb0bb" />


<img width="2497" height="1165" alt="image-23" src="https://github.com/user-attachments/assets/7ef497ea-138c-4ab1-93eb-82c5506f233e" />


<img width="1473" height="264" alt="image-24" src="https://github.com/user-attachments/assets/6e5d7876-b3e6-44a8-b4ca-394563118e24" />


Mình đã thành công upload lên với cách này, có vẻ như bên trong code backend tin tưởng ```Content-Type``` của user gửi lên nhưng ta có thể thay đổi nó được.

=> lỗ hỏng chính xác ở đây.

Giờ như yêu cầu đề bài ta phải RCE ([rce la gi?](https://www.cloudflare.com/learning/security/what-is-remote-code-execution/)) nó vậy phải làm như nào

Việc khó Google lo
<img width="1590" height="798" alt="image-25" src="https://github.com/user-attachments/assets/fb964b15-6653-4a9d-bcc9-b0e3dfdf4680" />


mình sẽ lấy Shell ở([Webshell](https://gist.github.com/joswr1ght/22f40787de19d80d110b37fb79ac3985)) sau đó tiến hành RCE.

Upload lên Script shell này với Extension là ```.php``` và ```Content-Type: image/png``` 
<img width="1084" height="952" alt="image-26" src="https://github.com/user-attachments/assets/477b19a3-2b38-49f9-8ac1-6cabe6d77f6e" />

<img width="2268" height="1102" alt="image-27" src="https://github.com/user-attachments/assets/f71e3925-3ba2-4a70-be82-aa62ba736e9f" />



vào file vừa upload
<img width="1994" height="287" alt="image-28-1" src="https://github.com/user-attachments/assets/57a12d9c-663d-4a1a-a777-764687c3c722" />



Chúng ta đã RCE thành công vậy bây giờ lấy flag thôi, theo như đề bài nói flag ở thư mục gốc.

<img width="2074" height="927" alt="image-30" src="https://github.com/user-attachments/assets/6414b17b-5b61-41fb-bd01-77b9b401235d" />




Problem solved: ```HUTECHCTF{OMG_YOU_ARE_REAL_HACKER!}```
