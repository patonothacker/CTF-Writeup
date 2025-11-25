# SOS
<img width="778" height="197" alt="ảnh" src="https://github.com/user-attachments/assets/d3a90ecc-a508-4882-b64d-84e267eeee9d" />

<img width="2359" height="990" alt="image-32" src="https://github.com/user-attachments/assets/e9987343-e337-4f7c-9e14-53ef520db855" />


Đầu tiên mình thấy 1 bảng và thử ghi vào 1 vài kí tự xem như nào.

![image-33](https://github.com/user-attachments/assets/844bfd84-9de7-4521-8df1-21bdeb92c5a5)


<img width="2191" height="1141" alt="image-34" src="https://github.com/user-attachments/assets/1cfe186c-76fe-4fea-b28f-fc6e7aa9bf8a" />



nhận thấy rằng khi nhập vào nó hiển thị lên bảng nhập của kí tự chúng ta vừa nhập

# Solution 

Hãy chú ý tới Log nội bộ ở phần 
```Executing: cowsay <your_input>```
ở đây nó đang thực thi một lệnh cowsay và input nhập vào của chúng ta.
Hãy cùng mình test trên linux.

<img width="603" height="336" alt="image-35" src="https://github.com/user-attachments/assets/7632eb6c-67f5-4869-992b-05c350ec2dd5" />


có 1 lệnh trên linux liên quan tới cái này đó là ```cowsay```([cowsay](https://opensource.com/article/18/12/linux-toy-cowsay)), ```cowsay``` tạo ra hình ảnh nghệ thuật ASCII của một chú bò (hoặc các loài động vật/nhân vật khác) kèm theo lời nhắn trong bong bóng thoại (hãy tự test trên linux của bạn để hiểu hơn).

có 1 lỗi liên quan tới việc thực command như thế đó là [Command Injection](https://owasp.org/www-community/attacks/Command_Injection).

thử 1 vài lệnh linux xem có bị chặn gì không, tôi thử ```;``` và đã bị chặn ngay tại đây về các lệnh này nó làm gì và nó hoạt động ra sao bạn có thể đọc([text](https://tldp.org/LDP/Bash-Beginners-Guide/html/sect_03_03.html)), hãy kết hợp với việc tìm trên AI hoặc google và youtube để hiểu hơn cách hoạt động của các command linux.
<img width="486" height="127" alt="image-36" src="https://github.com/user-attachments/assets/b2703183-ccd8-43c4-8a22-c509ab9ec0d7" />


tiếp theo mình thử các kí tự linux khác xem nó có bị chặn không và các kí tự bị chặn là:```;, &,|, (```, các kí tự này dùng để kéo dài chuỗi truy vấn và thực hiện nhiều ```command``` cùng lúc nhưng có vẻ đều bị chặn hết

- vậy sẽ ra sao nếu mình không thực thi lệnh linux trên 1 hàng mà xuống dòng thì sao, ví dụ trên linux
![image-37](https://github.com/user-attachments/assets/810e345e-25cc-4a5f-8c4b-15d487106e87)


Ok giờ mình sẽ thử nhét \n vào xem chuyện gì sẽ xảy ra
khi gửi kí tự ```\n``` phải url encode([URL encode](https://stackoverflow.com/questions/4667942/why-should-i-use-urlencode)) lại thành ```%0a```
với cách này mình đã đấm bằng ```OS command``` thành công


<img width="2179" height="1023" alt="image-39" src="https://github.com/user-attachments/assets/2f24cc7a-65f0-44d4-8ee7-a26f2e8d862f" />


<img width="1958" height="781" alt="image-40" src="https://github.com/user-attachments/assets/b10ae599-ed83-4b43-a2f4-0a4842587672" />

payload mình dùng là: ```haha%0acat /secret.txt```


problem solved: ```HUTECHCTF{0S_C0mmand_is_reAlly_Dangerous}```
