---
title: Tutorial Android Shared Preferences
description: Tutorial shared preferences di Android.
date: 2015-10-08
tags:
  - Tutorial
---
Halo, pada artikel ini kita akan belajar shared preferences di android. Fungsi dari shared preferences berbeda dengan database, shared preferences biasanya digunakan untuk menyimpan data pengaturan aplikasi kita seperti pengaturan tema, notifikasi atau menyimpan suatu nilai session, menyimpan skor terbaik dan masih banyak lagi.

<div class="video">
	<iframe width="560" height="315" src="https://www.youtube.com/embed/MWbLpPU42AQ" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen=""></iframe>
</div>

Disini saya menggunakan shared preferences untuk menyimpan nama dan nilai score. Yang pertama membuat nilai-nilai default yang akan disimpan ke shared preferences, yang dapat berguna sewaktu belum ada nilai dan sewaktu pengguna mereset aplikasi ke nilai atau pengaturan awal.

```java
public class Constant {
  public static final String DEFAULT_NAME = "Tidak ada nama";
  public static final int DEFAULT_SCORE = 0;
}
```

Setelah membuat nilai-nilai default, sekarang waktunya membuat shared preferencesnya. Pada class <code>PrefManager</code> ini saya membuat <code>getName()</code> dan <code>getScore()</code> berguna untuk mengambil data nama dan score, apabila datanya masih kosong yang diambil adalah <code>Constant.DEFAULT_NAME</code> dan <code>Constant.DEFAULT_SCORE</code>. Sedangkan <code>setName()</code> dan <code>setScore()</code> untuk mengubah data nama dan score pada shared preferences.

```java
public class PrefManager {
  private static final String PREF_NAME = "ExampleSharedPreferences";
  private static final String KEY_NAME = "name";
  private static final String KEY_SCORE = "score";
  private final SharedPreferences pref;
  private static final int PRIVATE_MODE = 0;

  public PrefManager(Context context) {
    pref = context.getSharedPreferences(PREF_NAME, PRIVATE_MODE);
  }

  public void setName(String name) {
    Editor editor = pref.edit();
    editor.putString(KEY_NAME, name);
    editor.apply();
  }

  public String getName() {
    return pref.getString(KEY_NAME, Constant.DEFAULT_NAME);
  }

  public void setScore(int score) {
    Editor editor = pref.edit();
    editor.putInt(KEY_SCORE, score);
    editor.apply();
  }

  public int getScore() {
    return pref.getInt(KEY_SCORE, Constant.DEFAULT_SCORE);
  }

}
```

Class App berikut ini optional saja, sebagai penghubung class <code>PrefManager</code>. Anda juga dapat mengakses <code>PrefManager</code> langsung melalui Activity atau Fragment.

```java
public class App extends Application {
  private static App mInstance;
  private PrefManager pref;

  @Override
  public void onCreate() {
    super.onCreate();
    mInstance = this;
    pref = new PrefManager(this);
  }

  public static synchronized App getInstance() {
    return mInstance;
  }

  public PrefManager getPref() {
    if (pref == null) {
      pref = new PrefManager(this);
    }

    return pref;
  }
}
```

Apabila pada aplikasi anda menggunakan class App yang mengextends Application tadi, jangan lupa tambahkan <code>android:name=”Nama Class Yang Mengextends Application”</code> di <code>AndroidManifest.xml</code> seperti dibawah ini:

```xml
...
<application android:name=".app.App" ... >
    ...
</application>
...
```

Nah, Setelah selesai membuat semuanya yang ada di atas, sekarang waktunya mengeksekusi shared preferences-nya tadi di Activity atau Fragment anda. Dan kali ini saya mengeksekusinya lewat Activity.

Berikut potongan kode untuk menyimpan nama dan score ke dalam shared preferences:

```java
@OnClick(R.id.btnSave) void save() {
  String name = etName.getText().toString();
  String score = etScore.getText().toString();

  if (name.length() > 0 || score.length() > 0) {
    pref.setName(name);
    pref.setScore(Integer.parseInt(score));
    showToast("Tersimpan");
  } else {
    showToast("Coba isi semua dulu ya");
  }
}
```

Berikut potongan kode untuk menampilkan data yang ada di shared preferences:

```java
@OnClick(R.id.btnShow) void show() {
  showToast(pref.getName() + " " + pref.getScore());
}
```

Berikut potongan kode untuk mereset ulang data yang ada di shared preferences:

```java
@OnClick(R.id.btnReset) void reset() {
  pref.setName(Constant.DEFAULT_NAME);
  pref.setScore(Constant.DEFAULT_SCORE);
  showToast("Reset");
}
```

Berikut kode lengkapnya <code>MainActivity.java</code>:

```java
public class MainActivity extends AppCompatActivity {
  @Bind(R.id.toolbar) Toolbar toolbar;
  @Bind(R.id.etName) EditText etName;
  @Bind(R.id.etScore) EditText etScore;
  private PrefManager pref;

  @Override
  protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.activity_main);
    ButterKnife.bind(this);

    setSupportActionBar(toolbar);

    pref = App.getInstance().getPref();

  }

  private void showToast(String preferences) {
    Toast.makeText(this, preferences, Toast.LENGTH_SHORT).show();
  }

  @OnClick(R.id.btnSave) void save() {
    String name = etName.getText().toString();
    String score = etScore.getText().toString();

    if (name.length() > 0 || score.length() > 0) {
        pref.setName(name);
        pref.setScore(Integer.parseInt(score));
        showToast("Tersimpan");
    } else {
        showToast("Coba isi semua dulu ya");
    }
  }

  @OnClick(R.id.btnShow) void show() {
    showToast(pref.getName() + " " + pref.getScore());
  }

  @OnClick(R.id.btnReset) void reset() {
    pref.setName(Constant.DEFAULT_NAME);
    pref.setScore(Constant.DEFAULT_SCORE);
    showToast("Reset");
  }

  @Override
  public boolean onCreateOptionsMenu(Menu menu) {
    // Inflate the menu; this adds items to the action bar if it is present.
    getMenuInflater().inflate(R.menu.menu_main, menu);
    return true;
  }

  @Override
  public boolean onOptionsItemSelected(MenuItem item) {
    // Handle action bar item clicks here. The action bar will
    // automatically handle clicks on the Home/Up button, so long
    // as you specify a parent activity in AndroidManifest.xml.
    int id = item.getItemId();

    //noinspection SimplifiableIfStatement
    if (id == R.id.action_settings) {
        return true;
    }

    return super.onOptionsItemSelected(item);
  }
}
```

Nah selesai sudah kita belajar shared preferences di android. Sekarang tinggal kita jalankan aplikasinya. Untuk hasilnya bisa dilihat pada video di atas.

Semoga bermanfaat.
