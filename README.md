 📖Tugas Pertemuan 12 

# Database.php
~~~php
<?php
class Database {
    protected $host;
    protected $user;
    protected $password;
    protected $db_name;
    protected $conn;

    public function __construct() {
        $this->getConfig();
        $this->conn = new mysqli(
            $this->host,
            $this->user,
            $this->password,
            $this->db_name
        );

        if ($this->conn->connect_error) {
            die("Connection failed: " . $this->conn->connect_error);
        }
    }

    private function getConfig() {
        include_once("config.php");
        $this->host     = $config['host'];
        $this->user     = $config['username'];
        $this->password = $config['password'];
        $this->db_name  = $config['db_name'];
    }

    public function query($sql) {
        return $this->conn->query($sql);
    }

    public function get($table, $where = null) {
        if ($where) {
            $where = " WHERE " . $where;
        }

        $sql = "SELECT * FROM " . $table . $where;
        $sql = $this->conn->query($sql);
        $sql = $sql->fetch_assoc();

        return $sql;
    }

    public function insert($table, $data) {
        if (is_array($data)) {
            foreach ($data as $key => $val) {
                $column[] = $key;
                $value[]  = "'{$val}'";
            }

            $columns = implode(",", $column);
            $values  = implode(",", $value);
        }

        $sql = "INSERT INTO " . $table . " (" . $columns . ") VALUES (" . $values . ")";
        $sql = $this->conn->query($sql);

        if ($sql == true) {
            return $sql;
        } else {
            return false;
        }
    }

    public function update($table, $data, $where) {
        $update_value = "";

        if (is_array($data)) {
            foreach ($data as $key => $val) {
                $update_value[] = "$key='{$val}'"; // ← perbaikan: tambah ;
            }

            $update_value = implode(",", $update_value);
        }

        $sql = "UPDATE " . $table . " SET " . $update_value . " WHERE " . $where;
        $sql = $this->conn->query($sql);

        if ($sql == true) {
            return true;
        } else {
            return false;
        }
    }

    public function delete($table, $filter) {
        $sql = "DELETE FROM " . $table . " " . $filter;
        $sql = $this->conn->query($sql);

        if ($sql == true) {
            return true;
        } else {
            return false;
        }
    }
}
?>
~~~

# from_input.php

~~~php
<?php
/**
 * Program memanfaatkan Program 10.2 untuk membuat form inputan sederhana.
 **/
include "form.php";

echo "<html><head><title>Mahasiswa</title></head><body>";

$form = new Form("", "Input Form");
$form->addField("txtnim", "Nim");
$form->addField("txtnama", "Nama");
$form->addField("txtalamat", "Alamat");

echo "<h3>Silahkan isi form berikut ini :</h3>";
$form->displayForm();

echo "</body></html>";
?>
~~~

# from.php

~~~php
<?php
/**
 * Nama Class: Form
 * Deskripsi: Class untuk membuat form inputan text sederhana
 **/
class Form
{
    private $fields = array();
    private $action;
    private $submit = "Submit Form";
    private $jumField = 0;

    public function __construct($action, $submit)
    {
        $this->action = $action;
        $this->submit = $submit;
    }

    public function displayForm()
    {
        echo "<form action='" . $this->action . "' method='POST'>";
        echo '<table width="100%" border="0">';

        for ($j = 0; $j < count($this->fields); $j++) {
            echo "<tr><td align='right'>" . $this->fields[$j]['label'] . "</td>";
            echo "<td><input type='text' name='" . $this->fields[$j]['name'] . "'></td></tr>";
        }

        echo "<tr><td colspan='2'>";
        echo "<input type='submit' value='" . $this->submit . "'></td></tr>";
        echo "</table>";
    }

    public function addField($name, $label)
    {
        $this->fields[$this->jumField]['name']  = $name;
        $this->fields[$this->jumField]['label'] = $label;
        $this->jumField++;
    }
}
?>
~~~

# mobil.php

~~~php
<?php
/**
 * Program sederhana pendefinisian class dan pemanggilan class.
 **/
class Mobil
{
    private $warna;
    private $merk;
    private $harga;

    public function __construct()
    {
        $this->warna = "Biru";
        $this->merk  = "BMW";
        $this->harga = "10000000";
    }

    public function gantiWarna($warnaBaru)
    {
        $this->warna = $warnaBaru;
    }

    public function tampilWarna()
    {
        echo "Warna mobilnya : " . $this->warna;
    }
}

// membuat objek mobil
$a = new Mobil();
$b = new Mobil();

// memanggil objek
echo "<b>Mobil pertama</b><br>";
$a->tampilWarna();

echo "<br>Mobil pertama ganti warna<br>";
$a->gantiWarna("Merah");
$a->tampilWarna();

// memanggil objek
echo "<br><b>Mobil kedua</b><br>";
$b->gantiWarna("Hijau");
$b->tampilWarna();
?>
~~~

# 📑 Hasil
<img width="2240" height="1328" alt="Cuplikan layar 2025-12-01 111109" src="https://github.com/user-attachments/assets/7982d7f2-c376-4bdb-a411-d342468b1ceb" />
<img width="2240" height="1328" alt="Cuplikan layar 2025-12-01 111128" src="https://github.com/user-attachments/assets/c3f66f07-444c-4c2d-b918-8416df9ea907" />
