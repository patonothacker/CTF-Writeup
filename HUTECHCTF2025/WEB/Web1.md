# Robots modern

<img width="1197" height="434" alt="image" src="https://github.com/user-attachments/assets/de9d0229-b0c9-4e4e-899d-bbf430cfd545" />


![image-1](https://github.com/user-attachments/assets/29484ae7-10b8-42f9-90a7-3e2a08b2beae)


Khi vào trang này có rất nhiều thông tin về robots, vậy robots là gì nhỉ 

<img width="1591" height="1141" alt="image-2" src="https://github.com/user-attachments/assets/c745e139-52de-4968-8ee5-c5a24d6cb4e9" />


một cú research đơn giản để giải quyết vấn đề này([robots?](https://developers.google.com/search/docs/crawling-indexing/robots/intro))


```robots.txt``` là một file đặt ở root website (ví dụ: example.com/robots.txt) dùng để hướng dẫn các web crawler (Googlebot, Bingbot, v.v.) xem nên thu thập (crawl) hay không nên thu thập những phần nào trong website.


### Solution

Theo như gợi ý bây giờ mình sẽ truy cập endpoint /robots.txt thử

<img width="1328" height="404" alt="image-3" src="https://github.com/user-attachments/assets/3d8925ab-1dae-448a-88ec-241a4e0465e1" />


Có vẻ như Pato không muốn cho bot truy cập endpoint đó và đã quên xóa đi ```/secret/flag.html``` 

truy cập vào ```/secret/flag.html``` chúng ta có flag

![image-4](https://github.com/user-attachments/assets/3f2823be-26f4-4315-a2ac-565e416e1a44)



```Cltrl+U``` để thấy rõ hơn
<img width="2305" height="489" alt="image-6" src="https://github.com/user-attachments/assets/3ea5c534-b631-4afb-881c-770a4b7c548a" />


Decode đoạn ```SFVURUNIQ1RGe1RyM19jMG5fc2FfbTRjXzdydXllbl9jaGFuX25oYXVfYkluaF8weGl9```

<img width="1517" height="91" alt="image-7" src="https://github.com/user-attachments/assets/892a682b-918e-4ed5-9b6e-af07cf520f39" />



Problem solved: ```HUTECHCTF{Tr3_c0n_sa_m4c_7ruyen_chan_nhau_bInh_0xi}```
