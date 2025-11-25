# D3l3t3 p4g3
<img width="1208" height="859" alt="image-9" src="https://github.com/user-attachments/assets/7456d228-e875-4a60-a502-647d0cd141cf" />


<img width="2423" height="1045" alt="image-10" src="https://github.com/user-attachments/assets/8b69f59c-8371-44b3-b327-27ad41cd5b09" />


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



Đây là request của mình và hãy nhìn hàng đầu tiên nó đang sử dụng method ```GET``` tức là nó đang yêu cầu website trả về dữ liệu.
Mình muốn "xóa" web mình sẽ thay ```GET``` bằng ```DELETE```


Sau khi xóa mình nhận được một thông báo 


Như trang web đã nói "chỉ có admin mới xóa được" vậy mình chỉ là 1 user ```thường``` vào trang web, làm thế nào để thành 1 user ```admin```

Phân tích kĩ lại các request mình để ý đoạn request này
<img width="1092" height="520" alt="image-14" src="https://github.com/user-attachments/assets/cad2c800-31f8-4097-8a60-c386acb678a7" />


Ở hàng số 5 có dòng ```X-Role: user```. Có vẻ như Backend xác thực Role bằng đoạn này và sẽ ra sao nếu mình chỉnh nó thành ```X-Role: admin``` với method là ```DELETE``` theo đúng yêu cầu của bài.
<img width="1087" height="499" alt="image-15" src="https://github.com/user-attachments/assets/e0e14695-31d8-40ff-8b26-174f0a5366e2" />


##### OK vậy là mình đã xóa trang web và tránh Hallow truy cập vào nó để xem lịch sử.

<img width="1590" height="271" alt="image-16" src="https://github.com/user-attachments/assets/affca278-1105-4cf8-b3b0-8097cab143e9" />



problem solved: ```HUTECHCTF{I_AM_A_BIG_fan_OF_J97_HOW_ABOUT_YOU???}```
