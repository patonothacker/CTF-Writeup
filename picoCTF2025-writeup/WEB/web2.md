#  Challenge 2 WEB: HEAD-DUMP
![image](https://github.com/user-attachments/assets/2144dacf-d263-4e5b-9da9-53aed8b5a8ba)
First when I clicked on that link and went to that website I saw quite a few articles
![image](https://github.com/user-attachments/assets/24a40670-3c81-477f-8abe-bf12e4a3078b)

I noticed that the second post talks about back end as hint suggests: 
![image](https://github.com/user-attachments/assets/1a84c73d-0443-42ea-9349-57045189ecd7)

when I clicked on #api, I was redirected to Swagger API Documentation (a little info about it here: https://swagger.io/solutions/api-documentation/), after trying for a while I realized the /headdump part was at the end as described by hint
![image](https://github.com/user-attachments/assets/1b02aada-2dde-4fbd-812a-be5677d7818a)

![image](https://github.com/user-attachments/assets/c5aa50c0-698b-4d7c-ab06-ce0687c84b6f)

I ran it and strangely enough found it downloadable:
![image](https://github.com/user-attachments/assets/166fc718-46db-49cb-9c10-311f60dbaba6)

after downloading i tried with command cat heapdump-1742376007132.heapsnapshot | grep -i "pico" and we got the flag
![image](https://github.com/user-attachments/assets/533d9327-a71c-41f5-9800-40d799eff407)

problem solved: picoCTF{Pat!3nt_15_Th3_K3y_bed6b6b8}

