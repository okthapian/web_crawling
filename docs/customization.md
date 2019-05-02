# TF - IDF 

## TF

TF atau term frequency adalah weighting scheme yang digunakan untuk menentukan relevansi dokumen dengan sebuah query (term). TF menentukan bobot relevansi sebuah dokumen dan term berdasarkan frekuensi kemunculan term pada dokumen terkait. Untuk menghitung TF terdapat beberapa jenis fungsi yang dapat digunakan

## DF

Jika TF adalah frekuensi kemunculan suatu term pada sebuah dokumen, DF merupakan jumlah dokumen dimana terdapat term yang bersangkutan. Konsep DF sendiri dilatarbelakangin oleh masalah pada TF, dimana semua term dianggap sama penting, sehingga term yang memiliki sedikit atau tidak memiliki discrimination power dapat mempengaruhi akurasi dalam menentukan relevansi antara term dan dokumen. Ide dari DF adalah dengan mengurangi bobot TF suatu term dengan membaginya dengan frekuensi term terhadap koleksi dokumen (DF). Jadi sebuah term yang memiliki bobot TF yang besar namun dengan bobot DF yang besar pula tidak akan memiliki pengaruh yang besar dalam menentukan sebuah relevansi .

## IDF

IDF adalah inverse dari DF, IDF akan melakukan proses scaling pada TF. Term yang memiliki DF yang rendah akan memiliki IDF yang tinggi. Dengan kata lain, sebuah term yang jarang ditemui pada koleksi dokumen atau bisa dikatakan sebagai term khusus akan memiliki nilai IDF yang tinggi. Untuk menghitung IDF pada sebuah term pada sebuah koleksi dokumen dapat menggunakan fungsi dibawah ini,


## TF-IDF
Selain menggunakan Bag of Words, kita juga bisa menggunakan metode TF-IDF. Hal ini karena Bag of Word memiliki kelemahan tersendiri.
TF-IDF sendiri merupakan kepanjangan dari Term Frequence (frekuensi Kata) dan Invers Document Frequence (invers frekuensi Dokumen). Rumus TF-IDF sendiri terbilang mudah karena hanya TFxIDF.
Kita telah mencari TF sebelumnya (yaitu Bag of Words), karena konsep keduanya yang memang sama. Sekarang kita tinggal mencari nilai IDF.
Untuk mendapatkan IDF, pertama kita perlu mencari DF (frekuensi Dokumen). 
Contoh:

- Artikel 1:anak itu pergi kesekolah
- Artikel2:anak dan ibu pergi 

| Kata      | Document yangmemiliki kata tersebut |
| --------- | ----------------------------------- |
| anak      | 2                                   |
| ibu       | 1                                   |
| dan       | 1                                   |
| pergi     | 2                                   |
| kesekolah | 1                                   |
| ibu       | 1                                   |

```
df = list()
total_doc = bow.shape[0]
for kolom in range(len(bow[0])):
    total = 0
    for baris in range(len(bow)):
        if (bow[baris, kolom] > 0):
            total +=1
    df.append(total)
df = np.array(df)

idf = list()
for i in df:
    tmp = 1 + log10(total_doc/(1+i))
    idf.append(tmp)
idf = np.array(idf)

tfidf = bow * idf
```

Tahap selanjutnya adalah  [Preprocessing ][3].

[3]: customization.md