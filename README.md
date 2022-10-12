
# Mengambil Data dari DB untuk ke View

Cara menggunakan query builder laravel


## Penggunaan

Biasanya penggunaan format selisih tanggal ini dipake untuk :

Menampilkan data untuk ke view index

Controller berelasi dari "models", jadi pastiin modelnya udah bener. Ini sama aja kaya proses join antar tabel jadi harus diperhatiin "models"nya

## Penjelasan Code Pada Controller

```php
public function index()
{
    $kodeakun       = Auth::pengguna()->kode; //untuk mendapatkan kode akun
    if ($kodeakun == "admin") { // mengecek variable "kodeakun" jika "admin" maka..
        
        //membuat variable "akun"
        //mengecek "models" dari "Account" apakah ada row "status" yang berisikan "1"
        //jika ada maka akan "get()" yang artinya mengambil isi field tersebut
        $akun       = Account::where('status', '1')->get();


        //membuat variable "area"
        //mengecek "models" dari "Area"
        //menyortir data tersebut berdasarkan "nama" secara "Assending"
        //dan kemudian akan mengambil data tersebut
        $area       = Area::orderBy('nama', 'ASC')->get();


        //membuat variable "produk"
        //mengecek "models" dari "Produk"
        //menyortir data tersebut berdasarkan "nama" secara "Assending"
        //dan kemudian akan mengambil data tersebut
        $produk     = Produk::orderBy('nama', 'ASC')->get();
    } else { //kondisi else

        //membuat variable "akun"
        //mengecek "models" dari "Account" apakah ada row "kode" yang berisikan kode sesuai dari variable "kodeakun"
        //mengecek "models" dari "Account" apakah ada row "status" yang berisikan "1"
        //jika ada maka akan "get()" mengambil seluruh data tersebut
        $akun       = Account::where('kode', $kodeakun)->where('status', '1')->get();


        //membuat variable "area"
        //mengecek "models" dari "Area"
        //join table "Area" dengan "wilayah"
        //mengecek apakah ada row "kode" yg berisikan sesuai dengan "kodeakun"
        //dan mengambil data tersebut
        $area       = Area::with('wilayah')->where('kode', $kodeakun)->get();


        //membuat variable "produk"
        //mengecek "models" dari "Produk"
        //mengambil data tersebut berdasarkan "nama" yg berisikan sesuai dengan "kodeakun"
        //menyortir data tersebut berdasarkan "nama" secara "Assending"
        //dan kemudian akan mengambil data tersebut
        $produk     = Produk::where('kode', $kodeakun)->orderBy('nama', 'ASC')->get();
    }


    //mengembalikan data dengan menampilkan view
    //view mengambil dari folder "reporting" - "kunjungan" - dan file "index" ditampilkan
    //compact brarti variable dan juga data yang akan ditampilkan sama
    //dalam compact brarti kita menampilkan variable = "akun", "area", "produk"
    return view('reporting.kunjungan.index', compact('akun', 'area', 'produk'));
}
```

## Code Bersih Pada Controller

```php
public function index()
{
    $kodeakun       = Auth::pengguna()->kode;
    if ($kodeakun == "admin") {
        $akun       = Account::where('status', '1')->get();ara "Assending"
        //dan kemudian akan mengambil data tersebut
        $area       = Area::orderBy('nama', 'ASC')->get();
        $produk     = Produk::orderBy('nama', 'ASC')->get();
    } else {
        $akun       = Account::where('kode', $kodeakun)->where('status', '1')->get();
        $area       = Area::with('wilayah')->where('kode', $kodeakun)->get();
        $produk     = Produk::where('kode', $kodeakun)->orderBy('nama', 'ASC')->get();
    }
    return view('reporting.kunjungan.index', compact('akun', 'area', 'produk'));
}
```
