# Physics Experience

Pada bagian ini kita akan mencoba menggunakan physics di LibGDX. Kita menggunakan extensions Box2d untuk membantu kita menggunakan physics untuk object yang akan kita gunakan. 

<img src="https://user-images.githubusercontent.com/30854454/30844444-3c03c402-a2b9-11e7-870b-da31c1ef8cc5.png" align="middle" />

Berikut code yang akan dibahas untuk dasar menggunakan physics :

```
import com.badlogic.gdx.ApplicationAdapter;
import com.badlogic.gdx.Gdx;
import com.badlogic.gdx.graphics.GL20;
import com.badlogic.gdx.graphics.Texture;
import com.badlogic.gdx.graphics.g2d.Sprite;
import com.badlogic.gdx.graphics.g2d.SpriteBatch;
import com.badlogic.gdx.math.Vector2;
import com.badlogic.gdx.physics.box2d.*;

public class PhysicsTest extends ApplicationAdapter {
    SpriteBatch batch;
    Sprite sprite;
    Texture img;
    World world;
    Body body;

    @Override
    public void create() {

        batch = new SpriteBatch();             
        img = new Texture("badlogic.jpg");
        sprite = new Sprite(img);
        sprite.setPosition(Gdx.graphics.getWidth() / 2 - sprite.getWidth() / 2, Gdx.graphics.getHeight() / 2);

        world = new World(new Vector2(0, -98f), true);
        
        BodyDef bodyDef = new BodyDef();
        bodyDef.type = BodyDef.BodyType.DynamicBody;                        
        bodyDef.position.set(sprite.getX(), sprite.getY());
        
        body = world.createBody(bodyDef);
        
        PolygonShape shape = new PolygonShape();
        shape.setAsBox(sprite.getWidth()/2, sprite.getHeight()/2);
        
        FixtureDef fixtureDef = new FixtureDef();
        fixtureDef.shape = shape;
        fixtureDef.density = 1f;

        Fixture fixture = body.createFixture(fixtureDef);
        
        shape.dispose();
    }

    @Override
    public void render() {
    
        world.step(Gdx.graphics.getDeltaTime(), 6, 2);
        
        sprite.setPosition(body.getPosition().x, body.getPosition().y);
        
        Gdx.gl.glClearColor(1, 1, 1, 1);
        Gdx.gl.glClear(GL20.GL_COLOR_BUFFER_BIT);
        batch.begin();
        batch.draw(sprite, sprite.getX(), sprite.getY());
        batch.end();
    }

    @Override
    public void dispose() {        
        img.dispose();
        world.dispose();
    }
}
```
Kita akan membahas satu persatu mengenai code. Pada contoh ini, nantinya kita akan menggunakan sampel gambar dari libgdx, gambar tersebut nantinya akan diberi bentuk fisik. Pertama kali kita mendeklarasikan beberapa class yang akan digunakan:
```
    SpriteBatch batch;
    Sprite sprite;
    Texture img;
    World world;
    Body body;
```
Keterangan:
* SprieBatch = memberikan tekstur dan koordinat masing-masing persegi panjang untuk digambar.
* Sprite     = memberikan TextureRegion, menggambarkan geometri, dan memberikan warna.
* Texture    = class yang mendekode file gambar dan meload ke GPU. File gambar harus ditempatkan di folder "assets".
* World      = mengelola semua entitas fisika, simulasi dinamis, dan query asinkron. World juga berisi fasilitas manajemen memori yang efisien.
* Body       = memberikan rigid-body (benda kaku/benda fisik). Body ini akan dibuat dengan method World.CreateBody.

