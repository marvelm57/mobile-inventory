# Tugas 7
##  Apa perbedaan utama antara _stateless_ dan _stateful widget_ dalam konteks pengembangan aplikasi Flutter?

| Stateless Widget          | Stateful Widget           |
| ------------------------- | ------------------------- |
| Widget yang tampilannya tidak pernah berubah. | Widget yang berubah tampilannya ketika ada interaksi dari user.|
| Digunakan ketika memiliki komponen yang tidak perlu menyimpan data atau keadaan dalam aplikasi. | Digunakan ketika  memiliki komponen yang perlu menyimpan dan memperbarui data atau keadaan dalam aplikasi. |
| Contohnya adalah Text, Icon, dan IconButton. | Contohnya adalah Checkbox, Radio, Form, dan TextField. |

## Sebutkan seluruh widget yang kamu gunakan untuk menyelesaikan tugas ini dan jelaskan fungsinya masing-masing.
- Scaffold: struktur dasar aplikasi yang menyediakan kerangka kerja untuk halaman MyHomePage
- AppBar: container yang menampilkan konten di paling atas layar
- Text: menampilkan text
- SingleChildScrollView: membungkus konten agar dapat discroll
- Padding: memberikan padding ke kontennya
- Column: menampilkan bagian children secara vertikal
- GridView: membuat tata letak grid dengan jumlah kolom yang ditentukan
- InkWell: membuat area responsif terhadap sentuhan pengguna
- SnackBar: menampilkan pesan kilat di bagian bawah layar saat pengguna mengklik item toko
- Container: menyimpan ikon dan text dari item
- Icon: menampilkan ikon dari item toko


## Jelaskan bagaimana cara kamu mengimplementasikan checklist di atas secara step-by-step (bukan hanya sekadar mengikuti tutorial)
- Menjalankan perintah `flutter create <APP_NAME>` pada terminal untuk membuat projek flutter baru dan masuk ke direktori proyek yang telah dibuat dengan menjalankan perintah `cd <APP_NAME>` dengan nama app *mobile_inventory*
- Membuat file baru bernama `menu.dart` di dalam direktori `mobile_inventory/lib` dan meng-_import_ `import 'package:flutter/material.dart';`
- Membuka file `main.dart` dan memindahkan potongan kode `class MyHomePage ...` dan `class _MyHomePageState ...` ke file `menu.dart`
- Meng-_import_ `import 'package:mobile_inventory/menu.dart';` pada file `main.dart`
- Menghapus kode `MyHomePage(title: 'Flutter Demo Home Page')` pada `main.dart` menjadi `MyHomePage()`
- Pada file `menu.dart`, ubah sifat _widget_ halaman dari _stateful_ menjadi _stateless_ dengan mengganti kode `({super.key, required this.title})` menjadi `({Key? key}) : super(key: key);` dan mengapus kode `final String title; ` sampai bawah serta tambahkan Widget build.
- Untuk menambahkan teks dan card, buat class bernama ShopItem di file `menu.dart` dengan atribut name, icon, dan color . Kemudian, membuat list items berisi 3 objek ShopItem di bawah kode `MyHomePage({Key? key}) : super(key: key);`
- Selanjutnya tambahkan kode di bawah ini di dalam Widget build
```
    return Scaffold(
      appBar: AppBar(
        title: const Text(
          'Shopping List',
        ),
      ),
      body: SingleChildScrollView(
        // Widget wrapper yang dapat discroll
        child: Padding(
          padding: const EdgeInsets.all(10.0), // Set padding dari halaman
          child: Column(
            // Widget untuk menampilkan children secara vertikal
            children: <Widget>[
              const Padding(
                padding: EdgeInsets.only(top: 10.0, bottom: 10.0),
                // Widget Text untuk menampilkan tulisan dengan alignment center dan style yang sesuai
                child: Text(
                  'PBP Shop', // Text yang menandakan toko
                  textAlign: TextAlign.center,
                  style: TextStyle(
                    fontSize: 30,
                    fontWeight: FontWeight.bold,
                  ),
                ),
              ),
              // Grid layout
              GridView.count(
                // Container pada card kita.
                primary: true,
                padding: const EdgeInsets.all(20),
                crossAxisSpacing: 10,
                mainAxisSpacing: 10,
                crossAxisCount: 3,
                shrinkWrap: true,
                children: items.map((ShopItem item) {
                  // Iterasi untuk setiap item
                  return ShopCard(item);
                }).toList(),
              ),
            ],
          ),
        ),
      ),
    );
```
- Terakhir, buat _widget stateless_ baru untuk menampilkan card dengan menambahkan kode di bawah ini
```
class ShopCard extends StatelessWidget {
  final ShopItem item;

  const ShopCard(this.item, {super.key}); // Constructor

  @override
  Widget build(BuildContext context) {
    return Material(
      color: Colors.indigo,
      child: InkWell(
        // Area responsive terhadap sentuhan
        onTap: () {
          // Memunculkan SnackBar ketika diklik
          ScaffoldMessenger.of(context)
            ..hideCurrentSnackBar()
            ..showSnackBar(SnackBar(
                content: Text("Kamu telah menekan tombol ${item.name}!")));
        },
        child: Container(
          // Container untuk menyimpan Icon dan Text
          padding: const EdgeInsets.all(8),
          child: Center(
            child: Column(
              mainAxisAlignment: MainAxisAlignment.center,
              children: [
                Icon(
                  item.icon,
                  color: Colors.white,
                  size: 30.0,
                ),
                const Padding(padding: EdgeInsets.all(3)),
                Text(
                  item.name,
                  textAlign: TextAlign.center,
                  style: const TextStyle(color: Colors.white),
                ),
              ],
            ),
          ),
        ),
      ),
    );
  }
}
```
# Tugas 8
## Jelaskan perbedaan antara Navigator.push() dan Navigator.pushReplacement(), disertai dengan contoh mengenai penggunaan kedua metode tersebut yang tepat!
### `Navigator.push()`
- Metode yang digunakan untuk menambahkan suatu _route_ ke dalam stack _route_ yang dikelola oleh `Navigator`. _Route_ yang ditambahkan akan berada di paling atas stack dan ditampilkan kepada pengguna.
Contoh:
```
if (item.name == "Tambah Item") {
  Navigator.push(context,
    MaterialPageRoute(builder: (context) => const ShopFormPage()));
}
```

