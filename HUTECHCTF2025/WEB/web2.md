# D3l3t3 p4g3
![alt text](image-9.png)

![alt text](image-10.png)

Đầu tiên theo như mô tả chúng ta phải xóa trang web này, nhưng khi truy cập trang và chúng ta thấy là ```chỉ có admin mới có quyền xóa```.
Hmm vậy làm thế nào để trở thành admin và xóa luôn trang web này?

### Solution
Như bạn đã biết khi truy cập bất cứ trang web nào sẽ có 1 định nghĩa là ```request``` và ```response``` ([khai niem](https://viblo.asia/p/http-request-va-http-response-naQZR42G5vx))

ở ```http Resquest``` sẽ có khái niệm gọi là: ```Method``` 
```
Method: là phương thức mà HTTP Request này sử dụng, thường là GET, POST, ngoài ra còn một số phương thức khác như HEAD, PUT, DELETE, OPTION, CONNECT.
```

Vì yêu cầu đề bài là xóa 1 trang web nên ta sử dụng ```method``` là ```DELETE``` cho request
khái niệm của ```method DELETE```
```
DELETE: xóa một tài nguyên định trước.
```

vậy áp dụng những gì ta phân tích ta sẽ gán method là ```DELETE``` cho request gửi đi là được. Nhưng gán bằng cách nào??

Có nhiều cách nhưng mình sẽ sử dụng tool ```Burp suite``` để bắt request gửi đi và chỉnh request đó theo yêu cầu của mình.
link cài tool: [Burp suite](https://portswigger.net/burp/communitydownload)

Bạn có thể xem video trên youtube hoặc học ở ([cach sai BUrp](https://portswigger.net/support/using-burp-suite)), hoặc học với AI cũng là lựa chọn hay.

sau khi sử dụng browser của burp và bắt request lại:

![alt text](image-11.png)

Đây là request của mình và hãy nhìn hàng đầu tiên nó đang sử dụng method ```GET``` tức là nó đang yêu cầu website trả về dữ liệu.
Mình muốn "xóa" web mình sẽ thay ```GET``` bằng ```DELETE```
![alt text](image-12.png)

Sau khi xóa mình nhận được một thông báo 
![alt text](image-13.png)

Như trang web đã nói "chỉ có admin mới xóa được" vậy mình chỉ là 1 user ```thường``` vào trang web, làm thế nào để thành 1 user ```admin```

Phân tích kĩ lại các request mình để ý đoạn request này
![alt text](image-14.png)

Ở hàng số 5 có dòng ```X-Role: user```. Có vẻ như Backend xác thực Role bằng đoạn này và sẽ ra sao nếu mình chỉnh nó thành ```X-Role: admin``` với method là ```DELETE``` theo đúng yêu cầu của bài.
![alt text](image-15.png)

##### OK vậy là mình đã xóa trang web và tránh Hallow truy cập vào nó để xem lịch sử.

![alt text](image-16.png)

problem solved: ```HUTECHCTF{I_AM_A_BIG_fan_OF_J97_HOW_ABOUT_YOU???}```