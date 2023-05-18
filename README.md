# mongodb-php-syntax
Catatan Praktis (Class) MongoDB dengan Syntax PHP

MONGODB PHP SYNTAX
==================

========================================
DATABASE & COLLECTIONS
========================================
# MEMBUAT KONEKSI / CONNECTION
```
$variabel = new MongoDB\Client
```

# DAFTAR SEMUA DATABASE
```
$variabel->listDatabases()
```

# MENGHAPUS DATABASE
```
$variabel->dropDatabase('namaDatabase')
```

# MEMBUAT /  MEMILIH DATABASE
```
$db = $variabel->namaDatabase
```

# MEMBUAT COLLECTION
```$db->createCollection('namaCollection')```

# DAFTAR SEMUA COLLECTION
```$db->listCollections()```

# MENGHAPUS COLLECTION
```$db->dropCollection('namaCollection')```

========================================
INSERT DOCUMENTS
========================================

# INSERT 1 DATA
```insertOne([])```

# INSERT BANYAK DATA
```insertMany([[],[]])```

# MEMBACA BERAPA BANYAK DATA YANG DI INSERT
```
getInsertedCount()
```

# MEMBACA 1 ID YANG BERHASIL DI INSERT
```
getInsertedId()
```

# MEMBACA BANYAK ID YANG BERHASIL DI INSERT
```
getInsertedIds()
```

========================================
QUERY DOCUMENTS
========================================

# MENAMPILKAN SEMUA FIELDS / RECORD PADA COLLECTION
```
find() *Hasil bisa di looping
```

# MENCARI 1 DOKUMEN / DATA (BERDASARKAN ID)
```
findOne(['_id' => integer|number|string])
```

# MENCARI 1 ATAU LEBIH DOKUMEN / DATA (BERDASARKAN SELAIN ID)
```
find(
['skill' => integer|number|string]
) *Hasil dapat di looping
```

========================================
PROJECTION
========================================

# PROJECTION / MENCARI DAN MENAMPILKAN HASIL FIELD/RECORD DI INGINKAN SAJA
1 = TAMPIL
0 = SEMBUNYIKAN
```
$cari = $db->find(
	['skill' => 'mongoDB'],
	['projection' => ['_id' => 0, 'name' => 1, ...field lainnya]]
	)
```
========================================
UPDATE DOCUMENT
========================================

# UPDATE 1 DOKUMEN (UPDATE TANPA PENCARIAN ID)
```
	updateOne(
	['umur' => 20],
	['$set' => ['umur'=> 21]]
	)
```
*Cari data yang pertama ditemukan dengan umur 20, update menjadi 21
*Parameter pertama: pencarian data
*Paremeter kedua: SET data

# UPDATE BANYAK DOKUMEN (UPDATE TANPA PENCARIAN ID)
```
updateMany(
	['umur' => 20],
	['$set' => ['umur'=> 21]]
	)
```
	
# UPDATE DOKUMEN DENGAN MENAMBAH KOLOM/FIELD/KEY
```
updateMany(
	['umur' => 20],
	['$set' => ['fieldYangTidakAda'=> 'sekarangAdaFieddanIsi']]
	)
*Bisa dengan updateOne / updateMany
```

# MENGEMBALIKAN BERAPA DATA YANG COCOK
```
getMatchedCount()
```

# MENGEMBALIKAN BERAPA DATA YANG BERHASIL DI UPDATE
```
getModifiedCount()
```


# REPLACE DOKUMEN
```
replaceOne(
['_id' => '1'],
['_id' => '1', 'keynya' => 'valuenya']
)
```

========================================
LIMIT, SKIP & SORT
========================================
1. Gunakan find untuk mencari dokumen, kosongkan jika cari semua
2. limit, untuk membatasi data yang tampil
3. skip, untuk melangkahi berapa banyak dokumen
4. sort, angka integer: 1 = Ascending, -1 = Descending
```
find(
[],
[
'limit' => 2,
'skip' => 2,
'sort'	=> ['usia' => -1]
]
)
```
*Parameter pertama berupa array pencarian atau kosongkan untuk semua dokumen


========================================
DELETE DOCUMENTS
========================================
# MENGHAPUS 1 DOKUMEN
```
deleteOne(['_id' => '1'])
```

#MENGHAPUS LEBIH DARI 1 DOKUMEN
```
deleteMany(['usia' => 20])
```
*Menghapus semua dokumen yang mempunyai usia 20

# MENGHAPUS SEMUA DOKUMEN DALAM COLLECTION
```
deleteMany([])
```

# MENGEMBALIKAAN JUMLAH YANG TERHAPUS
```
getDeletedCount()
```

========================================
UPDATE SEMUA KEYS
========================================
```
$db->nama_collection->updateMany([],
['$rename' => ['keyOld' => 'keyNew']]
)
```

* Gunakan variabel $rename untuk mengganti key
* Statement pertama (array) biarkan kosong untuk mencari semua


========================================
JOIN 2 ATAU LEBIH COLLECTIONS
========================================
```
$data = $conn->students->aggregate([
    [
        '$lookup' => [
            'localField' => 'id',
            'from' => 'classroom',
            'foreignField' => 'student_id',
            'as' => 'class_join'
        ]
    ],
    ['$project' => [
        'class_name' => 1,
    ]]
]);
```

* Join antara tabel 'classroom' dengan 'student'
* From: sengan tabel apa?
* localField: nama field yang ada di tabel students
* foreignField: field index yang ada di kolom classroom
* as: nama penampung hasil join antar tabel
--
* $project: variabel untuk menampilkan fied yang mana saja/pilihan

```
$data = $conn->posts->aggregate([
    [
        '$lookup' => [
            'localField' => 'author',
            'from' => 'users',
            'foreignField' => 'id',
            'as' => 'posts_users_joined'
        ]

    ],
[
        '$lookup' => [
            'localField' => 'updated_by',
            'from' => 'users',
            'foreignField' => 'id',
            'as' => 'posts_user_updated_joined'
        ]

    ],
	[
        '$lookup' => [
            'localField' => 'id',
            'from' => 'comments',
            'foreignField' => 'postid',
            'as' => 'posts_comments_joined'
        ]

    ],
    ['$project' => []]
]);
```

========================================
RENAME COLLECTION
========================================
```
$db->NAMA_COLLECTION->rename('NAM_COLLECTION_BARU')
```