### `Navigator.pushReplacement()`
- Metode yang digunakan untuk menghapus _route_ yang sedang ditampilkan kepada pengguna dan menggantinya dengan suatu _route_ baru. Pada stack _route_ yang dikelola `Navigator`, _route_ lama pada atas stack akan digantikan secara langsung oleh _route_ baru tanpa mengubah kondisi elemen stack yang berada di bawahnya.
Contoh:
```
onTap: () {
  Navigator.pushReplacement(
  context,
  MaterialPageRoute(
    builder: (context) => const ShopFormPage(),
  ));
},
```

## Jelaskan masing-masing layout widget pada Flutter dan konteks penggunaannya masing-masing!
1. Container: Widget yang dapat digunakan untuk mengatur tata letak dan penataan elemen-elemen lainnya.
2. Row dan Column: Digunakan untuk menyusun widget secara horizontal (Row) atau vertikal (Column).
3. Padding: Digunakan untuk menambahkan ruang padding (ruang putih) di sekeliling widget anaknya
4. ListView: Digunakan untuk membuat daftar elemen yang dapat di-scroll.
5. GridView: Digunakan untuk menampilkan data dalam bentuk grid atau tabel.
6. Stack: Digunakan untuk menumpuk widget di atas satu sama lain.
7. Align: Digunakan untuk mengatur posisi widget anak dengan mengacu pada tepi atau sudut tertentu.
8. SingleChildScrollView: Digunakan untuk membuat area tata letak yang dapat di-scroll dengan satu widget anak.

## Sebutkan apa saja elemen input pada form yang dipakai pada tugas kali ini dan jelaskan mengapa menggunakan elemen input tersebut!
- `TextFormField` untuk Nama Item karena kita ingin pengguna dapat memasukkan teks sebagai nama item, serta `validator` yang digunakan untuk memastikan bahwa input tidak boleh kosong.
- `TextFormField` untuk Harga Item karena kita ingin mengambil input angka sebagai harga item, serta `validator` yang digunakan untuk memastikan bahwa input bukan kosong dan merupakan angka.
- `TextFormField` untuk Deskripsi Item karena kita ingin pengguna dapat memasukkan teks sebagai deskripsi item, serta `validator` digunakan untuk memastikan bahwa input tidak boleh kosong.


