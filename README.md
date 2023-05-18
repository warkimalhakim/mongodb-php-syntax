# mongodb-php-syntax
Catatan Praktis (Class) MongoDB dengan Syntax PHP

MONGODB PHP SYNTAX
==================

========================================
DATABASE & COLLECTIONS
========================================
# MEMBUAT KONEKSI / CONNECTION
<code>$variabel = new MongoDB\Client</code>

# DAFTAR SEMUA DATABASE
<code>$variabel->listDatabases()</code>

# MENGHAPUS DATABASE
<code>$variabel->dropDatabase('namaDatabase')</code>

# MEMBUAT /  MEMILIH DATABASE
<code>$db = $variabel->namaDatabase</code>

# MEMBUAT COLLECTION
<code>$db->createCollection('namaCollection')</code>

# DAFTAR SEMUA COLLECTION
<code>$db->listCollections()<code>

# MENGHAPUS COLLECTION
<code>$db->dropCollection('namaCollection')</code>

========================================
INSERT DOCUMENTS
========================================

# INSERT 1 DATA
<code>insertOne([])</code>

# INSERT BANYAK DATA
<code>insertMany([[],[]])</code>

# MEMBACA BERAPA BANYAK DATA YANG DI INSERT
<code>getInsertedCount()</code>

# MEMBACA 1 ID YANG BERHASIL DI INSERT
<code>getInsertedId()</code>

# MEMBACA BANYAK ID YANG BERHASIL DI INSERT
<code>getInsertedIds()</code>

========================================
QUERY DOCUMENTS
========================================

# MENAMPILKAN SEMUA FIELDS / RECORD PADA COLLECTION
<code>find()</code> *Hasil bisa di looping

# MENCARI 1 DOKUMEN / DATA (BERDASARKAN ID)
<code>findOne(['_id' => integer|number|string])</code>

# MENCARI 1 ATAU LEBIH DOKUMEN / DATA (BERDASARKAN SELAIN ID)
<code>find(
['skill' => integer|number|string]
)</code> *Hasil dapat di looping

========================================
PROJECTION
========================================

# PROJECTION / MENCARI DAN MENAMPILKAN HASIL FIELD/RECORD DI INGINKAN SAJA
1 = TAMPIL
0 = SEMBUNYIKAN
<code>
$cari = $db->find(
	['skill' => 'mongoDB'],
	['projection' => ['_id' => 0, 'name' => 1, ...field lainnya]]
	)
</code>
========================================
UPDATE DOCUMENT
========================================

# UPDATE 1 DOKUMEN (UPDATE TANPA PENCARIAN ID)
<code>
	updateOne(
	['umur' => 20],
	['$set' => ['umur'=> 21]]
	)
</code>
*Cari data yang pertama ditemukan dengan umur 20, update menjadi 21
*Parameter pertama: pencarian data
*Paremeter kedua: SET data

# UPDATE BANYAK DOKUMEN (UPDATE TANPA PENCARIAN ID)
<code>
updateMany(
	['umur' => 20],
	['$set' => ['umur'=> 21]]
	)
</code>
	
# UPDATE DOKUMEN DENGAN MENAMBAH KOLOM/FIELD/KEY
<code>
updateMany(
	['umur' => 20],
	['$set' => ['fieldYangTidakAda'=> 'sekarangAdaFieddanIsi']]
	)
</code>
*Bisa dengan updateOne / updateMany

# MENGEMBALIKAN BERAPA DATA YANG COCOK
<code>getMatchedCount()</code>

# MENGEMBALIKAN BERAPA DATA YANG BERHASIL DI UPDATE
<code>getModifiedCount()</code>


# REPLACE DOKUMEN
<code>
replaceOne(
['_id' => '1'],
['_id' => '1', 'keynya' => 'valuenya']
)
</code>

========================================
LIMIT, SKIP & SORT
========================================
1. Gunakan find untuk mencari dokumen, kosongkan jika cari semua
2. limit, untuk membatasi data yang tampil
3. skip, untuk melangkahi berapa banyak dokumen
4. sort, angka integer: 1 = Ascending, -1 = Descending
<code>
find(
[],
[
'limit' => 2,
'skip' => 2,
'sort'	=> ['usia' => -1]
]
)
</code>
*Parameter pertama berupa array pencarian atau kosongkan untuk semua dokumen


========================================
DELETE DOCUMENTS
========================================
# MENGHAPUS 1 DOKUMEN
deleteOne(['_id' => '1'])

#MENGHAPUS LEBIH DARI 1 DOKUMEN
<code>deleteMany(['usia' => 20])</code>
*Menghapus semua dokumen yang mempunyai usia 20

# MENGHAPUS SEMUA DOKUMEN DALAM COLLECTION
<code>deleteMany([])</code>

# MENGEMBALIKAAN JUMLAH YANG TERHAPUS
<code>getDeletedCount()</code>



========================================
UPDATE SEMUA KEYS
========================================
<code>
$db->nama_collection->updateMany([],
['$rename' => ['keyOld' => 'keyNew']]
)
</code>

* Gunakan variabel $rename untuk mengganti key
* Statement pertama (array) biarkan kosong untuk mencari semua


========================================
JOIN 2 ATAU LEBIH COLLECTIONS
========================================
<code>
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
</code>

* Join antara tabel 'classroom' dengan 'student'
* From: sengan tabel apa?
* localField: nama field yang ada di tabel students
* foreignField: field index yang ada di kolom classroom
* as: nama penampung hasil join antar tabel
--
* $project: variabel untuk menampilkan fied yang mana saja/pilihan

<code>
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
</code>

========================================
RENAME COLLECTION
========================================
<code>$db->NAMA_COLLECTION->rename('NAM_COLLECTION_BARU')</code>