SpriteBatch,Sprite, dan Texture merupakan class yang sudah disediakan dari Libgdx. World dan Body merupakan class extension dari Box2D. Penjelasan lebih lanjut mengenai class-class yang digunakan, cek disini:
* [SpriteBatch,Sprite,Texture](https://github.com/libgdx/libgdx/wiki/Spritebatch%2C-Textureregions%2C-and-Sprites)
* [World,Body](https://github.com/libgdx/libgdx/wiki/Box2d)

## create()

Selanjutnya kita masuk ke method create(). Method ini merupakan method awal sebelum masuk ke fungsi render() untuk menginisialisasi hal-hal yang perlu kita berikan terlebih dahulu. Pertama kita lihat pada bagian memasukkan gambar.
```
    batch = new SpriteBatch();             
    img = new Texture("badlogic.jpg");
    sprite = new Sprite(img);
    sprite.setPosition(Gdx.graphics.getWidth() / 2 - sprite.getWidth() / 2, Gdx.graphics.getHeight() / 2);

```
membuat objek SpriteBatch terlebih dahulu untuk nantinya akan digunakan pada saat render. Gambar yang dimasukkan ke Texture kemudian dimasukkan ke dalam objek Sprite, supaya gambar dapat dioleh ke berbagai hal. sprite.setPosition terserbut untuk menaruh gambar tersebut ke tengah layar.

```
    world = new World(new Vector2(0, -98f), true);
```

world kita berikan objek dari class World, dengan Vector2, parameter pertama Vector2 0 artinya kita tidak memberikan gravitasi ke garis horizontal, parameter kedua Vector2 -98f artinya memberikan efek gravitasi ke vertikal bawah sebanyak 98unit. Ingat bahwa dalam Box2D 1 unit = 1 meter.

```
        BodyDef bodyDef = new BodyDef();
        bodyDef.type = BodyDef.BodyType.DynamicBody;                        
        bodyDef.position.set(sprite.getX(), sprite.getY());        
        body = world.createBody(bodyDef);
```
BodyDef merupakan sebuah class untuk menyimpan semua data yang dibutuhkan untuk membangun rigid-body. Ada 3 macam tipe rigid-body, yaitu Dynamic, Kinematic, dan Static.
* Dynamic   = objek yang bergerak dan dipengaruhi oleh gaya dan objek dinamis, kinematik dan statis lainnya. Cocok untuk benda yang perlu dipindahkan dan dipengaruhi oleh gaya
* Kinematic = peralihan antara statis dan dinamis. Seperti statis, tidak bereaksi terhadap gaya. Tetapi seperti dinamis, memiliki kemampuan untuk bergerak. Badan Kinematik sangat bagus untuk mengendalikan gerak tubuh secara keseluruhan, seperti platform yang bergerak dalam permainan platform.
* Static    = tidak bergerak dan tidak terpengaruh oleh gaya. statis sangat cocok untuk tanah, dinding, dan benda apapun yang tidak perlu bergerak.
Pada kasus ini, yang paling cocok untuk benda ini adalah DynamicBody, karena bergantung terhadap gaya yang akan kita berikan kepadanya(yaitu gravitasi). Lalu set DynamicBody dengan sprite yang kita gunakan, dan definisikan ke dalam Body dengan world.createBody().

Ketika sebuah body dikonstruk, kita harus memberikan bentuk(shape) setelah itu.
```
    PolygonShape shape = new PolygonShape();
    shape.setAsBox(sprite.getWidth()/2, sprite.getHeight()/2);
```
Sebuah alasan menggunakan PolygonShape ialah pada dasarnya mengatur poligon physics ke kotak dengan dimensi yang sama dengan sprite yang kita gunakan.

```
    FixtureDef fixtureDef = new FixtureDef();
    fixtureDef.shape = shape;
    fixtureDef.density = 1f;
    Fixture fixture = body.createFixture(fixtureDef);
```
FixtureDef sebuah class untuk memberikan beberapa jenis definisi fisika seperti gesekan, massa jenis, dan lainnya kepada objek yang akan kita definisikan. Cek di dokumentasi untuk lebih memahami FixtureDef [FixtureDef LibGDX](https://libgdx.badlogicgames.com/nightlies/docs/api/com/badlogic/gdx/physics/box2d/FixtureDef.html)

Kemudian shape yang sebelumnya kita gunakan, sekarang akan kita singkirkan karena kita tidak membutuhkannya pada saat render(karena shape sudah tersimpan di dalam fixture)
```
    shape.dispose();
```

## render()
