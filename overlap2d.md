# Overlap2D - UI and Level Editor

Overlap2D merupakan aplikasi third-party untuk membuat desain UI dan Level pada game yang akan kita buat. Terutama untuk membuat sebuah game berjenis platform/sidecroller, Overlap2D memudahkan kita untuk menempatkan gambar-gambar yang sudah kita miliki.

<img align="middle" src="https://user-images.githubusercontent.com/30854454/30862806-6a041808-a2f9-11e7-942f-51f3379decb1.png" >

Untuk mencoba Overlap2D di LibGDX, berikut ada video dari Youtube yang memberikan tutorial penjelasan contoh membuat game sidescroller platformer.

<div align="center">
<a href="https://www.youtube.com/watch?v=bhvHm2sM0qo" target="_blank"><img src="http://img.youtube.com/vi/bhvHm2sM0qo/0.jpg" 
alt="" width="240" height="180" border="10" /></a>
  <p>Making Simple Sidescroller with Overlap2D and libGDX - Tutorial</p>
</div>

## Experiment

Mengikuti tutorial tersebut, kita mencoba membuat contoh tampilan platformer dari Overlap2D lalu dimasukkan ke projek LibGDX. Pertama kita set projek LibGDX kita, pada saat ini kita mencoba menggunakan Desktop. Centang dibagian extensions Freetype,Box2D, Box2dlights, Ashley,Ai, dan pada third-party extensions centang Overlap2D

<img align="middle" src="https://user-images.githubusercontent.com/30854454/30869290-f26c85e8-a30a-11e7-91d1-f731c8bb6c33.png" >

Sebelum mulai, di tutorial tersebut disebutkan kita harus mengatur beberapa hal, seperti gardle dan tampilan pada Overlap2D. Di bagian gradle ubah:

```
  compile "com.underwaterapps.overlap2druntime:overlap2d-runtime-libgdx:0.1.0"
  
  // menjadi
  
  compile "com.underwaterapps.overlap2druntime:overlap2d-runtime-libgdx:0.1.2-SNAPSHOT"
  
  // kemudian ganti versi libgdx ke versi 1.9.4
  
  gdxVersion = '1.9.4'
```

Alasan tersebut, kita akan memakai versi Overlap2D snapshot untuk beberapa kemudahan, dan versi libgdx 1.9.4 supaya kita bisa memakai animasi tanpa harus terjadi error. Pada bagian Overlap2D, kita harus membuat projek lalu membuat scene. Scene tersebut nantinya akan dijadikan tampilan pada game yang akan dijalankan. Berikut contoh membuat projek dan scene:

<img align="middle" src="https://user-images.githubusercontent.com/30854454/30869781-42e09f36-a30c-11e7-8680-173974a790b8.png" >

Setelah itu diexport ke dalam projek LibGdx kita. Buka bagian class DesktopLauncher (karena kita memakai game Desktop). Kita atur width dan height pada layar gamenya. Contoh kita akan menggunakan ukuran layar 800x480.

```
public class DesktopLauncher {
	public static void main (String[] arg) {
		LwjglApplicationConfiguration config = new LwjglApplicationConfiguration();
		config.width = 800;
		config.height = 480;
		new LwjglApplication(new PlatformerTutorial(), config);
	}
}
```

Lalu pada class core kita, kita set beberapa code untuk menunjang beberapa hal yang ada di dalam game.

```
public class PlatformerTutorial extends ApplicationAdapter {
	private SceneLoader sceneLoader;
	@Override
	public void create () {
		Viewport viewport = new FitViewport(266,160);
		sceneLoader = new SceneLoader();
		sceneLoader.loadScene("MainScene",viewport);
	}

	@Override
	public void render () {
		Gdx.gl.glClearColor(0, 0, 0, 1);
		Gdx.gl.glClear(GL20.GL_COLOR_BUFFER_BIT);
    
		sceneLoader.getEngine().update(Gdx.graphics.getDeltaTime());
	}
}
```

Pertama kita set class SceneLoader untuk memanggil Scene yang sudah kita buat sebelumnya di Overlap2D. ``` private SceneLoader sceneLoader; ``` .

Pada bagian create(), kita menggunakan Viewport lalu memakai FitViewport supaya sesuai dengan garis/ruler yang kita hitung di Overlap2D.
```
@Override
	public void create () {
		Viewport viewport = new FitViewport(266,160);
		sceneLoader = new SceneLoader();
		sceneLoader.loadScene("MainScene",viewport);
	}
```
Kenapa FitViewport parameter nya 266,160? ini menunjukan ukuran dari World, karena angka tersebut setelah dihitung menyesuaikan dengan layar game yang kita gunakan yaitu 800x480. Setelah itu Viewport yang kita buat dimasukkan ke dalam Scene untuk menyesuaikan tampilan layar.

Lalu pada bagian render() kita hanya perlu memanggil Scene tersebut untuk bisa berjalan pada satuan waktu di dalam LibGDX.
```
@Override
	public void render () {
		Gdx.gl.glClearColor(0, 0, 0, 1);
		Gdx.gl.glClear(GL20.GL_COLOR_BUFFER_BIT);
		sceneLoader.getEngine().update(Gdx.graphics.getDeltaTime());
	}
```

Setelah selesai semua dilakukan, maka hasil dari yang kita buat akan menjadi seperti ini:

<img align="middle" src="https://user-images.githubusercontent.com/30854454/30870401-f669cdba-a30d-11e7-884c-ef79eabe32d8.png" >

## Kesimpulan
Extensions third-party Overlap2D ternyata memudahkan kita dalam membuat game berjenis platformer/side-scroller. Kita hanya perlu membuat desain game di Overlap2D, setelah itu semua selesai dilakukan kita harus mengekspornya ke dalam projek LibGDX kita. Jadi, kita tidak perlu bersusah payah membuat desain langsung di dalam LibGDX, kita hanya harus menggunakan bantuan extension third-party seperti Overlap2D. Masih banyak lagi yang bisa dilakukan di tutorial tersebut, kita sedang mempelajarinya lebih mendalam.