## Bagaimana penerapan clean architecture pada aplikasi Flutter?
Clean architecture pada Flutter dapat diterapkan dengan membagi proyek menjadi tiga modul utama:
1. *Presentasion Layer*: 
- Berisi komponen antarmuka pengguna, seperti widget, layar, dan tampilan
- Bertanggung jawab untuk menangani interaksi pengguna dan merender antarmuka pengguna. 
- Tidak bergantung pada logika bisnis dan detail implementasi akses data.

2. *Domain Layer (Business Logic)*: 
- Mewakili logika bisnis inti dari aplikasi. 
- Berisi use case yang menentukan operasi atau tindakan yang dapat dilakukan dalam aplikasi.
- Tidak boleh memiliki ketergantungan pada lapisan presentasi atau data.

3. *Data Layer*:
- Bertanggung jawab atas pengambilan dan penyimpanan data. 
- Terdiri dari repositori dan sumber data.
- Melindungi lapisan domain dari detail penyimpanan dan pengambilan data

## Jelaskan bagaimana cara kamu mengimplementasikan checklist di atas secara step-by-step! (bukan hanya sekadar mengikuti tutorial)
- Membuat berkas baru `shoplist_form.dart` pada direktori `lib`. Kemudian, buat sebuah form sederhana menggunakan widget `Form` yang berfungsi sebagai wadah bagi beberapa input field widget dan tambahkan widget `SingleChildScrollView` untuk membuat child widget di dalamnya menjadi scrollable.
- Buat variabel baru bernama `_formKey` lalu tambahkan `_formKey` tersebut ke dalam atribut `key` milik widget `Form`. Atribut `key` akan berfungsi sebagai handler dari form state, validasi form, dan penyimpanan form
- Selanjutnya, isi widget `Form` dengan field dan buat variabel `_name`, `_price`, dan `_description` untuk menyimpan input dari masing-masing field yang akan dibuat.
- Buat widget `Column` sebagai child dari `SingleChildScrollView`. Kemudian, buat tiga widget `TextFormField` yang dibungkus oleh `Padding` sebagai child dari `Column` untuk field `name`, `price`, `description`. Setelah itu, tambahkan atribut `crossAxisAlignment` untuk mengatur alignment children dari `Column`. Terakhir, buat tombol yang dibungkus ke dalam widget `Padding` dan `Align` sebagai child selanjutnya dari `Column` yang akan memunculkan pop-up berisi data yang telah diinput apabila ditekan.
- Setelah membuat form, buka berkas `menu.dart` pada `lib` dan tambahkan navigasi pada tombol `Tambah Item`. Navigasi yang digunakan adalah   `Navigator.push()` sehingga pengguna dapat menekan tombol Back untuk kembali ke halaman menu.
- Selanjutnya untuk membuat drawer, buat direktori baru `widgets` di dalam `lib` dan buat berkas baru `left_drawer.dart` di dalam  `widgets`. Tambahkan kode berikut pada berkas:
```
import 'package:flutter/material.dart';

class LeftDrawer extends StatelessWidget {
  const LeftDrawer({super.key});

  @override
  Widget build(BuildContext context) {
    return Drawer(
      child: ListView(
        children: [
          const DrawerHeader(
            // TODO: Bagian drawer header
          ),
          // TODO: Bagian routing
        ],
      ),
    );
  }
}
```
- Tambahkan impor untuk halaman-halaman yang kita ingin masukkan navigasinya ke dalam Drawer Menu, yaitu halaman `MyHomePage` dan `ShopFormPage`. Kemudian, masukkan routing untuk halaman-halaman tersebut pada bagian routing dengan menggunakan navigasi `Navigator.pushReplacement()`. Selanjutnya, hias drawer dengan memasukkan drawer header pada bagian drawer header.
- Kemudian, masukkan drawer yang telah dibuat ke halaman yang ingin ditambahkan drawer. Contohnya seperti berikut untuk menambahkan drawer pada berkas `menu.dart`,
```
// Impor drawer widget
import 'package:shopping_list/widgets/left_drawer.dart';
...
return Scaffold(
  appBar: AppBar(
    title: const Text(
      'Shopping List',
    ),
    backgroundColor: Colors.indigo,
    foregroundColor: Colors.white,
  ),
  // Masukkan drawer sebagai parameter nilai drawer dari widget Scaffold
  drawer: const LeftDrawer(),
```
- Terakhir, buat berkas `shop_card.dart` pada direktori `widgets` dan pindahkan isi widget `ShopItem` pada `menu.dart` ke berkas `widgets/shop_card.dart`. Pastikan untuk mengimpor halaman `shoplist_form.dart` pada berkas `shop_card.dart` dan import halaman `shop_card.dart` pada berkas `menu.dart`. Buat direktori baru `screens` dan pindahkan file `menu.dart` dan `shoplist_form.dart` ke dalam direktori tersebut.

