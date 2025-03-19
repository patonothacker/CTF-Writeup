# Challengge web 4: n0s4n1ty 1.
![image](https://github.com/user-attachments/assets/0a9e10f4-4693-439e-98e7-48ce33a71dcd)

First when entering the website we get a place to upload files, when you try any .zip or any other extension you can upload to the server and the vulnerability starts from there:
![image](https://github.com/user-attachments/assets/55eca5ef-dc21-44b8-8e7c-8a5c4bfaff00)

as you can see i uploaded any file and it has no filter or validation here, so i will use a php file when it is transmitted to the server it will execute on the system(https://drive.google.com/file/d/1ctHeH-W5F24t6oQT-9QOlNVjP4oAh6tj/view?usp=drive_link), create a file under my link content and name it whatever you want and have .php for this file to execute on the system
![image](https://github.com/user-attachments/assets/0e515d0b-f891-49a2-b57a-611fd1b211d6)

as you can see above i have uploaded it and now we will access it like this:
![image](https://github.com/user-attachments/assets/685e5717-83cd-4954-8307-98a9b6174d5e)

and here we can execute any command we want, and as the problem above describes we need to read the flag in the /root directory, so now our command will be:
sudo ls -al /root/ , sudo cat /root/flag.txt 

![image](https://github.com/user-attachments/assets/d2f75e56-6cea-4b91-bd8a-63a7495d9410)

This is also known as the Arbitrary File Upload vulnerability, some useful information here: https://theadminbar.com/security-weekly/what-is-an-arbitrary-file-upload-vulnerability/

Problem solved: picoCTF{wh47_c4n_u_d0_wPHP_8ca28f94}
