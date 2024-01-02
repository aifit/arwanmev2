---
title: Tutorial Android Material Design
description: Tutorial membuat aplikasi android dengan menggunakan android design support library.
date: 2015-10-09
tags:
  - Tutorial
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

<code>menu_drawer.xml</code> saya gunakan untuk menu didalam <code>NavigationView</code>.


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

### header.xml

<code>menu_drawer.xml</code> saya gunakan untuk menu didalam <code>NavigationView</code>.

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

### header.xml

Header disini merupakan layout yang saya gunakan sebagai header pada <code>NavigationView</code>.

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android" xmlns:app="http://schemas.android.com/apk/res-auto" android:layout_width="match_parent" android:layout_height="200dp" android:background="@drawable/bg" android:gravity="bottom" android:orientation="vertical" android:padding="20dp">
  <de.hdodenhof.circleimageview.CircleImageView android:id="@+id/imgUser" android:layout_width="100dp" android:layout_height="100dp" app:border_color="@android:color/white" app:border_width="2dp" android:src="@drawable/pev" />
  <TextView android:layout_marginTop="5dp" android:layout_width="wrap_content" android:layout_height="wrap_content" android:text="Pevita Pearce" android:textColor="@android:color/white" android:textAppearance="?android:attr/textAppearanceMedium"/>
</LinearLayout>
```

### activity_main.xml

Berikut struktur layout <code>activity_main.xml</code>:

```xml
<android.support.v4.widget.DrawerLayout ... >
  <android.support.design.widget.CoordinatorLayout ... >
    <android.support.design.widget.AppBarLayout ... >
      <android.support.design.widget.CollapsingToolbarLayout ... >
          <ImageView ... />
          <android.support.v7.widget.Toolbar ... />
      </android.support.design.widget.CollapsingToolbarLayout>
      <android.support.design.widget.TabLayout ... />
    </android.support.design.widget.AppBarLayout>
    <android.support.v4.view.ViewPager ... />
    <android.support.design.widget.FloatingActionButton .../>
  </android.support.design.widget.CoordinatorLayout>
  <android.support.design.widget.NavigationView ... />
</android.support.v4.widget.DrawerLayout>
```

Pada <code>CollapsingToolbarLayout</code> untuk defaultnya saya memakai <code>scroll|exitUntilCollapsed</code> agar <code>ImageView</code> yang ada di dalamnya collapsed ketika scroll kebawah dan expanded ketika scroll keatas ketika scroll sudah pada posisi 0 (scroll sampai atas habis). Selain <code>exitUntilCollapsed</code> ada juga <code>enterAlways</code> yang berfungsi ketika scroll kebawah dan keatas akan collapsed dan expanded selalu. Dan ada juga <code>enterAlwaysCollapsed</code>. Alangkah lebih baiknya mencoba semuanya, agar dapat cepat memahami.

```xml
...
  <android.support.design.widget.CollapsingToolbarLayout ... app:layout_scrollFlags="scroll|exitUntilCollapsed">
    ...
  </android.support.design.widget.CollapsingToolbarLayout>
...
```

Untuk memberikan paralax effect pada <code>ImageView</code> yang ada didalam <code>CollapsingToolbarLayout</code> cukup menambahkan:

```xml
...
  <ImageView ... app:layout_collapseMode="parallax" app:layout_collapseParallaxMultiplier="0.7"/>
...

```

Agar Toolbar tidak ikut hilang ketika scroll keatas kita beri kode:

```xml
...
  <android.support.v7.widget.Toolbar ... app:layout_collapseMode="pin"/>
...
```

Sedangkan <code>TabLayout</code> tempatkan di dalam <code>AppBarLayout</code> bukan didalam <code>CollapsingToolbarLayout</code> dan bukan diluar <code>AppbarLayout</code> yang bertujuan agar ketika kita scroll kebawah akan berhenti dibawah Toolbar dan ketika scroll keatas sampai habis, akan mengexpand dan posisi <code>TabLayout</code> akan dibawah <code>ImagView</code>.

```xml
<android.support.design.widget.AppBarLayout ... >
  <android.support.design.widget.CollapsingToolbarLayout ... >
      ...
  </android.support.design.widget.CollapsingToolbarLayout>
  <android.support.design.widget.TabLayout ... />
</android.support.design.widget.AppBarLayout>
```

<code>ViewPager</code> dan <code>FloatingActionButton</code> tempatkan diluar CollapsingToolbarLayout serta jangan lupa menambahkan:

```xml
<android.support.design.widget.CoordinatorLayout ... >
  ...
  <android.support.v4.view.ViewPager ... app:layout_behavior="@string/appbar_scrolling_view_behavior"/>
  <android.support.design.widget.FloatingActionButton ... />