# Tugas 9
## Apakah bisa kita melakukan pengambilan data JSON tanpa membuat model terlebih dahulu? Jika iya, apakah hal tersebut lebih baik daripada membuat model sebelum melakukan pengambilan data JSON?
Iya, kita dapat mengambil data `JSON` tanpa membuat model terlebih dahulu. Jika hanya perlu mengakses dan menggunakan data `JSON` tanpa pemodelan yang kompleks, maka tidak membuat model dapat lebih efisien dan memudahkan pengembangan. Namun, jika memiliki kebutuhan yang lebih kompleks, seperti membuat objek data baru, pembuatan model mungkin diperlukan.

## Jelaskan fungsi dari CookieRequest dan jelaskan mengapa instance CookieRequest perlu untuk dibagikan ke semua komponen di aplikasi Flutter.
`CookieRequest` dapat berfungsi untuk menyimpan informasi autentikasi, seperti token akses, sesi, atau _cookies_ lainnya. Dengan membagikan _instance_ ini ke seluruh komponen, aplikasi dapat dengan mudah mengakses dan memperbarui informasi autentikasi tanpa perlu menyimpannya di setiap widget secara terpisah.

## Jelaskan mekanisme pengambilan data dari JSON hingga dapat ditampilkan pada Flutter.
1. Menambahkan dependensi http ke proyek Flutter; dependensi ini digunakan untuk bertukar HTTP request.
2. Membuat model sesuai dengan respons dari data (berupa JSON) yang berasal dari _web service_.
3. Membuat http request ke _web service_ menggunakan dependensi http.
4. Mengkonversi objek yang didapatkan dari _web service_, yaitu JSON ke model yang telah kita buat di langkah kedua.
5. Menampilkan data yang telah dikonversi ke aplikasi dengan ‘FutureBuilder’.

## Jelaskan mekanisme autentikasi dari input data akun pada Flutter ke Django hingga selesainya proses autentikasi oleh Django dan tampilnya menu pada Flutter.
1. Membuat instance `CookieRequest` untuk mengelola permintaan HTTP dan informasi cookie.
2. Setelah melakukan input data dan menekan tombol login, Flutter mengambil nilai dari `TextField` yang berisi nama pengguna dan kata sandi.
3. Selanjutnya, Flutter menggunakan `CookieRequest` untuk mengirim permintaan autentikasi ke endpoint Django.
4. Flutter memeriksa status autentikasi menggunakan `request.loggedIn`.
5. Jika autentikasi berhasil, pengguna dialihkan ke halaman menu (MyHomePage), dan pesan selamat datang ditampilkan.

## Sebutkan seluruh widget yang kamu pakai pada tugas ini dan jelaskan fungsinya masing-masing.
- `Provider`: Di dalam `MyApp`, `Provider` digunakan untuk membuat dan menyediakan instance dari CookieRequest, yang akan dapat diakses oleh widget-widget di bawahnya.
- `MaterialApp`: menentukan konfigurasi seperti tema, judul aplikasi, dan halaman utama.
- `ThemeData`: menentukan tema aplikasi, termasuk skema warna dan penggunaan Material Design 
- `Scaffold`: kerangka dasar untuk tata letak aplikasi Flutter.
- `AppBar`: menyediakan baris atas atau header pada antarmuka pengguna. 
- `LeftDrawer`: custom widget berisi drawer menu di sebelah kiri aplikasi.
- `FutureBuilder`: membuat antarmuka pengguna berdasarkan hasil dari operasi async. Digunakan untuk menampilkan data produk yang diambil melalui HTTP request.
- `CircularProgressIndicator`: indikator putar yang menunjukkan bahwa aplikasi sedang menunggu data. Muncul saat data sedang dimuat.
- `ListView.builder`: menampilkan daftar item yang diambil dari server.
- `Container`: membatasi dan mengatur tata letak elemen di dalamnya.
- `Column `: menata anak-anaknya dalam kolom vertikal
- `Text`: menampilkan teks di antarmuka pengguna.
- `TextField`: memungkinkan pengguna memasukkan teks.
- `SizedBox`: memberikan ruang kosong sesuai dengan ukuran yang ditentukan.
- `ElevatedButton`: tombol yang memberikan efek naik ketika ditekan.
- `AlertDialog`: menampilkan dialog dengan teks dan tindakan yang ditentukan.
- `Navigator`: mengelola navigasi antar halaman aplikasi.
- `ScaffoldMessenger`: memberikan akses ke Scaffold dari luar Scaffold saat ini.

