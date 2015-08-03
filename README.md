# docker-locust

Docker base image for locust master/slave/standalone.

## Step by Step Cara Kerja Locust Test Dibawah Docker
## Email : addhe.warman@gmail.com
## Slack : https://docker-locustio-indo.slack.com/messages/@slackbot/

Saya akan jelaskan sedetail mungkin, kalau ada yang kurang detail dan bila ada
yang butuh bantuan kindly email or slack me

# Step 1

Install Docker, lalu git clone atau Copy paste tulisan di antara ``` dibawah ini,
pindahkan ke file dengan nama Dockerfile, saya juga meletakkan file Dockerfile di 
repository ini sebagai bahan refrensi. Thanks to hakobera/locust


```
FROM hakobera/locust

ADD ./test /test
ENV SCENARIO_FILE /test/locustfile.py
```

atau cara termudah


```
git clone git@github.com:addhewarman/docker-locust.git
cd docker-locust
```


# Step 2

Mari kita build docker kita dengan file diatas, eksekusi perintah ini di 
terminal linux anda, gunakan sudo jika di perlukan.

```
$ sudo docker build -t locust-test .
```

# Step 3

Bila telah selesai check images docker anda apakah sudah ada di docker,
gunakan sudo jika di perlukan

```
$ sudo docker ps images
```

# Step 4

Sampai di step ini anda telah memiliki locust docker ready to use tanpa
harus install dan download semua dependency yang dibutuhkan oleh locust.
langkah selanjutnya adalah mengcopy file locustfile.py atau menggunakan
file locustfile.py sample yang saya letakkan di repository ini bila anda
menjalan Dockerfile dari folder yang berbeda ketika anda checkout.
Nama Image docker akan menjadi penentu misalnya

ubuntu@awan-docker:~/test$ sudo docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
hakobera/locust     latest              ad16872d7009        6 months ago        416.5 MB


```
$ sudo docker run -p {port_main}:{port_docker} -e LOCUST_MODE=standalone -e TARGET_URL=http://{nama_website} {nama_docker_mages}
$ sudo docker run -p 8089:8089 -e LOCUST_MODE=standalone -e TARGET_URL=http://www.namawebsiteyangmauditest.com hakobera/locust
```


# Step 5

folder test akan di mount kedalam docker, jadi anda dapat melakukan perubahan
pada file test anda, sample locust file akan test halaman depan website saja
untuk lebih custom bisa di lihat di documentasi resminya atau request nanti
akan saya buatkan.



### standalone

```
$ docker run -e LOCUST_MODE=standalone -e TARGET_URL=http://{{nama_website}} {{ locust_image }}
$ docker run -e LOCUST_MODE=standalone -e TARGET_URL=http://www.google.com locust-test
```

### distribution

#### master

```
$ docker run \
  -e LOCUST_MODE=master \
  -e TARGET_URL=http://<your-target-server> \
  locust-test
```

#### slave

```
$ docker run \
  -e LOCUST_MODE=slave \
  -e MASTER_HOST=http://<master-server-ip> \
  -e TARGET_URL=https://<your-target-server> \
  locust-test
```
