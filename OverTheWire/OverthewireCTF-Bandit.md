# OverTheWire CTF Write-Ups: Bandit
At the time I’m writing these lines I’m studying Web Development, with the objective of getting into Cybersec next year. I already completed [Bandit](https://overthewire.org/wargames/bandit/) a couple of months ago, but I want to test myself again, as I’m far from anything “pro”-like. Without further delay:

### Level 0:

I just enter the game by using `ssh` and the provided user and password (bandit0).

```
ssh bandit0@bandit.labs.overthewire.org -p2220
```

Then I open the _readme_ file with `cat`. The password is inside.

```
cat readme
```

|Password|
|--------|
|    NH2SXQwcBdpmTEzi3bvBHMM9H66vVXjL    |


### Level 1:

This level, as most of the them, can be solved in a handful of ways. However, in order to keep this document somewhat short, I will show only one of them, usually the one that first comes to my mind. In this case a `cat` to the path of the file provided is enough.

```
cat ./-
```



|Password|
|--------|
|    rRGizSaX8Mk1RTb1CNQoXTcYZWU6lgzi		|

### Level 2:

I used `cat` and TAB to autocomplete with backslashes.

```
cat spaces\ in\ this\ filename
```

|Password|
|--------|
|    aBZ0W5EmUfAf7kHTQeOwd8bauFJ2lAiG|

### Level 3:

First I utilize `ls -la` to show the hidden file name in the folder _inhere_ and then `cat` it.

```
ls -la inhere/
cat inhere/.hidden
```


|Password|
|--------|
|    2EW7BBsr6aMMoJ2HjW067dm8EgX26xNe|

### Level 4:

I used `more` to display the name of the files and it’s content, then `cat` to show them all at once. Then I look manually for the password.

```
more inhere/* | cat
```


|Password|
|--------|
|    lrIWWI6bB37kxfiCQZqUdOIYfr6eEeqR|

### Level 5:

I searched the file through `find` and passed it through `xargs cat` to write the password on the console.

```
find inhere/* -size 1033c ! -executable | xargs cat
```


|Password|
|--------|
|    P4L4vucdmLnm8I7Vl7jG1ApGSfjYKqJU|

### Level 6:

Again I used `find` plus the arguments required and a pipe to`xargs cat` to show the password.

```
find / -user bandit7 -group bandit6 -size 33c 2>/dev/null | xargs cat
```


|Password|
|--------|
|    z7WtoNQU2XfjmMtWA8u5rN4vzqu4v99S|

### Level 7:

I filtered the text with `grep`, showing the line in which the given keyword was located.

```
grep millionth data.txt
```


|Password|
|--------|
|    TESKZC0XvTetK0S9xNwm25STk5iWrBvP|

### Level 8:

I knew `uniq` was the go-to utility, and after reading it’s `man` entry I remembered it needs _adjacent matching lines_ to work properly, so first of all I had to `sort` the file.

```
sort data.txt | uniq -u
```

|Password|
|--------|
|    EN632PlfYiZbn3PhVK3XOGSlNInNE00t|

### Level 9:

I used `strings` after trying to filter human-readable strings efficiently with `grep`; then I looked for the lines with ‘=’ signs preceding.

```
strings data.txt | grep ===
```

|Password|
|--------|
|    G7w8LIi6J3kTb8A7j9LgrywtEUlyyp6s|

### Level 10:

For this one I used `base64` plus the decode option onto the given file.

```
base64 data.txt -d
```


|Password|
|--------|
|    6zPeziLdR2RKNdNYFNb6nVCKzphlXHBM|

### Level 11:

`tr` has always been quite a headache for me, especially for the bracket syntax. After watching some videos I could finally solve it on my own. Now I hope I dont forget it… again…

```
cat data.txt | tr '[G-ZA-Fg-za-f]' '[T-ZA-St-za-s]'
```
|Password|
|--------|
|    JVNBBFSmZwKKOP0XbFXOoW8chDz5yVRv|

### Level 12:

After a bunch of `mv`, `gzip`,`bzip2` and `tar` I got the password.

I know it can be scripted with something like `7z` but for me it is faster by hand right now.

```
cat data.txt | xxd -r > data2
file data2
mv data2 data2.gz
gzip data2.gz -d
file data2
mv data2 data3.bz2
bzip2 data3.bz2 -d
file data3
mv data3 data4.gz
gzip data4.gz -d
file data4
mv data4 data4.tar
tar -xf data4.tar
file data5.bin
mv data5.bin data5.tar
tar -xf data5.tar
file data6.bin
mv data6.bin data6.bz2
bzip2 -d data6.bz2
file data6
mv data6 data6.tar
tar -xf data6.tar
file data8.bin
mv data8.bin data8.gz
gzip -d data8.gz
file data8
cat data8
```


|Password|
|--------|
|    wbWdlBxEir4CaE8LaPhauuOo6pwRmrDw|

### Level 13:

I used `ssh -i` to log the private key onto the next level.

```
ssh bandit14@localhost -i sshkey.private -p2220
cat /etc/bandit_pass/bandit14
```


|Password|
|--------|
|    fGrHPx402xGC7U7rXKDaxiWFTOiF0ENq|

### Level 14:

With `telnet` I easily connect to the server and port, and then provide the previous password to get the next one.

```
telnet localhost 30000
```


|Password|
|--------|
|    jN2kgmIXJ6fShzhT2avhotn4Zcka6tnt|

### Level 15:

I researched and found `openssl s_client` to be a nice solution.

```
openssl s_client -connect localhost:30001
```


|Password|
|--------|
|    JQttfApK4SeyHwDlI9SXGR50qclOAil1|

### Level 16:

I scanned the ports using `nc` (with 2>&1 I redirected the output allowing me to `grep` it later) and test them manually with `openssl s_client` .

```
nc localhost 31000-32000 -zv 2>&1 | grep succeeded
openssl s_client -connect localhost:31790
```
|Password|
|--------|
|    ----BEGIN RSA PRIVATE KEY----- MIIEogIBAAKCAQEAvmOkuifmMg6HL2YPIOjon6iWfbp7c3jx34YkYWqUH57SUdyJ imZzeyGC0gtZPGujUSxiJSWI/oTqexh+cAMTSMlOJf7+BrJObArnxd9Y7YT2bRPQ Ja6Lzb558YW3FZl87ORiO+rW4LCDCNd2lUvLE/GL2GWyuKN0K5iCd5TbtJzEkQTu DSt2mcNn4rhAL+JFr56o4T6z8WWAW18BR6yGrMq7Q/kALHYW3OekePQAzL0VUYbW JGTi65CxbCnzc/w4+mqQyvmzpWtMAzJTzAzQxNbkR2MBGySxDLrjg0LWN6sK7wNX x0YVztz/zbIkPjfkU1jHS+9EbVNj+D1XFOJuaQIDAQABAoIBABagpxpM1aoLWfvD KHcj10nqcoBc4oE11aFYQwik7xfW+24pRNuDE6SFthOar69jp5RlLwD1NhPx3iBl J9nOM8OJ0VToum43UOS8YxF8WwhXriYGnc1sskbwpXOUDc9uX4+UESzH22P29ovd d8WErY0gPxun8pbJLmxkAtWNhpMvfe0050vk9TL5wqbu9AlbssgTcCXkMQnPw9nC YNN6DDP2lbcBrvgT9YCNL6C+ZKufD52yOQ9qOkwFTEQpjtF4uNtJom+asvlpmS8A vLY9r60wYSvmZhNqBUrj7lyCtXMIu1kkd4w7F77k+DjHoAXyxcUp1DGL51sOmama +TOWWgECgYEA8JtPxP0GRJ+IQkX262jM3dEIkza8ky5moIwUqYdsx0NxHgRRhORT 8c8hAuRBb2G82so8vUHk/fur85OEfc9TncnCY2crpoqsghifKLxrLgtT+qDpfZnx SatLdt8GfQ85yA7hnWWJ2MxF3NaeSDm75Lsm+tBbAiyc9P2jGRNtMSkCgYEAypHd HCctNi/FwjulhttFx/rHYKhLidZDFYeiE/v45bN4yFm8x7R/b0iE7KaszX+Exdvt SghaTdcG0Knyw1bpJVyusavPzpaJMjdJ6tcFhVAbAjm7enCIvGCSx+X3l5SiWg0A R57hJglezIiVjv3aGwHwvlZvtszK6zV6oXFAu0ECgYAbjo46T4hyP5tJi93V5HDi Ttiek7xRVxUl+iU7rWkGAXFpMLFteQEsRr7PJ/lemmEY5eTDAFMLy9FL2m9oQWCg R8VdwSk8r9FGLS+9aKcV5PI/WEKlwgXinB3OhYimtiG2Cg5JCqIZFHxD6MjEGOiu L8ktHMPvodBwNsSBULpG0QKBgBAplTfC1HOnWiMGOU3KPwYWt0O6CdTkmJOmL8Ni blh9elyZ9FsGxsgtRBXRsqXuz7wtsQAgLHxbdLq/ZJQ7YfzOKU4ZxEnabvXnvWkU YOdjHdSOoKvDQNWu6ucyLRAWFuISeXw9a/9p7ftpxm0TSgyvmfLF2MIAEwyzRqaM 77pBAoGAMmjmIJdjp+Ez8duyn3ieo36yrttF5NSsJLAbxFpdlc1gvtGCWW+9Cq0b dxviW8+TFVEBl1O4f7HVm6EpTscdDxU+bCXWkfjuRb7Dy9GOtt9JPsX8MBTakzh3 vBgsyi/sN3RqRBcGU40fOoZyfAMT8s1m/uYv52O6IgeuZ/ujbjY= -----END RSA PRIVATE KEY---—|


### Level 17:

I used `diff` to get the different string in the files given.

```
diff passwords.new passwords.old
```
|Password|
|--------|
|    hga5tuuCLF6fFzUpnagiMN8ssu9LFrdg|

### Level 18:

With `ssh -t` we can execute commands on the server before it closes the connection.

```
ssh bandit18@bandit.labs.overthewire.org -p2220 -t cat ./readme
```


|Password|
|--------|
|    awhqfNnAbc1naukrpqDYcF95h7HoMTrC|

### Level 19:

The binary file requested a command to execute as bandit20, so I passed `bash -p` allowing me to type commands as _suid_. I could have done this in one step, passing `cat` to the binary.

```
./bandit20-do bash -p
cat /etc/bandit_pass/bandit20
```


|Password|
|--------|
|    VxCazJaVykI6W36BkBU0mJTCM8rR95XT|

### Level 20:

This level amazed me at first. First we open another terminal, connect to bandit20 and listen with `nc -l` on one port. Then we pass that port to the binary on our first instance. Finally we write the previous password on the new window, and it returns us back the next one.

```
nc -l 1239
# in another terminal
./suconnect 1239
# write the current password in the nc terminal
```


|Password|
|--------|
|    NvEJF7oVjkddltPSrdKEFOllh9V1IBcq|

### Level 21:

We just follow the trace of the given directory and the bandit22 script being executed by `cron` to find the password copied in the _tmp_ file.

```
cat /etc/cron.d/*
cat /usr/bin/cronjob_bandit22.sh
cat /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
```


|Password|
|--------|
|    WdDozAdTM2z9DiFEQ2mGlwngMfj4EZff|

### Level 22:

Same process as before, but I had to reply the script in order to get the name of the _tmp_ file, in which the password was being copied into.

```
cat /etc/cron.d/*
cat /usr/bin/cronjob_bandit23.sh
echo I am user bandit23 | md5sum | cut -d ' ' -f 1
cat /tmp/8ca319486bfbbc3663ea0fbe81326349
```
|Password|
|--------|
|    QYw0Y2aiA672PsMmh9puTQuhoz8SyR2G|

### Level 23:

A `cron` is running periodically executing scripts, so all we have to do is write one to do whatever we want to.

```
cat /etc/cron.d/*
cat /usr/bin/cronjob_bandit24.sh
mktemp -d
cd /tmp/tmp.Z3JcS58pR4
nano script.sh
touch password.txt
chmod 777 password.txt
chmod 777 /tmp/tmp.Z3JcS58pR4
chmod 777 script.sh
cp script.sh /var/spool/bandit24/foo/script.sh
cat password.txt
```

script:
```
#!/bin/bash
cat /etc/bandit_pass/bandit24 > /tmp/tmp.Z3JcS58pR4/password.txt
```
|Password|
|--------|
|    VAfGXJ1PBSsPSnvsjI8p759leLZ9GGar|

### Level 24:

We write a script to output the password plus an iterating 4-digit pincode. After saving the combinations to a file, we pipe it to the 30002 daemon and exclude the “Wrong” output messages.

```
nano script.sh
chmod +x script.sh
./script.sh > dictionary.txt
cat dictionary.txt | nc localhost 30002 | grep -Ev "Wrong|Please"
```

script:

```
#!/bin/bash
for i in {0001..9999}; do
echo 'VAfGXJ1PBSsPSnvsjI8p759leLZ9GGar' $i
done
```


|Password|
|--------|
|    p7TaowMYrmu23Ol8hiZh9UvD0O9hpx8d|

### Level 25:

This one is pretty cool. After looking for the shell in bandit26 we find out `exec more ~/text.txt exit 0` , so to not get fired out at the moment we resize the terminal, allowing `more` to show up. Then we enter _Vim_ and read the password of bandit26 on bandit_pass.

```
cat /etc/passwd | grep bandit26
cat /usr/bin/showtext
#We resize the terminal, 6 lines max size
ssh -i bandit26.sshkey bandit26@localhost -p2220
#In more we press 'v' to start up an editor
v
#On vim, we use 'e' to access the file
:e /etc/bandit_pass/bandit26
```
|Password|
|--------|
|    c7GvcKlw9mC7aUQaPx7nwFstuAIBw1o1|

### Level 26:

From last level: we set the shell variable to “_/bin/bash_”, get into it and use the binary given to read the next password from bandit_pass.

```
#From level 25's Vim
:set shell=/bin/bash
:shell
#We are now logged as bandit26. Let's use the binary given to read the next password
./bandit27-do cat /etc/bandit_pass/bandit27
```
|Password|
|--------|
|    YnQpBuifNMas1hcUFk70ZmqkhUU2EuaS|

### Level 27:

Very straightforward, just clone the Git repository given.

```
#From mktemp -d directory
git clone ssh://bandit27-git@localhost:2220/home/bandit27-git/repo
cd repo/
cat README
```
|Password|
|--------|
|    AVanL161y9rsbcJIsFHuw35rjaOM19nR|

### Level 28:

Clone the Git repository given and look up past versions.

```
#From mktemp -d directory
git clone ssh://bandit28-git@localhost:2220/home/bandit27-git/repo
cd repo/
cat README
#Let's find out previous versions
git log -p
```


|Password|
|--------|
|    tQKvmcwNYcFS6vmPHIUSI3ShmsrQZK8S|

### Level 29:

Clone the Git repository given and compare the branches to find out what the password is (or was).

```
#From mktemp -d directory
git clone ssh://bandit29-git@localhost:2220/home/bandit27-git/repo
cd repo/
cat README
#We list the project branches, then compare it
git branch -va
git diff remotes/origin/HEAD remotes/origin/dev
```


|Password|
|--------|
|    xbhV3HpNGlTIdnjUrdAlPzc2L6y9EOnS|

### Level 30:

Clone the Git repository given and list the project tags. Show the non-suspicious “secret” one…

```
#From mktemp -d directory
git clone ssh://bandit29-git@localhost:2220/home/bandit27-git/repo
cd repo/
cat README
#We list the project tags, there is one called "secret"
git tag
git show secret
```


|Password|
|--------|
|    OoffzGDlzhAlerFJ2cAiz1D41JW1Mhmt|

### Level 31:

Clone the Git repository given and push the required file and content within. Before it we have to edit or delete _.gitignore_ file, due to it stopping us to push files with ”_.txt_” extension.

```
#From mktemp -d directory
git clone ssh://bandit29-git@localhost:2220/home/bandit27-git/repo
cd repo/
cat README
#We have to push a file named key.txt with the given text inside
nano key.txt
git show secret
#If I try to add the file to the push a warning is triggered. I have to edit or delete the .gitignore file (can't push .txt)
rm .gitignore
git add key.txt
git commit -m "Adding key"
git push -u origin master
```


|Password|
|--------|
|    rmCBvG56y58BXzv98yZGdO7ATVL5dW8y|

### Level 32:

Last one… We are spawned in a shell in which every command we write is parsed as uppercase. Using `$0` (-bash or another shell) we get into bandit33 to copy the final password form it’s file.

```
$0 
whoami 
#bandit33 
cat /etc/bandit_pass/bandit33 
```


|Password|
|--------|
|    odHo63fHiFqcWWJG9rLiLDtPm45KzUKy|
