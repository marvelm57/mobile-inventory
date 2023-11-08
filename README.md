# Tugas 1
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