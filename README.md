[![](https://images.microbadger.com/badges/image/babim/pure-ftpd.svg)](https://microbadger.com/images/babim/pure-ftpd "Get your own image badge on microbadger.com")[![](https://images.microbadger.com/badges/version/babim/pure-ftpd.svg)](https://microbadger.com/images/babim/pure-ftpd "Get your own version badge on microbadger.com")
[![](https://images.microbadger.com/badges/image/babim/pure-ftpd:ssh.svg)](https://microbadger.com/images/babim/pure-ftpd:ssh "Get your own image badge on microbadger.com")[![](https://images.microbadger.com/badges/version/babim/pure-ftpd:ssh.svg)](https://microbadger.com/images/babim/pure-ftpd:ssh "Get your own version badge on microbadger.com")

Thanks stilliard/pure-ftpd

### User pure-pw have uid and gid = 1000

# Docker Pure-ftpd Server
============================

## Starting it 
------------------------------

`docker run -d --name ftpd_server -p 21:21 -p 30000-30009:30000-30009 -e "PUBLICHOST=localhost" babim/pure-ftpd`

add for map volume for ftp data:
```
-v /data/ftpdata:/home/ftpusers
-v /data/pure-ftpd:/etc/pure-ftpd
```

*Or for your own image, replace babim/pure-ftpd with the name you built it with, e.g. my-pure-ftp*

## Operating it
------------------------------

`docker exec -it ftpd_server /bin/bash`

## Example usage once inside
------------------------------

Create an ftp user: `e.g. bob with chroot access only to /home/ftpusers/bob`
```bash
pure-pw useradd bob -u ftpuser -d /home/ftpusers/bob
pure-pw mkdb
```
*No restart should be needed.*

Create an ftp user: `e.g. bob with chroot access only to /home/ftpusers/bob`

add user
```
pure-pw useradd bob -u ftpuser -d /home/ftpusers/bob
pure-pw mkdb
```
list user
```
pure-pw list
```
del user
```
pure-pw userdel bob
```
show information
```
pure-pw show bob
```
change password
```
pure-pw passwd bob
```
More info on usage here: https://download.pureftpd.org/pure-ftpd/doc/README.Virtual-Users

## Test your connection
-------------------------
From the host machine:
```bash
ftp -p localhost 21
```

----------------------------------------

## Our default pure-ftpd options explained
----------------------------------------

```
/usr/sbin/pure-ftpd # path to pure-ftpd executable
-c 50 # --maxclientsnumber (no more than 50 people at once)
-C 10 # --maxclientsperip (no more than 10 requests from the same ip)
-l puredb:/etc/pure-ftpd/pureftpd.pdb # --login (login file for virtual users)
-E # --noanonymous (only real users)
-j # --createhomedir (auto create home directory if it doesnt already exist)
-R # --nochmod (prevent usage of the CHMOD command)
-P $PUBLICHOST # IP/Host setting for PASV support, passed in your the PUBLICHOST env var
-p 30000:30009 # PASV port range
```

For more information please see `man pure-ftpd`, or visit: https://www.pureftpd.org/
