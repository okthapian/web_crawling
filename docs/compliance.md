# Preprocessing 

Tahap preprocessing merupakan tahap mengolah data agar data lebih mudah diproses. Untuk teman-teman tidak diharuskan mengunakaan python saja, namun terserah teman-teman
Tpi pada penejalasan ini saya menggunakan python





### Seleksi Fitur

Seleksi fitur merupakan salah satu cara untuk mengurangi dimensi fitur yang sangat banyak. Seperti pada kasus kita, Text Mining, jumlah fitur yang didapatkan bisa mencapai lebih dari 2000 kata yang berbeda. Namun, tidak semua kata tersebut benar-benar berpengaruh pada hasil akhir nantinya.
Selain itu, kita tahu bahwa semakin banyak data yang diproses, maka lebih banyak biaya dan waktu yang digunakan untuk memprosesnya. Oleh karena itu, kita perlu melakukan pengurangan fitur tanpa mengurangi kualitas hasil akhir, misalnya dengan Seleksi Fitur.
Pada dasarnya, seleksi fitur memiliki 3 tipe umum:

- Wrap
- Filter
- Embed

```
def seleksiFiturPearson(data, threshold):
    global meanFitur
    meanFitur = meanF(data)
    u=0
   while u < len(data[0]):
       dataBaru=data[:, :u+1]
        meanBaru=meanFitur[:u+1]
        v = u
        while v < len(data[0]):
            if u != v:
                value = pearsonCalculate(data, u,v)
               if value < threshold:
                   dataBaru = np.hstack((dataBaru, data[:, v].reshape(data.shape[0],1)))
                   meanBaru = np.hstack((meanBaru, meanFitur[v]))
            v+=1
        data = dataBaru
        meanFitur=meanBaru
        if u%50 == 0 : print(u, data.shape)
        u+=1
    return data

```

Selain itu, Seleksi Fitur juga memiliki banyak sekali metode-metode, seperti Information Gain, Chi Square, Pearson, dll.





### pearson Correlation

Pendekatan Pearson merupakan pendekatan paling sederhana. Pada pendekatan ini, setiap fitur akan dihitung korelasinya. Semakin tinggi nilainya, maka fitur tersebut semakin kuat korelasinya. Lalu fitur yang memiliki korelasi tinggi akan dibuang salah satunya.
Pendekatan ini digunakan untuk data tipe numerik.
Kodenya bisa dilihat dibawah ini  

```
def pearsonCalculate(data, u,v):
    "i, j is an index"
    atas=0; bawah_kiri=0; bawah_kanan = 0
    for k in range(len(data)):
        atas += (data[k,u] - meanFitur[u]) * (data[k,v] - meanFitur[v])
        bawah_kiri += (data[k,u] - meanFitur[u])**2
        bawah_kanan += (data[k,v] - meanFitur[v])**2
    bawah_kiri = bawah_kiri ** 0.5
    bawah_kanan = bawah_kanan ** 0.5
    return atas/(bawah_kiri * bawah_kanan)
def meanF(data):
    meanFitur=[]
    for i in range(len(data[0])):
        meanFitur.append(sum(data[:,i])/len(data))
    return np.array(meanFitur)

```

### Clustering

Clustering merupakan pengelompokan data menjadi k-kelompok (dengan k merupakan banyak kelompok). Pengelompkan tersebut berdasarkan ciri yang mirip. Pada kasus ini, maka ciri yang mirip bisa diketahui dari kata yang menjadi ciri dari setiap dokumen.
Metode Clustering sendiri ada banyak. Salah duanya adalah K-Means Clustering dan Fuzzy C-Means Clustering.
Setelah dilakukan proses Clustering, perlu kita cari nilai Silhouette Coefficient untuk melihat apakah hasil cluster tersebut sudah bagus atau tidak.

#### K-Means

```
# Clustering
kmeans = KMeans(n_clusters=5, random_state=0).fit(tfidf_matrix.todense())

for i in range(len(kmeans.labels_)):
    print("Doc %d =>> cluster %d" %(i+1, kmeans.labels_[i]))

```

Code di atas adalah code untuk melakukan clustering. Pada contoh ini cluster dibagi menjadi 5. Banyak cluster bisa diubah sesuai kebutuhan.

#### Klasifikasi

Berbeda dengan Clustering yang mengelompokkan data, klasifikasi lebih mirip pada peramalan data dengan merujuk kategori. Contoh simplenya, seperti peramalan golf. Jika cuaca cerah, angin tidak kencang, maka akan diadakan pertandingan golf. Jika cuaca cerah tapi angin kencang, maka pertandingan golf dibatalkan.
Proses ini biasanya memerlukan data latih dan data test. Untuk menguji keberhasilannya, diperlukan untuk menghitung akurasi atau confution matrix.
Metode-metode yang sering dipakai untuk klasifikasi di antaranya Naive Bayes.

parameter dari fuzzy c-means berturut-turut : data, jumlahCluster, pembobot, erorMaksimal, serta IterasiMaksimal.

#### Naive Bayes

Naive bayes termasuk pada supervised learning berdasarkan teorema Bayes, dengan asumsi "naive", bahwa setiap fitur tidak saling terkait (independen). Terdapat beberapa jenis Naive Bayes, seperti Gaussian NB, Multinomial NB, dll. Namun pada kasus text classification, Multinomial lebih cocok digunakan.
Sebelum itu, kita perlu membagi data menjadi dua bagian: data training, dan data test

```
x_train, x_test, y_train, y_test = train_test_split(xBaru1, label, test_size=0.33, random_state=0)
model = MultinomialNB().fit(x_train, y_train)
predicted = model.predict(x_test)

```