## Jelaskan bagaimana cara kamu mengimplementasikan checklist di atas secara step-by-step! (bukan hanya sekadar mengikuti tutorial).
- Setup autentikasi pada Django untuk Flutter, kemudian deploy ulang proyek tugas Django.
- Selanjutnya, lakukan integrasi sistem autentikasi pada Flutter.
- Untuk memudahkan sistem autentikasi, pada terminal proyek flutter, jalankan
```
flutter pub add provider 
flutter pub add pbp_django_auth
```
- Kemudian, modifikasi _root widget_ yang ada di `main.dart` untuk menyediakan `CookieRequest` library ke semua child widgets dengan menggunakan `Provider`.
- Buat berkas `login.dart` pada folder `screens` dan salin kode berikut
```
import 'package:flutter/material.dart';
import 'package:pbp_django_auth/pbp_django_auth.dart';
import 'package:provider/provider.dart';
import 'package:mobile_inventory/screens/menu.dart';

void main() {
    runApp(const LoginApp());
}

class LoginApp extends StatelessWidget {
const LoginApp({super.key});

@override
Widget build(BuildContext context) {
    return MaterialApp(
        title: 'Login',
        theme: ThemeData(
            primarySwatch: Colors.blue,
    ),
    home: const LoginPage(),
    );
    }
}

class LoginPage extends StatefulWidget {
    const LoginPage({super.key});

    @override
    _LoginPageState createState() => _LoginPageState();
}

class _LoginPageState extends State<LoginPage> {
    final TextEditingController _usernameController = TextEditingController();
    final TextEditingController _passwordController = TextEditingController();

    @override
    Widget build(BuildContext context) {
        final request = context.watch<CookieRequest>();
        return Scaffold(
            appBar: AppBar(
                title: const Text('Login'),
            ),
            body: Container(
                padding: const EdgeInsets.all(16.0),
                child: Column(
                    mainAxisAlignment: MainAxisAlignment.center,
                    children: [
                        TextField(
                            controller: _usernameController,
                            decoration: const InputDecoration(
                                labelText: 'Username',
                            ),
                        ),
                        const SizedBox(height: 12.0),
                        TextField(
                            controller: _passwordController,
                            decoration: const InputDecoration(
                                labelText: 'Password',
                            ),
                            obscureText: true,
                        ),
                        const SizedBox(height: 24.0),
                        ElevatedButton(
                            onPressed: () async {
                                String username = _usernameController.text;
                                String password = _passwordController.text;

                                // Cek kredensial
                                // TODO: Ganti URL dan jangan lupa tambahkan trailing slash (/) di akhir URL!
                                // Untuk menyambungkan Android emulator dengan Django pada localhost,
                                // gunakan URL http://10.0.2.2/
                                final response = await request.login("https://marvel-martin-tugas.pbp.cs.ui.ac.id/auth/login/", {
                                'username': username,
                                'password': password,
                                });
                    
                                if (request.loggedIn) {
                                    String message = response['message'];
                                    String uname = response['username'];
                                    Navigator.pushReplacement(
                                        context,
                                        MaterialPageRoute(builder: (context) => MyHomePage()),
                                    );
                                    ScaffoldMessenger.of(context)
                                        ..hideCurrentSnackBar()
                                        ..showSnackBar(
                                            SnackBar(content: Text("$message Selamat datang, $uname.")));
                                    } else {
                                    showDialog(
                                        context: context,
                                        builder: (context) => AlertDialog(
                                            title: const Text('Login Gagal'),
                                            content:
                                                Text(response['message']),
                                            actions: [
                                                TextButton(
                                                    child: const Text('OK'),
                                                    onPressed: () {
                                                        Navigator.pop(context);
                                                    },
                                                ),
                                            ],
                                        ),
                                    );
                                }
                            },
                            child: const Text('Login'),
                        ),
                    ],
                ),
            ),
        );
    }
}
```
- Pada file `main.dart`, pada widget `MaterialApp()`, ubah `home:MyHomePage()` menjadi `home:LoginPage()`.
- Salin data `JSON` pada endpoint `JSON` projek django dan gunakan situs web [Quicktype](https://app.quicktype.io/) untuk membuat model kustom berdasarkan data tersebut.
- Menambahkan dependensi http dengan menjalankan `flutter pub add http` di terminal.
- Kemudian, pada file `android/app/src/main/AndroidManifest.xml`, tambahkan kode untuk memperbolehkan akses Internet pada aplikasi Flutter yang sedang dibuat.
- Membuat berkas `list_product.dart` pada folder `screens` sebagai page untuk menampilkan daftar item dan salin kode berikut
```
import 'package:flutter/material.dart';
import 'package:http/http.dart' as http;
import 'dart:convert';
import 'package:mobile_inventory/models/product.dart';

import 'package:mobile_inventory/widgets/left_drawer.dart';

class ProductPage extends StatefulWidget {
    const ProductPage({Key? key}) : super(key: key);

    @override
    _ProductPageState createState() => _ProductPageState();
}

class _ProductPageState extends State<ProductPage> {
Future<List<Product>> fetchProduct() async {
    // TODO: Ganti URL dan jangan lupa tambahkan trailing slash (/) di akhir URL!
    var url = Uri.parse(
        'https://marvel-martin-tugas.pbp.cs.ui.ac.id/json/');
    var response = await http.get(
        url,
        headers: {"Content-Type": "application/json"},
    );

    // melakukan decode response menjadi bentuk json
    var data = jsonDecode(utf8.decode(response.bodyBytes));

    // melakukan konversi data json menjadi object Product
    List<Product> list_product = [];
    for (var d in data) {
        if (d != null) {
            list_product.add(Product.fromJson(d));
        }
    }
    return list_product;
}

@override
Widget build(BuildContext context) {
    return Scaffold(
        appBar: AppBar(
        title: const Text('Item'),
        backgroundColor: Colors.indigo,
        foregroundColor: Colors.white,
        ),
        drawer: const LeftDrawer(),
        body: FutureBuilder(
            future: fetchProduct(),
            builder: (context, AsyncSnapshot snapshot) {
                if (snapshot.data == null) {
                    return const Center(child: CircularProgressIndicator());
                } else {
                    if (!snapshot.hasData) {
                    return const Column(
                        children: [
                        Text(
                            "Tidak ada data Item.",
                            style:
                                TextStyle(color: Color(0xff59A5D8), fontSize: 20),
                        ),
                        SizedBox(height: 8),
                        ],
                    );
                } else {
                    return ListView.builder(
                        itemCount: snapshot.data!.length,
                        itemBuilder: (_, index) => Container(
                                margin: const EdgeInsets.symmetric(
                                    horizontal: 16, vertical: 12),
                                padding: const EdgeInsets.all(20.0),
                                child: Column(
                                mainAxisAlignment: MainAxisAlignment.start,
                                crossAxisAlignment: CrossAxisAlignment.start,
                                children: [
                                    Text(
                                    "${snapshot.data![index].fields.name}",
                                    style: const TextStyle(
                                        fontSize: 18.0,
                                        fontWeight: FontWeight.bold,
                                    ),
                                    ),
                                    const SizedBox(height: 10),
                                    Text("${snapshot.data![index].fields.amount}"),
                                    const SizedBox(height: 10),
                                    Text(
                                        "${snapshot.data![index].fields.description}")
                                ],
                                ),
                            ));
                    }
                }
            }));
    }
}
```
- Tambahkan halaman `list_product.dart` ke `widgets/left_drawer.dart` dengan menambahkan kode berikut.
```
ListTile(
    leading: const Icon(Icons.shopping_basket),
    title: const Text('Daftar Produk'),
    onTap: () {
        // Route menu ke halaman produk
        Navigator.push(
        context,
        MaterialPageRoute(builder: (context) => const ProductPage()),
        );
    },
),
```
- Ubah fungsi tombol `Lihat Item` pada file `widgets/shop_card.dart` agar mengarahkan ke halaman `ProductPage` dengan menambahkan kode berikut di bagian akhir `onTap: () { } ` yang ada pada file `widgets/shop_card.dart`
```
else if (item.name == "Lihat Produk") {
        Navigator.push(context,
            MaterialPageRoute(builder: (context) => const ProductPage()));
      }
```
- Impor file yang dibutuhkan saat menambahkan `ProductPage` ke `left_drawer.dart` dan `shop_card.dart`.


