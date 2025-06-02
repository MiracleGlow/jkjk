Berikut adalah contoh program CRUD (Create, Read, Update, Delete) sederhana menggunakan PHP dan MySQLi yang mudah dipahami, cocok untuk pemula yang ingin belajar dasar-dasar pengelolaan data dengan PHP.

---

## 🗂️ Struktur Folder

Buat folder bernama `crud_php` di dalam direktori `htdocs` (jika menggunakan XAMPP). Di dalamnya, buat file-file berikut:([bikincoding.com][1])

* `koneksi.php`
* `index.php`
* `tambah.php`
* `edit.php`
* `hapus.php`([My Notes Code][2], [Petani Kode][3], [bikincoding.com][1])

---

## 🛠️ 1. Membuat Database dan Tabel

Gunakan phpMyAdmin atau MySQL untuk membuat database dan tabel:([bikincoding.com][1])

```sql
CREATE DATABASE db_siswa;

USE db_siswa;

CREATE TABLE siswa (
    id INT AUTO_INCREMENT PRIMARY KEY,
    nama VARCHAR(100) NOT NULL,
    email VARCHAR(100) NOT NULL,
    telepon VARCHAR(15) NOT NULL,
    alamat TEXT NOT NULL
);
```



---

## 🔌 2. File `koneksi.php`

File ini digunakan untuk menghubungkan PHP dengan database MySQL.([My Notes Code][2])

```php
<?php
$host = "localhost";
$user = "root";
$pass = "";
$db   = "db_siswa";

$koneksi = mysqli_connect($host, $user, $pass, $db);

if (!$koneksi) {
    die("Koneksi gagal: " . mysqli_connect_error());
}
?>
```



---

## 📄 3. File `index.php` (Menampilkan Data)

File ini menampilkan semua data siswa dari database.([My Notes Code][2])

```php
<?php
include 'koneksi.php';
?>

<!DOCTYPE html>
<html>
<head>
    <title>Data Siswa</title>
</head>
<body>
    <h2>Data Siswa</h2>
    <a href="tambah.php">Tambah Data</a><br><br>
    <table border="1" cellpadding="8">
        <tr>
            <th>Nama</th>
            <th>Email</th>
            <th>Telepon</th>
            <th>Alamat</th>
            <th>Aksi</th>
        </tr>
        <?php
        $sql = mysqli_query($koneksi, "SELECT * FROM siswa");
        while($data = mysqli_fetch_array($sql)){
            echo "<tr>";
            echo "<td>".$data['nama']."</td>";
            echo "<td>".$data['email']."</td>";
            echo "<td>".$data['telepon']."</td>";
            echo "<td>".$data['alamat']."</td>";
            echo "<td>
                    <a href='edit.php?id=".$data['id']."'>Edit</a> |
                    <a href='hapus.php?id=".$data['id']."' onclick=\"return confirm('Yakin ingin menghapus data ini?')\">Hapus</a>
                  </td>";
            echo "</tr>";
        }
        ?>
    </table>
</body>
</html>
```



---

## ➕ 4. File `tambah.php` (Menambah Data)

File ini digunakan untuk menambahkan data siswa baru.

```php
<?php
include 'koneksi.php';

if(isset($_POST['submit'])){
    $nama    = $_POST['nama'];
    $email   = $_POST['email'];
    $telepon = $_POST['telepon'];
    $alamat  = $_POST['alamat'];

    $sql = "INSERT INTO siswa (nama, email, telepon, alamat) VALUES ('$nama', '$email', '$telepon', '$alamat')";
    if(mysqli_query($koneksi, $sql)){
        header("Location: index.php");
    } else {
        echo "Error: " . mysqli_error($koneksi);
    }
}
?>

<!DOCTYPE html>
<html>
<head>
    <title>Tambah Data Siswa</title>
</head>
<body>
    <h2>Tambah Data Siswa</h2>
    <form method="post" action="">
        <label>Nama:</label><br>
        <input type="text" name="nama" required><br>
        <label>Email:</label><br>
        <input type="email" name="email" required><br>
        <label>Telepon:</label><br>
        <input type="text" name="telepon" required><br>
        <label>Alamat:</label><br>
        <textarea name="alamat" required></textarea><br><br>
        <input type="submit" name="submit" value="Simpan">
    </form>
</body>
</html>
```



