# Vector Space Model (VSM).

Pada VSM sebuah dokumen akan direpresentasikan dalam sebuah vektor, dimana setiap nilai pada vektor tersebut mewakili bobot term yang bersangkutan.
adalah model aljabar yang merepresentasikan kumpulan dokumen sebagai vetctor. VSM dapat diaplikasikan dalam klasifikasi dokumen, clustering dokumen, dan scoring dokumen terhadap sebuah query. Dalam VSM setiap dokumen direpresentasikan sebagai sebuah vector, dimana nilai dari setiap nilai dari vector tersebut mewakili weight sebuah term. Dalam menghitung term-weight dapa digunakan beberapa teknik seperti frequency, binary-document vector, atau tf-idf.

Dengan menggunakan Metode Bag of Words
Bag of Words merupakan salah satu metode untuk membuat sebuah Vector Space Model (VSM) dengan cara menghitung setiap kata pada setiap dokumen. Contohnya seperti ini
Contoh:

- Artikel 1:anak itu pergi kesekolah

- Artikel2:anak dan ibu pergi 

Kita akan menghitung setiap kata tersebut. Maka didapat VSM sebagai berikut:

|          | Anak | itu  | pergi | kesekolah | dan  | ibu  |
| -------- | ---- | ---- | ----- | --------- | ---- | ---- |
| artikel1 | 1    | 1    | 1     | 1         | 0    | 0    |
| artikel2 | 1    | 0    | 1     | 0         | 1    | 1    |

Untuk kodenya:

```
def countWord(txt, ngram=1):
    d = dict()
    token = tokenisasi(txt, ngram)
    for i  in token:
        if d.get(i) == None:
            d[i] = txt.count(i)
    return d
```

Fungsi diatas digunakan untuk menghitung setiap kata pada satu string

```
def add_row_VSM(d):
    VSM.append([])
    for i in VSM[0]:
        if d.get(i) == None:
            VSM[-1].append(0)
        else :
            VSM[-1].append(d.pop(i)); 
    for i in d:
        VSM[0].append(i)
        for j in range(1, len(VSM)-1):
            #VSM[j].insert(-2,0)
            VSM[j].append(0)
        VSM[-1].append(d.get(i))
```

Fungsi diatas adalah untuk membangun VSM ,jadi kita hanya memasukkan datanya saja  , Method counWord digunakan untuk menghitung banyaknya kata pada dokumen. Sementara method add_row_VSM digunakan untuk membuat sebuah matrix VSM tersebut.

Untuk pemakaian fungsi diatas adalah seperti dibawah ini.

```
cursor = conn.execute("SELECT * from jurnal2")
cursor = cursor.fetchall()
cursor = cursor[:60]
pertama = True
corpus = list()
label = list()
c=1
n = int(input("ngram : "))
#n=1
for row in cursor:
    print ('Proses : %.2f' %((c/len(cursor))*100) + '%'); c+=1
    label.append(row[-1])
    txt = row[-2]
    cleaned = preprosesing(txt)
    cleaned = cleaned[:-1]
    corpus.append(cleaned)

    d = countWord(cleaned, n)
    if pertama:
        pertama = False
        VSM = list((list(), list()))
        for key in d:
            VSM[0].append(key)
            VSM[1].append(d[key])
    else:
        add_row_VSM(d)

```

Lalu untuk menampilkan hasilnya (agar terlihat rapi) maka kita export ke csv. Serta kita pisahkan antara kata-kata dengan nilainya:

```
 
write_csv("bow_manual_%d.csv"%n, VSM)
feature_name = VSM[0]
bow = np.array(VSM[1:])
	 

```

Setelah langkah diatas selanjutnya kita akan melakukan  [TF-IDF][3].

[3]: customization.md