</android.support.design.widget.CoordinatorLayout>
```

Pada ViewPager yang berfungsi sebagai patokan scroll. Ketika di scroll, posisi <code>ViewPager</code> tetap dibawah <code>AppBarLayout</code>. Dan kalau ada widget atau layout yang tertimpa / sembunyi / ketutup, kita bisa memainkan atau menambahkan:

```xml
<android.support.v4.widget.DrawerLayout ... android:fitsSystemWindows="true">
  ...
</android.support.v4.widget.DrawerLayout>
```

Berikut kode lengkap dari <code>activity_main.xml</code>:

```xml
<android.support.v4.widget.DrawerLayout xmlns:android="http://schemas.android.com/apk/res/android" xmlns:app="http://schemas.android.com/apk/res-auto" xmlns:tools="http://schemas.android.com/tools" android:id="@+id/drawerLayout" android:layout_width="match_parent" android:layout_height="match_parent" android:fitsSystemWindows="true" tools:context=".MainActivity">
  <android.support.design.widget.CoordinatorLayout android:id="@+id/coordinatorLayout" android:layout_height="match_parent" android:layout_width="match_parent">
    <android.support.design.widget.AppBarLayout android:layout_width="match_parent" android:layout_height="wrap_content" android:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar">
        <android.support.design.widget.CollapsingToolbarLayout android:id="@+id/collapsingToolbar" android:layout_width="match_parent" android:layout_height="192dp" app:layout_scrollFlags="scroll|exitUntilCollapsed" app:expandedTitleMarginStart="64dp" app:contentScrim="?attr/colorPrimary">
            <ImageView android:id="@+id/img" android:layout_width="match_parent" android:layout_height="match_parent" android:src="@drawable/ra" android:scaleType="centerCrop" app:layout_collapseMode="parallax" app:layout_collapseParallaxMultiplier="0.7"/>
            <android.support.v7.widget.Toolbar android:id="@+id/toolbar" android:layout_width="match_parent" android:layout_height="?actionBarSize" android:theme="@style/ThemeOverlay.AppCompat.Dark" app:popupTheme="@style/ThemeOverlay.AppCompat.Light" app:layout_collapseMode="pin"/>
        </android.support.design.widget.CollapsingToolbarLayout>
        <android.support.design.widget.TabLayout android:id="@+id/tabLayout" android:layout_width="match_parent" android:layout_height="wrap_content" app:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar" android:background="?attr/colorPrimary" app:tabMode="fixed" app:tabGravity="fill"/>
    </android.support.design.widget.AppBarLayout>
    <android.support.v4.view.ViewPager android:id="@+id/viewPager" android:layout_width="match_parent" android:layout_height="match_parent" app:layout_behavior="@string/appbar_scrolling_view_behavior"/>
    <android.support.design.widget.FloatingActionButton android:id="@+id/fab" android:orientation="vertical" android:layout_width="wrap_content" android:layout_height="wrap_content" android:src="@drawable/ic_mode_edit_white_24dp" android:layout_gravity="bottom|end" app:fabSize="normal" android:layout_alignParentBottom="true" android:layout_alignParentRight="true" android:layout_alignParentEnd="true" android:layout_margin="15dp"/>
  </android.support.design.widget.CoordinatorLayout>
  <android.support.design.widget.NavigationView android:id="@+id/navView" android:layout_width="wrap_content" android:layout_height="match_parent" android:layout_gravity="start" android:clickable="true" app:headerLayout="@layout/header" app:menu="@menu/menu_drawer" />
</android.support.v4.widget.DrawerLayout>
```


### fragment_ngopi.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<android.support.v4.widget.NestedScrollView xmlns:android="http://schemas.android.com/apk/res/android" android:layout_width="match_parent" android:layout_height="wrap_content" >
  <LinearLayout android:layout_width="match_parent" android:layout_height="match_parent" android:orientation="vertical">
    <TextView android:layout_width="match_parent" android:layout_height="wrap_content" android:text="lakdclkadca jhbjh kjnkj akdnclakdcla jhkh jjkn ladclakmdclakmdclakmdclakmdclakdmcal lakdmclakmdclkamdclkamdclkmaldkc lakdmclakdclkadlckalkdc kdlckmlkmalkdmclkmlkmlkmadkmlkmclkm kmadlc kdc lkdc alkd clkd clka dclkac lakdc la calkd calkdc alkd calkd clak dcalkcda lakdmclakdmclkad caldclakdcmald calkd calkd calkd clad clakc akdmclakdmcad cla" android:textSize="50dp"/>
  </LinearLayout>
</android.support.v4.widget.NestedScrollView>
```

