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
      
        sprite since it's going to move
        img = new Texture("badlogic.jpg");
        sprite = new Sprite(img);
   
        sprite.setPosition(Gdx.graphics.getWidth() / 2 - sprite.getWidth() / 2,
                Gdx.graphics.getHeight() / 2);

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
* Body       = memberikan rigid-body. Body ini akan dibuat dengan method World.CreateBody.

