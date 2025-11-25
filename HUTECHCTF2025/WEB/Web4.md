# SOS
![alt text](image-32.png)

Đầu tiên mình thấy 1 bảng và thử ghi vào 1 vài kí tự xem như nào.

![alt text](image-33.png)

![alt text](image-34.png)

nhận thấy rằng khi nhập vào nó hiển thị lên bảng nhập của kí tự chúng ta vừa nhập

# Solution 

Hãy chú ý tới Log nội bộ ở phần 
```Executing: cowsay <your_input>```
ở đây nó đang thực thi một lệnh cowsay và input nhập vào của chúng ta.
Hãy cùng mình test trên linux.

![alt text](image-35.png)

có 1 lệnh trên linux liên quan tới cái này đó là ```cowsay```([cowsay](https://opensource.com/article/18/12/linux-toy-cowsay)), ```cowsay``` tạo ra hình ảnh nghệ thuật ASCII của một chú bò (hoặc các loài động vật/nhân vật khác) kèm theo lời nhắn trong bong bóng thoại (hãy tự test trên linux của bạn để hiểu hơn).

có 1 lỗi liên quan tới việc thực command như thế đó là [Command Injection](https://owasp.org/www-community/attacks/Command_Injection).

thử 1 vài lệnh linux xem có bị chặn gì không, tôi thử ```;``` và đã bị chặn ngay tại đây về các lệnh này nó làm gì và nó hoạt động ra sao bạn có thể đọc([text](https://tldp.org/LDP/Bash-Beginners-Guide/html/sect_03_03.html)), hãy kết hợp với việc tìm trên AI hoặc google và youtube để hiểu hơn cách hoạt động của các command linux.
![alt text](image-36.png)

tiếp theo mình thử các kí tự linux khác xem nó có bị chặn không và các kí tự bị chặn là:```;, &,|, (```, các kí tự này dùng để kéo dài chuỗi truy vấn và thực hiện nhiều ```command``` cùng lúc nhưng có vẻ đều bị chặn hết

- vậy sẽ ra sao nếu mình không thực thi lệnh linux trên 1 hàng mà xuống dòng thì sao, ví dụ trên linux
![alt text](image-37.png)

Ok giờ mình sẽ thử nhét \n vào xem chuyện gì sẽ xảy ra
khi gửi kí tự ```\n``` phải url encode([URL encode](https://stackoverflow.com/questions/4667942/why-should-i-use-urlencode)) lại thành ```%0a```
với cách này mình đã đấm bằng ```OS command``` thành công
![alt text](image-38.png)

![alt text](image-39.png)

![alt text](image-40.png)

payload mình dùng là: ```haha%0acat /secret.txt```

problem solved: ```HUTECHCTF{0S_C0mmand_is_reAlly_Dangerous}```