### fragment_turu.xml

Di layout <code>fragment_turu.xml</code> ini saya sertakan juga <code>TextInputLayout</code> yang juga dari android design support library, dengan menggunakan <code>TextInputLayout</code> sekarang tidak ribet lagi untuk membuat pesan error didalam <code>EditText</code> ataupun membuat <code>TextView</code> untuk keterangan di <code>EditText</code> (cukup menggunakan hint). Cara penggunaannya seperti berikut ini:

```xml
<android.support.design.widget.TextInputLayout android:layout_width="match_parent" android:layout_height="wrap_content">
  <EditText android:layout_width="match_parent" android:layout_height="wrap_content" android:inputType="textPersonName" android:hint="Title"/>
</android.support.design.widget.TextInputLayout>
```

Berikut kode lengkap layout <code>fragment_turu.xml</code>:

```xml
<?xml version="1.0" encoding="utf-8"?>
<android.support.v4.widget.NestedScrollView xmlns:android="http://schemas.android.com/apk/res/android" android:layout_width="match_parent" android:layout_height="wrap_content" >
  <LinearLayout android:layout_width="match_parent" android:layout_height="match_parent" android:orientation="vertical">
    <android.support.design.widget.TextInputLayout android:layout_width="match_parent" android:layout_height="wrap_content">
      <EditText android:layout_width="match_parent" android:layout_height="wrap_content" android:inputType="textPersonName" android:hint="Title"/>
    </android.support.design.widget.TextInputLayout>
    <android.support.design.widget.TextInputLayout android:layout_width="match_parent" android:layout_height="wrap_content">
      <EditText android:layout_width="match_parent" android:layout_height="wrap_content" android:inputType="textPersonName" android:hint="Date"/>
    </android.support.design.widget.TextInputLayout>
    <android.support.design.widget.TextInputLayout android:layout_width="match_parent" android:layout_height="wrap_content">
      <EditText android:layout_width="match_parent" android:layout_height="wrap_content" android:inputType="textPersonName" android:hint="Descripion"/>
    </android.support.design.widget.TextInputLayout>
  </LinearLayout>
</android.support.v4.widget.NestedScrollView>
```

### NgopiFragment.java

```java
public class NgopiFragment extends Fragment {
  public NgopiFragment() {}
  @Override
  public View onCreateView(LayoutInflater inflater, ViewGroup container,
    Bundle savedInstanceState) {
    return inflater.inflate(R.layout.fragment_ngopi, container, false);
  }
}
```

### TuruFragment.java

```java
public class TuruFragment extends Fragment {
  public TuruFragment() {}
  @Override
  public View onCreateView(LayoutInflater inflater, ViewGroup container,
    Bundle savedInstanceState) {
    return inflater.inflate(R.layout.fragment_turu, container, false);
  }
}
```

### ViewPagerAdapter.java

```java
public class ViewPagerAdapter extends FragmentPagerAdapter {
	private String[] TAB_TITLE = {"Ngopi", "Turu"};

	public ViewPagerAdapter(FragmentManager fm) {
		super(fm);
	}

	@Override
	public Fragment getItem(int position) {
		switch (position) {
			case 0:
				return new NgopiFragment();
			case 1:
				return new TuruFragment();
		}
		return null;
	}

	@Override
	public int getCount() {
		return TAB_TITLE.length;
	}

	@Override
	public CharSequence getPageTitle(int position) {
		return TAB_TITLE[position];
	}
}
```

### MainActivity.java

#### Menampilkan Snackbar:

```java
Snackbar.make(v,"Tenan pora?", Snackbar.LENGTH_LONG)
	.setAction("Iyo leh", this)
	.show();
```

#### Setup Toolbar:

```java
setSupportActionBar(toolbar);
getSupportActionBar().setDisplayHomeAsUpEnabled(true);
getSupportActionBar().setHomeAsUpIndicator(R.drawable.ic_menu_white_24dp);
```

#### Setup ActionBarDrawerToggle:

```java
ActionBarDrawerToggle toggle = new ActionBarDrawerToggle(
	this,
	drawerLayout,
	toolbar,
	R.string.open,
	R.string.close
);

drawerLayout.setDrawerListener(toggle);
toggle.syncState();
```

#### Setup NavigationView:

