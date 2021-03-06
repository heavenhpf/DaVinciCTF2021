# Web

## Obfuscation
### Deskripsi Soal

My password is my secret. You will never find it...

http://challs.dvc.tf:5555/

To validate this chall, please enter the secret code as the flag.

### Penyelesaian Soal
Setelah masuk ke dalam link yang diberikan, maka ditemukan bahwa terdapat sebuah form input untuk mensubmit sesuatu. Kemudian, saya melakukan inspect elemen pada webpage tersebut. Salah satu cuplikan yang menarik adalah pada fungsi '''testsecret''':

```java
function testSecret() {
  /** @type {function(number, ?): ?} */
  var getHistoryTitle = _0x5ae8;
  var title = getHistoryTitle(386);
  var vlyr = document[getHistoryTitle(388)](getHistoryTitle(390))[getHistoryTitle(380)];
  if (vlyr == unescape(title)) {
    document[getHistoryTitle(388)](getHistoryTitle(373))[getHistoryTitle(377)] = getHistoryTitle(378);
  } else {
    /** @type {string} */
    document[getHistoryTitle(388)](getHistoryTitle(373))[getHistoryTitle(377)] = "HEHE my secret is well kept !";
  }
}
```

Setelah title didecode, maka ditemukan flag nya adalah sebagai berikut:

**Flag: dvCTF{1t_is_n0t_4_secr3t_4nym0r3}**

## Authentication

### Deskripsi Soal
Can you find a way to authenticate as admin?

http://challs.dvc.tf:1337/
### Penyelesaian
Jadi di sini saya terpikirkan untuk bagaimana saya bisa masuk dalam autentikasi tersebut sebagai admin, di mana tidak diketahui password, saya harus mencoba me-bypass beberapa metode autentikasi.
Serangan umum yang saya gunakan di sini adalah dengan metode sql injection. Saya mencoba dengan menggunakan:

```Username: admin```

```Password: ' or 1 -- -```

Dan berhasil. Kemudian saya melakukan inspect elemen web dan memperoleh flag yang dimaksud.

**Flag: dvCTF{!th4t_w4s_34sy!}**

## Members

### Deskripsi Soal
Can you get more information about the members?

http://challs.dvc.tf:1337/members

### Penyelesaian
Jadi untuk menyelesaikan challenge ini diharuskan menyelesaikan challenge Web: Authentication terlebih dahulu, yang kemudian akan diberikan izin untuk mengakses layanan web http://challs.dvc.tf:1337/members. 

Saat dilihat pada website nya, terdapat page berisi table yang mengandung informasi tentang anggota di sebelah kanan dan formulir yang memungkinkan untuk mencari anggota di sebelah kiri. Dengan menganalisis kode sumber halaman, saya melihat bahwa form tersebut menggunakan metode ```GET``` untuk mengirimkan parameter pencarian, sehingga semua teks yang ditulis akan dikodekan ke dalam url. Setelah server menerima data yang saya kirim, server tersebut akan mengembalikan informasi tentang anggota. Jadi sepertinya ada Database MySQL yang mendukung aplikasi tersebut, sehingga saya memasukkan beberapa kode berbahaya ke dalam text field yang ada:

```SQL
Leonard" OR 1=1; --
```
Secara khusus, ```tabel information_schema.tables``` berisi informasi tentang semua tabel yang terletak di db. Kemudian dimasukkan kode berikut:
```SQL
Leonard "UNION (SELECT TABLE_NAME, 2,3 FROM INFORMATION_SCHEMA.TABLES); -
```
Dan aplikasi akan mengembalikan semua nama tabel yang disimpan dalam database. Jika kita menggulir ke bawah dapat melihat dua tabel: anggota yaitu tabel yang berisi semua informasi anggota dan tabel lain bernama ```supa_secret_table```.
Tabel bernama ```information_schema.columns``` berisi semua informasi tentang kolom dari semua tabel yang disimpan. Jadi kita bisa mendapatkan nama field ```supa_secret_table``` dengan memasukkan kode:

```SQL
Leonard" UNION (SELECT COLUMN_NAME,2,3 FROM INFORMATION_SCHEMA.COLUMNS WHERE TABLE_NAME='supa_secret_table'); --
```

Aplikasi akan mengembalikan dua record dengan nama field: ```id``` dan ```flag```.

```SQL
Leonard" UNION (SELECT flag,2,3 FROM supa_secret_table); --
```

dan kemudian aplikasi akan mencetak flag nya.

**Flag: dvCTF{1_h0p3_u_d1dnt_us3_sqlm4p}**

## High security
### Deskripsi Soal

You can now see who tried to connect to your account.

What could go wrong? http://challs.dvc.tf:65535/

### Penyelesaian

Ketika mengetahui bahwa IP address dari web bersangkutan adalah IP: 43.21.178.10, saya menggunakan XMLHttpRequest untuk melakukan request admin dan sehingga diperoleh flag nya pada Raw Content.

**Flag: dvCTF{xss_l0ve<3}**

# Stega
## Read
### Deskripsi Soal

Just read!!

<br>
<img height="300" src="https://github.com/HeavenPutra208/Write-Up-CTF/blob/main/flag.png" />
<br>

### Penyelesaian

Pada challenge ini diberikan sebuah gambar bernama flag.png dengan deskripsi mengatakan "Just read!!". Di sini, saya terpikirkan bahwa ada gamabr tersembunyi di balik gambar png tersebut, dikarenakan terutama pada posisi tengah gambar terlihat pola hitam-putih yang tidak rapi. Kemudian, saya menggunakan tools image online https://29a.ch/photo-forensics, kemudian setelah saya mengubah beberapa format gambar ditemukan flag sebagai berikut:


<br>
<img height="300" src="https://github.com/HeavenPutra208/Write-Up-CTF/blob/main/read.bmp" />
<br>

**Flag: dvCTF{th4t5_4_l0t_0f_n0153}**
## Pidieff
### Deskripsi Soal

(Hanya menampilkan file format .pdf yang bisa didownload)

### Penyelesaian

Pada pdf tersebut, tidak ada yang spesial pada isi dari pdf nya dan bersifat mengecoh. Rahasia sebenarnya adalah terletak di luar dari pdf tersebut, di mana terletak pada attachments nya. Apabila dibuka, maka akan ditemukan file bernama flag.txt.

**Flag: dvCTF{look_at_the_files}**
