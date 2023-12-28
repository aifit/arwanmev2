---
title: Tutorial Android CRUD Realm Database
description: Tutorial membuat aplikasi android dengan menggunakan android design support library.
date: 2015-10-10
tags:
  - tutorial
---

Halo, kali ini kita akan belajar CRUD sederhana menggunakan database Realm di android, yang saya gunakan sebagai pengganti SQLite dalam membuat aplikasi android yang berhubungan dengan database.

<div class="video">
	<iframe width="560" height="315" src="https://www.youtube.com/embed/cxeF7opnqCc" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen=""></iframe>
</div>

Baik, yang pertama adalah mengedit <code>build.gradle</code> untuk menambahkan Realm itu sendiri. Dan pastikan mempunyai koneksi internet.

```java
android {
  ...
	packagingOptions {
		exclude 'META-INF/services/javax.annotation.processing.Processor'
	}
}

dependencies {
	compile fileTree(dir: 'libs', include: ['*.jar'])
	compile 'com.android.support:appcompat-v7:22.2.1'
	//Optional
	compile 'com.android.support:design:22.2.1'
	compile 'com.android.support:recyclerview-v7:22.2.1'
	compile 'com.android.support:cardview-v7:22.2.1'
	compile 'com.jakewharton:butterknife:7.0.1'
	//Tambahkan ini dan gunakan versi terbaru
	compile 'io.realm:realm-android:0.82.0'
}
```

Setelah menambahkan dependency, selanjutnya kita membuat konfigurasi default Realm ke dalam class yang meng-extends Application.

```java
public class App extends Application {
	@Override
	public void onCreate() {
		super.onCreate();
		RealmConfiguration config = new RealmConfiguration.Builder(this).build();
		Realm.setDefaultConfiguration(config);
	}
}
```

Serta pada <code>AndroidManifest.xml</code> tambahkan class yang mengextends Application tersebut, seperti dibawah ini:

```xml
...
<application
  android:name=".app.App"
	... >
  ...
</application>
...
```

Selanjutnya, disini saya membuat class <code>Article</code> sebagai Realm data models dengan mengextends <code>RealmObject</code>, dan juga saya menggunakan id sebagai <code>PrimaryKey</code>-nya.

```java
public class Article extends RealmObject {
	@PrimaryKey
	private String id;
	private String title;
	private String description;

	public void setId(String id) {
		this.id = id;
	}

	public String getId() {
		return id;
	}

	public void setTitle(String title) {
		this.title = title;
	}

	public String getTitle() {
		return title;
	}

	public void setDescription(String description) {
		this.description = description;
	}

	public String getDescription() {
		return description;
	}
}
```

Setelah membuat class-class yang diperlukan di atas, sekarang kita akan membuat CRUD-nya. Sebelumnya tambahkan baris dibawah ini ke dalam <code>onCreate()</code> nya Activity atau Fragment kalian:


```java
private Realm realm;

@Override
protected void onCreate(Bundle savedInstanceState) {
  super.onCreate(savedInstanceState);
  ...
  realm = Realm.getDefaultInstance();
  ...
}
```

### Menambahkan data Article:

```java
private void addArticle(String title, String description) {
	realm.beginTransaction();

	Article article = realm.createObject(Article.class);
	article.setId(Utils.getCurrentTimestamp());
	article.setTitle(title);
	article.setDescription(description);
	articleAdapter.add(article);

	realm.commitTransaction();

	showToast("Added : " + title);
}
```

### Menampilkan semua data Article:

```java
private void findAllArticle() {
	realmResults = realm.where(Article.class).findAll();
	realmResults.sort("id", RealmResults.SORT_ORDER_DESCENDING);
	articleAdapter.addAll(realmResults);

	showToast("Size : " + String.valueOf(realmResults.size()));
}
```

### Mengupdate data Article:

```java
private void updateArticle(String id, String title, String description) {
	realm.beginTransaction();

	Article article = realm.where(Article.class).equalTo("id", id).findFirst();
	article.setTitle(title);
	article.setDescription(description);
	articleAdapter.update();

	realm.commitTransaction();

	showToast("Updated : " + id);
}
```

Menghapus data Article:

```java
private void deleteArticle(int position) {
	realm.beginTransaction();

	realmResults.remove(position);
	articleAdapter.remove(position);

	realm.commitTransaction();

	showToast("Deleted position : " + position);
}
```

Pada potongan kode diatas terdapat <code>articleAdapter</code>, disitu saya menggunakan <code>RecyclerView</code> Adapter untuk memberitahukan ke adapter <code>notifyDataSetChanged()</code> setiap menghapus, mengedit, menambah ataupun menampilkan.

Yang harus diingat, setiap kali kita akan menambahkan, mengedit atau menghapus data selalu gunakan:

```java
//dimulai dengan
realm.beginTransaction();

...

//ditutup dengan
realm.commitTransaction();
```

Atau jika sedang malas atau lupa atau yang lainnya untuk membuka / menutup, kita juga dapat menggunakan:

```java
realm.executeTransaction(new Realm.Transaction() {
  @Override
		public void execute(Realm realm) {
			...
		}
	}
);
```

Sekarang tinggal kita jalankan aplikasinya. Untuk hasilnya bisa dilihat pada video diatas.

Nah, selesai sudah tutorial CRUD menggunakan Realm Database kali ini. Ternyata sangat simpel, cepat dan sederhana sekali ya? Dan yang asyik disini kita bisa melakukan query tanpa syntax SQL.

Semoga bermanfaat.
