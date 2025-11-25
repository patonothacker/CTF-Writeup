# Robots modern

![alt text](image.png)

![alt text](image-1.png)

Khi vào trang này có rất nhiều thông tin về robots, vậy robots là gì nhỉ 

![alt text](image-2.png)

một cú research đơn giản để giải quyết vấn đề này([robots?](https://developers.google.com/search/docs/crawling-indexing/robots/intro))


```robots.txt``` là một file đặt ở root website (ví dụ: example.com/robots.txt) dùng để hướng dẫn các web crawler (Googlebot, Bingbot, v.v.) xem nên thu thập (crawl) hay không nên thu thập những phần nào trong website.


### Solution

Theo như gợi ý bây giờ mình sẽ truy cập endpoint /robots.txt thử

![alt text](image-3.png)

Có vẻ như Pato không muốn cho bot truy cập endpoint đó và đã quên xóa đi ```/secret/flag.html``` 

truy cập vào ```/secret/flag.html``` chúng ta có flag

![alt text](image-4.png)


```Cltrl+U``` để thấy rõ hơn
![alt text](image-6.png)

Decode đoạn ```SFVURUNIQ1RGe1RyM19jMG5fc2FfbTRjXzdydXllbl9jaGFuX25oYXVfYkluaF8weGl9```

![alt text](image-7.png)

Problem solved: ```HUTECHCTF{Tr3_c0n_sa_m4c_7ruyen_chan_nhau_bInh_0xi}```