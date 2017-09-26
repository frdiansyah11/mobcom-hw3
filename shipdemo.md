# Getting Started with LibGDX

Tahap pertama yang harus kita lakukan adalah mendownload terlebih dahulu projek [ShipDemo](http://www.cs.cornell.edu/courses/cs3152/2017sp/labs/program1/). Setelah terdownload kemudia extak file rar tersebut kemudian buka android studio untuk editornya.

<div align="center"><img src="https://user-images.githubusercontent.com/30854434/30863530-7c911816-a2fb-11e7-94fc-e670677dd322.jpg" widht="50%" /></div>

Setalah android studionya terbuka lalu pilihlah import project(EclipseADT, Gradle, etc.)

<div align="center"><img src="https://user-images.githubusercontent.com/30854434/30863813-2c3df752-a2fc-11e7-9037-5d5e58a103de.jpg"/></div>

Kemudian pilih pada tab menu Debug Configurations dan pilih add new configuration lalu isi menu pada tabel configuration.

## 1. Play with the FrameRate

Untuk merubah pengaturan board screen maka kita perlu sedikit merubah ukuran dari screen itu sendir atau bisa dibuat full screen, serta menambahkan <em>config.foregroundFPS = 60;</em> untuk melancarkan permain karena retina manusia hanya 60 fps.

<div align="center"><img src="https://user-images.githubusercontent.com/30854434/30867747-fd095124-a306-11e7-97de-ccba9bd35b5d.jpg" /> </div>

## 2. Change the ship sizes

Untuk memperbesar ukuran ship agar tidak manual, kita perlu adanya variabel tambahan untuk mempasing perubahan seperti contoh ini :
<div align="center"><img src="https://user-images.githubusercontent.com/30854434/30869105-90c61fc0-a30a-11e7-98b8-a003a5f6efbd.jpg" /></div>

Jika ingin memper besar gambarnya menjadi 2x maka tinggal di kalikan 2 saja variabel tersebut.

## 3. Implement wrap-around

Pada bagian ini untuk membuat gambar muncul bisa menembus, jadi tidak memantul. seperti gambar dibawah ini :

<div align="center"><img src="https://user-images.githubusercontent.com/30854434/30869524-91d1ab40-a30b-11e7-8563-056dff93f21d.jpg" /></div>

dan untuk codenya seperti dibawah ini :

<div align="center"><img src="https://user-images.githubusercontent.com/30854434/30869676-fdba5af0-a30b-11e7-96d9-084dd55cb9a0.jpg" /></div>

## 4. Play with the draw mode

Pada bagian ini untuk merubah jenis pelurunya kita bisa merubahnya dengan codenya seperti ini :

<div align="center"><img src="https://user-images.githubusercontent.com/30854434/30869924-a0cdc3da-a30c-11e7-8e0f-4c0ed70a4dbd.jpg" /></div>

Dan untuk melihat asset dari jenis pelurunya bisa kita lihat difoldernya.Disini kita juga bisa merubah warna pelurunya jika peluru kita setting pada mode additive, kita asumsikan warna yang akan kita pilih ada 7 dan kita juga bisa mengatur fade out pelurunya. Sehingga, jika kita tembakkan pelurunya semakin jauh maka peluru semakin menipis kemudian hilang. berikut codenya :

<div align="center"><img src="https://user-images.githubusercontent.com/30854434/30870295-b5d7a9b6-a30d-11e7-8b5f-19a5ccd46a2e.jpg" /></div>
