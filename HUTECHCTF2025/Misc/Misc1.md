## L13n Qu4n


<img width="796" height="267" alt="ảnh" src="https://github.com/user-attachments/assets/b9dbe016-68a6-4421-a080-fbec4e9801c4" />

Đầu tiên mình đọc đoạn trên và đoán ra được con quái vật khổng lồ đang ngủ say đó là: ```CAESAR``` và cùng có một đoạn cipher rất nổi tiếng là ```Caesar Cipher```([Caesar la gi?](https://vi.wikipedia.org/wiki/M%E1%BA%ADt_m%C3%A3_Caesar)) 

Sau đó mình dùng tool để decrypt ```Caesar``` [Tool decrypt](https://www.dcode.fr/caesar-cipher), của đoạn mã ```zxk_vlr_mixv_dxjb_xka_zebzh_jb_qefp_fp_jv_molcfib_kxjb_:QrfHvZrz71390906```
mình ra được kết quả:
<img width="840" height="286" alt="ảnh" src="https://github.com/user-attachments/assets/61946b8b-c4a3-4ae4-ace7-38e07d6b55b3" />

```	can_you_play_game_and_check_me_this_is_my_profile_name_:TuiKyCuc71390906```

khi đọc đoạn sau decode và tên đề là liên quân, mình vào liên quân và check profile: ```TuiKyCuc71390906```

<img width="2778" height="1284" alt="ảnh" src="https://github.com/user-attachments/assets/bd80beda-1d8e-4999-93e1-9116e51edc6f" />

Để ý kĩ phần giới thiệu là ```_23dHaRqjSkd``` mình tiếp tục mang đoạn này ra decode.

<img width="852" height="374" alt="ảnh" src="https://github.com/user-attachments/assets/a04785cb-0a63-4905-b959-53dbf93a663c" />

mình được 1 part của flag ```_23aExOngPha```.

tiếp theo check khách của profile: ```TuiKyCuc71390906``` ta có thể thấy được part 1 flag.

<img width="2778" height="1284" alt="ảnh" src="https://github.com/user-attachments/assets/268617c4-42d0-4771-90b3-b0d6fff5cd5c" />

part 1: ```HUTECHCTF{1```

ghép hai đoạn lại: ```HUTECHCTF{1_23aExOngPha```

tiếp đến check tiếp phần khác của profile ```TuiKyCuc71390906``` mình thấy 1 account khá giống flag, mà lại có tên giống với Author nữa check profile này chúng ta có phần cuối của flag.

<img width="2778" height="1284" alt="ảnh" src="https://github.com/user-attachments/assets/4c0455bd-7724-442d-866c-96ac68c4606c" />

<img width="2778" height="1284" alt="ảnh" src="https://github.com/user-attachments/assets/366359e9-25fe-4867-883d-2126212df2f6" />

part cuối là: ```_wrlbhxYlhwQdp}``` sau khi decode là:

<img width="839" height="446" alt="ảnh" src="https://github.com/user-attachments/assets/4b9d1705-a7d0-4ddd-99e5-38cc6a11cf90" />

part cuối: ```_toiyeuVietNam}``` 

Ghép 3 part lại: ```HUTECHCTF{1_23aExOngPha_toiyeuVietNam}```

problem solved: ```HUTECHCTF{1_23aExOngPha_toiyeuVietNam}```










