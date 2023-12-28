---
title: Tutorial Android Material Design
description: Tutorial membuat aplikasi android dengan menggunakan android design support library.
date: 2015-10-09
tags:
  - tutorial
---
Tahu kan sekarang lagi musimnya material design? Mau tidak mau juga harus mengikutinya biar menjadi kekinian. Disini kita akan membuat aplikasi android dengan menggunakan android design support library, seperti : <code>NavigationView</code>, <code>TabLayout</code>, <code>TextInputLayout</code>, <code>Snackbar</code>, <code>CoordinatorLayout</code>, <code>AppBarLayout</code>, <code>CollapsingToolbarLayout</code> dan <code>FloatingActionButton</code> yang akan bermaterialan.

<div class="video">
	<iframe width="560" height="315" src="https://www.youtube.com/embed/kWgh6kEHDK0" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen=""></iframe>
</div>

### build.gradle

Yang pertama adalah menambahkan library ke dalam project kita. Jika baru pertama kali / sewaktu update ke versi lainnya, pastikan terhubung dengan koneksi internet. Edit <code>build.gradle</code> seperti berikut ini:

```java
public class Constant {
  dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    compile 'com.android.support:appcompat-v7:22.2.1'
    compile 'com.android.support:design:22.2.1' //tambahkan ini dan gunakanlah versi terbaru
    compile 'com.jakewharton:butterknife:7.0.1' //opsional
    compile 'de.hdodenhof:circleimageview:1.3.0' //opsional
  }
}
```

Setelah berhasil menambahkan library kedalam project kita. Selanjutnya kita mulai koding rianya.

### strings.xml

```xml
<resources>
  <string name="app_name">Njajal Material</string>
  <string name="hello_world">Hello world!</string>
  <string name="action_settings">Settings</string>
  <string name="open">Open</string>
  <string name="close">Close</string>
</resources>
```

String open dan close akan digunakan di <code>ActionBarDrawerToggle()</code>

### color.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
  <color name="primary">#ff2f88b5</color>
  <color name="primary_dark">#ff29769f</color>
  <color name="accent">#ffff61cd</color>
</resources>
```

### styles.xml (values)

Jika anda membuat aplikasi dengan versi minimum SDK kurang dari 21 alangkah lebih baiknya buat 2 buah <code>styles.xml</code>, yang satu pada folder values (default), dan yang satunya pada folder values-v21.

```xml
<resources>
  <!-- Base application theme. -->
  <img src="data:image/gif;base64,R0lGODlhAQABAIAAAAAAAP///yH5BAEAAAAALAAAAAABAAEAAAIBRAA7" data-wp-preserve="%3Cstyle%20name%3D%22AppTheme%22%20parent%3D%22Theme.AppCompat.Light.DarkActionBar%22%3E%0A%20%20%20%20%20%20%20%20%3C!--%20Customize%20your%20theme%20here.--%3E%0A%20%20%20%20%20%20%20%20%3Citem%20name%3D%22windowNoTitle%22%3Etrue%3C%2Fitem%3E%0A%20%20%20%20%20%20%20%20%3Citem%20name%3D%22windowActionBar%22%3Efalse%3C%2Fitem%3E%0A%20%20%20%20%20%20%20%20%3Citem%20name%3D%22colorPrimary%22%3E%40color%2Fprimary%3C%2Fitem%3E%0A%20%20%20%20%20%20%20%20%3Citem%20name%3D%22colorPrimaryDark%22%3E%40color%2Fprimary_dark%3C%2Fitem%3E%0A%20%20%20%20%20%20%20%20%3Citem%20name%3D%22colorAccent%22%3E%40color%2Faccent%3C%2Fitem%3E%0A%20%20%20%20%3C%2Fstyle%3E" data-mce-resize="false" data-mce-placeholder="1" class="mce-object" width="20" height="20" alt="&amp;lt;style&amp;gt;" title="&amp;lt;style&amp;gt;" />
</resources>
```

### styles.xml (values-v21)

Pada <code>styles.xml</code> di folder values-v21 tambahkan baris dibawah ini untuk membuat <code>DrawerLayout</code> dan <code>NavigationView</code> fullscreen ketika dibuka, sehingga warna <code>statusBar</code> mengikuti header dari <code>NavigationView</code>.

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
  <img src="data:image/gif;base64,R0lGODlhAQABAIAAAAAAAP///yH5BAEAAAAALAAAAAABAAEAAAIBRAA7" data-wp-preserve="%3Cstyle%20name%3D%22AppTheme%22%20parent%3D%22Theme.AppCompat.Light.DarkActionBar%22%3E%0A%20%20%20%20%20%20%20%20...%0A%20%20%20%20%20%20%20%20%3Citem%20name%3D%22android%3AwindowDrawsSystemBarBackgrounds%22%3Etrue%3C%2Fitem%3E%0A%20%20%20%20%20%20%20%20%3Citem%20name%3D%22android%3AstatusBarColor%22%3E%40android%3Acolor%2Ftransparent%3C%2Fitem%3E%0A%20%20%20%20%3C%2Fstyle%3E" data-mce-resize="false" data-mce-placeholder="1" class="mce-object" width="20" height="20" alt="&amp;lt;style&amp;gt;" title="&amp;lt;style&amp;gt;" />
</resources>
```

### menu_drawer.xml

<code class="language-plaintext highlighter-rouge">menu_drawer.xml</code> saya gunakan untuk menu didalam <code class="language-plaintext highlighter-rouge">NavigationView</code>.


```xml
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">
  <group android:checkableBehavior="single">
    <item android:id="@+id/nav_home" android:icon="@drawable/ic_home_black_24dp" android:title="Home" />
    <item android:id="@+id/nav_messages" android:icon="@drawable/ic_message_black_24dp" android:title="Messages" />
    <item android:id="@+id/nav_friends" android:icon="@drawable/ic_people_black_24dp" android:title="Friends" />
    <item android:id="@+id/nav_communities" android:icon="@drawable/ic_group_work_black_24dp" android:title="Communities" />
  </group>
</menu>
```
