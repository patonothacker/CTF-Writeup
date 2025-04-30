# # Challenge WEB: Commit & Order: Version Control Unit.
![image](https://github.com/user-attachments/assets/8cf2ee89-913c-4395-b91d-fbb555233ebc)

First after entering the website we will see a login form, when I tried to view the source code it didn't have anything suspicious so I tried fuzzing and found a suspicious point:
![image](https://github.com/user-attachments/assets/7d6c8f89-ce21-4a8f-9efe-241f776721de)

its command is: ffuf -u http://23.179.17.40:58002/FUZZ -w /usr/share/wordlists/dirb/common.txt -mc 200 
It simply scans for hidden links on the page.  
we can easily see a hidden exposed path with .git/HEAD , now let's access it:
![image](https://github.com/user-attachments/assets/315fc617-a9fd-40c1-a8ee-3b13bd7cd5e3)

ref: refs/heads/master is a line that specifies the current branch of a Git repository – specifically stored in the .git/HEAD file.
This is a serious security vulnerability: the server has exposed the .git/ directory, meaning you can download the entire Git repository of this web source code and read the source code, sensitive information (such as passwords, API keys, flags...).
download the tool using the command:
git clone https://github.com/internetwache/GitTools.git
then cd into: cd GitTools/Dumper
we will download the entire .git repo from the server to the local machine using this tool
Next, use the command:
./gitdumper.sh http://23.179.17.40:58002/.git/ /tmp/git-leak/
we will download all of this .git and go to the newly created directory:
cd /tmp/git-leak/
git checkout .

![image](https://github.com/user-attachments/assets/ff1fec4f-4ed8-41fe-9ffe-b4007797ea73)

we have 2 files admin.php and index.php , when reading index i see login information and think i succeeded but wait
![image](https://github.com/user-attachments/assets/85d6d5b3-18ff-45df-bcfb-0f042a7fa3a2)

![image](https://github.com/user-attachments/assets/4a645c8c-29cc-4366-884c-11af91eadefc)

It seems that there is no flag and we need to dig further, after a while of looking around I realized that there is a review section I realized we have to Check the commit history:
git log --oneline
![image](https://github.com/user-attachments/assets/7bf3da9f-a6d3-4e3c-914c-b03c4926112f)

we check each commit with the command:
git checkout ....
and ls, cat reads what is important
here after checking each command to a commit with the code 68f8fcd
git checkout 68f8fcd
ls
cat admin.php and I see
![image](https://github.com/user-attachments/assets/8b9196c2-1242-4046-84cb-5b2ca3fd94ba)

![image](https://github.com/user-attachments/assets/308edb79-4fa8-4346-a2cf-68d43aac04ad)

problem solved: CIT{5d81f7743f4bc2ab}