---

## ✏️ 5. File `edit.php` (Mengedit Data)

File ini digunakan untuk mengedit data siswa yang sudah ada.

```php
<?php
include 'koneksi.php';

$id = $_GET['id'];
$sql = mysqli_query($koneksi, "SELECT * FROM siswa WHERE id='$id'");
$data = mysqli_fetch_array($sql);

if(isset($_POST['submit'])){
    $nama    = $_POST['nama'];
    $email   = $_POST['email'];
    $telepon = $_POST['telepon'];
    $alamat  = $_POST['alamat'];

    $update = mysqli_query($koneksi, "UPDATE siswa SET nama='$nama', email='$email', telepon='$telepon', alamat='$alamat' WHERE id='$id'");
    if($update){
        header("Location: index.php");
    } else {
        echo "Error: " . mysqli_error($koneksi);
    }
}
?>

<!DOCTYPE html>
<html>
<head>
    <title>Edit Data Siswa</title>
</head>
<body>
    <h2>Edit Data Siswa</h2>
    <form method="post" action="">
        <label>Nama:</label><br>
        <input type="text" name="nama" value="<?php echo $data['nama']; ?>" required><br>
        <label>Email:</label><br>
        <input type="email" name="email" value="<?php echo $data['email']; ?>" required><br>
        <label>Telepon:</label><br>
        <input type="text" name="telepon" value="<?php echo $data['telepon']; ?>" required><br>
        <label>Alamat:</label><br>
        <textarea name="alamat" required><?php echo $data['alamat']; ?></textarea><br><br>
        <input type="submit" name="submit" value="Update">
    </form>
</body>
</html>
```



---

## 🗑️ 6. File `hapus.php` (Menghapus Data)

File ini digunakan untuk menghapus data siswa berdasarkan ID.([My Notes Code][2])

```php
<?php
include 'koneksi.php';

$id = $_GET['id'];
$delete = mysqli_query($koneksi, "DELETE FROM siswa WHERE id='$id'");

if($delete){
    header("Location: index.php");
} else {
    echo "Error: " . mysqli_error($koneksi);
}
?>
```



---

## 📚 Referensi Tambahan

Untuk memperdalam pemahaman Anda tentang PHP dan MySQL, berikut beberapa sumber yang dapat Anda pelajari:

* **Tutorial CRUD PHP dan MySQL oleh Petani Kode**: Tutorial ini membahas langkah-langkah membuat aplikasi CRUD dengan PHP dan MySQL secara detail. ([My Notes Code][2])

* **Tutorial CRUD PHP dengan MySQL oleh Codepolitan**: Menyediakan panduan lengkap membuat aplikasi CRUD dengan PHP dan MySQL, cocok untuk pemula.&#x20;

* **Tutorial Membuat Aplikasi CRUD PHP oleh Bikin Coding**: Menjelaskan cara membuat aplikasi CRUD dengan PHP dan MySQL, termasuk validasi data dan struktur folder yang rapi. ([bikincoding.com][1])

---

Untuk panduan visual, Anda juga dapat menonton video tutorial berikut:

[Cara Mudah Membuat CRUD dengan PHP & MySQL](https://www.youtube.com/watch?v=fDntkybfvVA&utm_source=chatgpt.com)

---

Dengan mengikuti langkah-langkah di atas, Anda dapat membangun aplikasi CRUD sederhana menggunakan PHP dan MySQL. Jika Anda memerlukan bantuan lebih lanjut atau ingin mengembangkan fitur tambahan seperti pencarian data atau pagination, jangan ragu untuk bertanya!

[1]: https://bikincoding.com/tutorial-membuat-aplikasi-crud-php/?utm_source=chatgpt.com "Tutorial Membuat Aplikasi CRUD PHP Paling Sederhana"
[2]: https://www.mynotescode.com/crud-php-mysql/?utm_source=chatgpt.com "Cara Mudah Membuat CRUD dengan PHP dan MySQL"
[3]: https://www.petanikode.com/kode-crud-dalam-satu-file-php/?utm_source=chatgpt.com "Kode CRUD dalam Satu File PHP"
