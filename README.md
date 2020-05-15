## Cara membuat local domain dan mengaktifkan https localhost XAMPP di windows 10

- Buka folder instalan xampp biasanya di drive C:\ atau disesuaikan dengan direktori pada saat menginstal
- Buka folder apache di ```C:\xampp\apache```. 
- Buat file baru ```https.txt``` kemudian edit lewat text editor anda salin kode berikut :
  ```
  authorityKeyIdentifier=keyid,issuer 
  basicConstraints=CA:FALSE 
  keyUsage = digitalSignature, nonRepudiation, keyEncipherment, dataEncipherment 
  subjectAltName = @alt_names 

  [alt_names] 
  DNS.1 = localhost
  DNS.1 = codespace.test
  ```
  Perhatikan DNS.1 = localhost ini bisa anda edit atau tambah jika ada DNS yang diinginkan menggunakan protokol https. sebagai contoh:

  ```
  [alt_names] 
  DNS.1 = localhost
  DNS.2 = produk.test
  DNS.3 = wordpress.com
  DNS.4 = perpustakaan.dev
  DNS.5 = coba.test
  ```
  Lalu di save.
- Kemudian rename file tersebut menjadi ```https.ext```
- Selanjutnya masih di direktori yang sama buka file ```makecert.bat``` kedalam text editor anda bisa notepad, sublime text, vscode dll.  jika tidak bisa dibuka, coba ubah file nya menjadi format ```.txt``` . lalu cari kode yang semula nya:

  ```
  bin\openssl x509 -in server.csr -out server.crt -req -signkey server.key -days 365
  ```
  Ubah kode nya menjadi seperti ini :
  ```
  bin\openssl x509 -in server.csr -out server.crt -req -signkey server.key -days 500 -sha256 -extfile https.ext
  ```
  Lalu di save.

- Setelah itu buka cmd bisa menggunakan perintah windows + R dan ketikkan cmd lalu enter. setelah itu ketikkan:
  ```
  cd C:\xampp\apache
  makecert
  ```
  Ikuti perintah seperti dibawah ini :
  ```
  Enter PEM pass phrase : 12345678
  Verifying – Enter PEM pass phrase: 12345678
  Country Name (2 letter code) [AU]:ID
  State or Province Name (full name) [Some-State]: Jakarta
  Locality Name (eg, city) []: Jakarta
  Organization Name (eg, company) [Internet Widgits Pty Ltd]: Web Dev
  Organizational Unit Name (eg, section) []:laravel
  Common Name (e.g. server FQDN or YOUR name) []:localhost
  Email Address []:example@mail.com
  A challenge password []:
  An optional company name []:localhost
  Enter pass phrase for privkey.pem:12345678
  ```
  Jika mengikuti perintah diatas maka akan menampilkan ```Press any key to continue . . .``` di akhir. Berarti penginstalan ssl sudah berhasil. 
- Selanjutnya buka ```Manage user certificates``` di search windows atau bisa menggunakan perintah windows + R dan ketikkan ```certmgr.msc``` lalu enter. 
- Kemudian pilih ```Trusted Root Certification Authorities``` lalu klik kanan Certificates > All Tasks > Import… lalu klik next
- Kemudian klik tombol browse cari file ```server.crt``` di direktory ```C:\xampp\apache\conf\ssl.crt\server.crt``` klik next
- Pilih all certificates in the following store kemudian isikan ```Trusted Root Certification Authorities``` lalu next > finish.
  Jika anda ingin merubah domain project framework laravel yang menggunakan ip ```127.0.0.1``` . anda bisa mengubah nya dengan membuka file ```hosts``` di direktori ```C:\Windows\System32\drivers\etc``` . lalu tambahkan ini di bawah didalam file hosts :
  ```
  127.0.0.1         codespace.test
  ```
  atau menggunakan ip lain selain ini juga bisa , itu sebagai contoh saja untuk merubah domain nya dan jangan lupa di save .
- Kemudian buka file ```httpd-vhosts.conf``` di direktori ```C:\xampp\apache\conf\extra``` lalu tambah kan kode atau salin kode dibawah ini.
  ```
  <VirtualHost 127.0.0.1:80>
      DocumentRoot "C:/xampp/htdocs/Develompent/codespace/public"
      ServerName codespace.test
      ServerAlias www.codespace.test
  </VirtualHost>

  <VirtualHost 127.0.0.1:443>
      DocumentRoot "C:/xampp/htdocs/Develompent/codespace/public"
      ServerName codespace.test
      ServerAlias www.codespace.test
      SSLEngine on
      SSLCertificateFile "C:/xampp/apache/conf/ssl.crt/server.crt"
      SSLCertificateKeyFile "C:/xampp/apache/conf/ssl.key/server.key"
      <Directory "C:/xampp/htdocs/Develompent/codespace/public">
          Options All
    	  AllowOverride All
    	  Require all granted
      </Directory>
  </VirtualHost>
  ```
  Ip bisa diubah tinggal disesuaikan, untuk contoh itu ip laravel yang biasa memakai ```127.0.0.1``` kemudian untuk ```DocumentRoot "C:/xampp/htdocs/Develompent/codespace/public"``` direktori path nya disesuaikan dengan file anda yang ingin dibuat https. ```Directory "C:/xampp/htdocs/Develompent/codespace/public"``` juga sama tinggal disesuaikan. jika sudah diubah jangan lupa untuk save file nya.

- Setelah itu buka aplikasi xampp jika sudah start apache nya kita stop dulu baru start lagi atau di restart apache xampp nya. lalu buka web browser kesayangan anda bisa google chrome, mozila, opera dll dan buka https://localhost atau klo saya buka https://codespace.test maka akan menjadi secure.

[source](https://www.iltekkomputer.com/cara-mengaktifkan-https-localhost-xampp-di-windows-menjadi-secure/)
