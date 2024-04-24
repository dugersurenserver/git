windows дээр тохиргоо хийх

Credential Manager -руу орж өөрчлөлт хийнэ

git config --global user.name "FIRST_NAME LAST_NAME"

git config --global user.email "MY_NAME@example.com"


git config user.name "dugersuren-server"

git config user.email "dugersuren.server@gmail.com"


cat .git/config




echo "# git" >> README.md

git init

git add README.md

git commit -m "first commit"

git branch -M main

git remote add origin https://github.com/dugersurenserver/git.git

git push -u origin main






git remote add origin https://github.com/dugersurenserver/git.git

git branch -M main

git push -u origin main



LINUX үйлдлийн системд хэрэглэгч нэмэх, эрх олгох үүрэгтэй командууд

nano /etc/passwd 
useradd -c "Dugersuren Battsend" -m -s /bin/bash bd

userdel bd 
userdel -r bd


useradd -c "Dugersuren Battsend" -m -s /bin/bash bd
useradd -c "Munkhbayar Nayantai" -m -s /bin/bash munkhbayar

Эрхүүдийг олгож өгөх
sudo visudo
