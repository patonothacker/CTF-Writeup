# Leak Force 
<img width="466" height="255" alt="image" src="https://github.com/user-attachments/assets/7164fdc1-40dd-432e-bd45-65202f211104" />



the firstly, we have a login form and we try register
<img width="952" height="543" alt="image" src="https://github.com/user-attachments/assets/19e18fda-7a97-462f-9b68-3e2b31205920" />

then we login on and easy see some feature such as: save changes, reset password......

<img width="1251" height="724" alt="image" src="https://github.com/user-attachments/assets/90aaf9ce-e04f-4535-b3d5-2cea2e81f26e" />


we try save changes to see what happens and see more clearly in burp suite  

<img width="1069" height="423" alt="image" src="https://github.com/user-attachments/assets/9042fa86-4ab1-4b10-9b4d-5f5c8cfe596e" />

and test feature change password 

<img width="1026" height="377" alt="image" src="https://github.com/user-attachments/assets/f7695755-040f-4fb9-85d6-2a923c78a88f" />


how to get flag ????

let notice to the API , what happen if we change API as other number

<img width="1070" height="415" alt="image" src="https://github.com/user-attachments/assets/2fa1392f-64b2-41ac-b6b8-fab96de1b815" />

<img width="1029" height="442" alt="image" src="https://github.com/user-attachments/assets/046fe70d-ebe9-49c1-ac22-9cb8a1de4c76" />

woww , we can see another acount , it's IDOR vulnerability, try it as admin

<img width="1010" height="438" alt="image" src="https://github.com/user-attachments/assets/bc260c65-54a8-4b55-b450-0d6341497f85" />


you can see that , we can login as admin, and right now , we change password as 1 and id of admin is 1

<img width="1042" height="407" alt="image" src="https://github.com/user-attachments/assets/210d876d-c4d0-440b-8fe2-9f8801d3df24" />

yayyyy , we changed password of admin = 1 

<img width="994" height="364" alt="image" src="https://github.com/user-attachments/assets/7ad2daa3-c03d-4dbf-abbd-516e76cf1855" />

and right now , login with account admin and we can get flag 
<img width="944" height="356" alt="image" src="https://github.com/user-attachments/assets/48bae41b-e8c9-4d30-8001-1ca1c7a18174" />

<img width="1064" height="367" alt="image" src="https://github.com/user-attachments/assets/36e6b252-7372-4884-b7f2-78b15b0ea0fb" />

Problem solved: CSCV2025{7h3_Uni73d_N47i0ns_C0nv3n7i0n_4g4ins7_Cyb3rcrim3}






