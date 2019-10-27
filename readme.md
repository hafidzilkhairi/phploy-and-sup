# PHPloy dan sup

<Table>
<thead>
<tr>
<td>
Nama
</td>
<td>
NIM
</td>
</tr>
<thead>

<tr>
<td>
Hafidzil Khairi
</td>
<td>
1301160171
</td>
</tr>
<tr>
<td>
Yola Adipratama
</td>
<td>
1301144156
</td>
</tr>

</Table>

## PHPloy
PHPloy merupakan tool yang powerful pada masanya, dan juga berfungsi untuk mendeploy kodingan atau app ke sebuah server.

PHPloy mengatasi masalah dimana ketika seorang programmer tidak mengupload file kodingan ke server secara effisien, alih-alih hanya mengupload file kodingan yang diubah saja, programmer yang sudah lupa, file apa saja yang telah diubah malah mengupload semua file kodingan. Cara mengupload seperti ini justru menghabiskan banyak resource mulai dari waktu sampai bandwidth.

PHP loy hadir untuk masalah yang telah disebutkan pada paragraf sebelumnya, dengan memanfaatkan Git, PHPloy hanya mengupload file yang telah diubah melalui protokol FTP.

### Cara kerja
PHPloy menyimpan sebuah file pada server bernama ".revision". File tersebut menyimpan nilai hash dari commit yang telah dideploy pada server. Ketika kita menjalankan "phploy" pada PC lokal, file .revision didownload kemudian mencari sejauh berapa commit server tertinggal, kemudian memperbaharui file-file yang ada pada server.

### Dokumentasi penggunaan

PHPloy dapat didownload dari repository berikut:
<a href="https://github.com/banago/PHPloy">Link</a>

1. tempatkan phploy pada project anda
2. Buat file ``` deploy.ini ``` berisi seperti contoh berikut dan disesuaikan dengan konfigurasi anda
```php
; This is a sample deploy.ini file.
; You can specify as many servers as you need
; and use whichever configuration way you like.

[staging]
user = example
pass = password
host = staging-example.com
path = /path/to/installation
port = 21passive = true

[production]
user = example
pass = password
host = production-example.com
path = /path/to/installation
port = 21
passive = true

; If that seemed too long for you, you can use quickmode instead:
[quickmode]
  staging = ftp://example:password@staging-example.com:21/path/to/installation
  production = ftp://example:password@production-example.com:21/path/to/installation
```
3. Jalankan phploy pada PC anda setiap anda ingin mendeploy dengan perintah ``` php phploy``` pada terminal


# Stack Up (sup)

Merupakan sebuah deployment tool yang menjalankan sekumpulan perintah/syntax pada satu atau banyak host secara bersamaan. Tool ini membaca file konfigurasi YAML, yang mana berisi networks(kumpulan host), perintah-perintah serta targetnya.

## Cara kerja

sup membaca file konfigurasi kemudian melakukan syntax yang telah didefinisikan pada file tersebut.

## Dokumentasi penggunaan

### Requirement / ketergantungan package

- golang

### instalasi
Ketik ``` go get -u github.com/pressly/sup/cmd/sup ``` untuk menginstal sup pada komputer

### mendefenisikan host-host pada file konfigurasi

```yaml
networks:
    production:
        hosts:
            - api1.example.com
            - api2.example.com
            - api3.example.com
    staging:
        # fetch dynamic list of hosts
        inventory: curl http://example.com/latest/meta-data/hostname
```
Dari file diatas didapat ada 2 jenis host: production dan staging

untuk menjalankan perintah pada jenis host tertentu, ketik perintah berikut:
```bash
sup [jenis host] [command]
```

### Mendefinisikan commands/perintah-perintah

Perintah-perintah atau command juga didefenisikan didalam file konfigurasi, sebagai contoh:

```YAML
commands:
    restart:
        desc: Restart example Docker container
        run: sudo docker restart example
    tail-logs:
        desc: Watch tail of Docker logs from all hosts
        run: sudo docker logs --tail=20 -f example
```

### Menjalankan

studi kasus: merestart docker pada host production

Perintah:
```bash
sup production restart
```