```java
//yang di cek pertama
navView.getMenu().getItem(0).setChecked(true);

navView.setNavigationItemSelectedListener(
	new NavigationView.OnNavigationItemSelectedListener() {
		@Override
		public boolean onNavigationItemSelected(MenuItem menuItem) {
				menuItem.setChecked(true);
				drawerLayout.closeDrawers();
				int id = menuItem.getItemId();
				switch (id) {
					case R.id.nav_home:
						showToast("Nav Home");
						break;
					case R.id.nav_messages:
						showToast("Nav Messages");
						break;
					case R.id.nav_friends:
						showToast("Nav Friends");
						break;
					case R.id.nav_communities:
						showToast("Nav Communities");
						break;
				}
			return false;
		}
	}
);
```


#### Setup CollapsingToolbarLayout title:

```java
collapsingToolbar.setTitle(toolbar.getTitle());
```

#### Setup TabLayout:

```java
viewPager.setAdapter(new ViewPagerAdapter(getSupportFragmentManager()));
tabLayout.setupWithViewPager(viewPager);
```

Berikut kode lengkap dari <code>MainActivity.java</code>:

```java
public class MainActivity extends AppCompatActivity implements View.OnClickListener {
	@Bind(R.id.toolbar)
	Toolbar toolbar;
	@Bind(R.id.drawerLayout)
	DrawerLayout drawerLayout;
	@Bind(R.id.collapsingToolbar)
	CollapsingToolbarLayout collapsingToolbar;
	@Bind(R.id.navView)
	NavigationView navView;
	@Bind(R.id.tabLayout)
	TabLayout tabLayout;
	@Bind(R.id.viewPager)
	ViewPager viewPager;

	@OnClick(R.id.fab) void fabOnClick(View v) {
		Snackbar.make(v,"Tenan pora?", Snackbar.LENGTH_LONG)
		.setAction("Iyo leh", this)
		.show();
	}

	@Override
	protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.activity_main);

		ButterKnife.bind(this);

		setupToolbarToggle();
		setupNavView();
		setupCollapseToolbar();
		setupTabLayout();
	}

	private void setupToolbarToggle() {
		setSupportActionBar(toolbar);
		getSupportActionBar().setDisplayHomeAsUpEnabled(true);
		getSupportActionBar().setHomeAsUpIndicator(R.drawable.ic_menu_white_24dp);

		ActionBarDrawerToggle toggle = new ActionBarDrawerToggle(
			this,
			drawerLayout,
			toolbar,
			R.string.open,
			R.string.close
		);

		drawerLayout.setDrawerListener(toggle);
		toggle.syncState();
	}

	private void setupNavView() {
		//yang di cek pertama
		navView.getMenu().getItem(0).setChecked(true);

		navView.setNavigationItemSelectedListener(
			new NavigationView.OnNavigationItemSelectedListener() {
				@Override
				public boolean onNavigationItemSelected(MenuItem menuItem) {
					menuItem.setChecked(true);
					drawerLayout.closeDrawers();
					int id = menuItem.getItemId();
					switch (id) {
						case R.id.nav_home:
							showToast("Nav Home");
							break;
						case R.id.nav_messages:
							showToast("Nav Messages");
							break;
						case R.id.nav_friends:
							showToast("Nav Friends");
							break;
						case R.id.nav_communities:
							showToast("Nav Communities");
							break;
						}
					return false;
				}
			}
		);
	}

	private void setupCollapseToolbar() {
		collapsingToolbar.setTitle(toolbar.getTitle());
	}

	private void setupTabLayout() {
		viewPager.setAdapter(new ViewPagerAdapter(getSupportFragmentManager()));
		tabLayout.setupWithViewPager(viewPager);
	}

	private void showToast(String toast) {
		Toast.makeText(this, toast, Toast.LENGTH_SHORT).show();
	}

	@Override
	public boolean onCreateOptionsMenu(Menu menu) {
		getMenuInflater().inflate(R.menu.menu_main, menu);
		return true;
	}

	@Override
	public boolean onOptionsItemSelected(MenuItem item) {
		switch (item.getItemId()) {
			case android.R.id.home:
				drawerLayout.openDrawer(GravityCompat.START);
				return true;
			case R.id.action_settings:
				return true;
		}
		return super.onOptionsItemSelected(item);
	}

	@Override
	public void onClick(View v) {}
}
```

Setelah selesai semuanya, sekarang tinggal jalankan aplikasinya. Untuk hasilnya bisa lihat video diatas.

Ternyata dengan Android design support library kita tidak dibuat <i>njelimet</i> lagi untuk membuat ini itu, hanya dengan bermain layout, aplikasi yang kita buat bisa jadi lebih menarik dan kekinian.

Semoga bermanfaat.
