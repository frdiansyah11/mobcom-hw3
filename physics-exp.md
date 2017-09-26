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
* Body       = memberikan body untuk objek yang akan dibuat. Body ini akan dibuat dengan method World.CreateBody.

SpriteBatch,Sprite, dan Texture merupakan class yang sudah disediakan dari Libgdx. World dan Body merupakan class extension dari Box2D. Penjelasan lebih lanjut mengenai class-class yang digunakan, cek disini:
* [SpriteBatch,Sprite,Texture](https://github.com/libgdx/libgdx/wiki/Spritebatch%2C-Textureregions%2C-and-Sprites)
* [World,Body](https://github.com/libgdx/libgdx/wiki/Box2d)

Selanjutnya kita masuk ke method create(). Method ini merupakan method awal sebelum masuk ke fungsi render() untuk menginisialisasi hal-hal yang perlu kita berikan terlebih dahulu. Pertama kita lihat pada bagian memasukkan gambar.
```
    batch = new SpriteBatch();             
    img = new Texture("badlogic.jpg");
    sprite = new Sprite(img);
    sprite.setPosition(Gdx.graphics.getWidth() / 2 - sprite.getWidth() / 2, Gdx.graphics.getHeight() / 2);

```
membuat objek SpriteBatch terlebih dahulu untuk nantinya akan digunakan pada saat render. Gambar yang dimasukkan ke Texture kemudian dimasukkan ke dalam objek Sprite, supaya gambar dapat dioleh ke berbagai hal. sprite.setPosition terserbut untuk menaruh gambar tersebut ke tengah layar
