# Challenge WEB : Apriti sesamo
![image](https://github.com/user-attachments/assets/19e6189d-7b16-460a-8ffc-41f286fa85ea)

In this article, when you see 2 hints related to emacs and Backup files:
![image](https://github.com/user-attachments/assets/eeca5d70-354b-4372-8c1d-81b8e4cb7fd9)

![image](https://github.com/user-attachments/assets/384cff94-2b7c-4fef-809a-2e8fdc30a79f)


normally when you edit a file with emacs, it will give you a ~ sign after the file, so pay a little attention to the url, I added a ~ sign after it:
![image](https://github.com/user-attachments/assets/72cae1d6-2bc5-4179-8f5a-2e747ff61cfe)


of course when on the web you won't see anything unless you use burp suite or curl ....., here I use curl with the command: curl -i "http://verbal-sleep.picoctf.net:50765/impossibleLogin.php~" , and I saw a very strange paragraph:
![image](https://github.com/user-attachments/assets/f9586b67-c72c-4038-ae1a-9ed7b3a0ef5e)

This is how to write ASCII characters in hex and decimal codes, also known as Escape Sequences , and we use the command:
![image](https://github.com/user-attachments/assets/f8c213ea-fe23-4d91-aec2-7dfd9f882e05)

Because php supports Escape sequences, when these characters are encountered, php will convert them into valid base64 code and automatically decode them with the command:
![image](https://github.com/user-attachments/assets/8b78e057-9842-49af-8f43-12c2f592e767)

pay attention to the 3rd if condition: Compare the SHA1 hash value of 2 strings $yuf85e0677 and $rs35c246d5 , If the hash of 2 strings is the same, will read the content of flag.txt file and display it. So now what do we need to do to make these 2 strings equal? ​​, and here I use [] and send request with burp suite with username[]=a&pwd[]=b


In PHP, if you pass an array instead of a string, PHP will skip the normal comparison and may result in bypassing the check condition, username[]=a is equivalent to $_POST['username'] = array('a'); , pwd[]=b is equivalent to $_POST['pwd'] = array('b'); , When sha1($yuf85e0677) === sha1($rs35c246d5), PHP compares the 2 arrays. SHA1 does not support arrays, so PHP will convert the array to another type, causing a logic error, so when the error occurs it will return NULL for both username and pwd so NULL=NULL so the condition is true and the flag is printed:
![image](https://github.com/user-attachments/assets/6ed85281-9340-42f7-bdd8-ff9229fdaa47)

Problem Solved: picoCTF{w3Ll_d3sErV3d_Ch4mp_2d9f3447}




