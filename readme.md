
# Rust sqlx öğrenme
Bu projede rust ile sqlx paketini kullanarak postgres bağlantısı nasıl yapılır onu öğreneceğiz.

# Kurulum
Daha önceden kurmadıysak bilgisayarıma sqlx-cli kurmamız gerekiyor. Bu paket migration ve veritabanı işlemleri için yardımcı olacak.

```bash 
  cargo install sqlx-cli --no-default-features --features rustls,postgres
```

# Veritabanını oluşturma
Proje dizinindeki .env.example dosyasını .env dosyasına kopyalıyoruz. Eğer postgres bağlantı bilgileri farklı ise kendi bilgisayarımıza göre ayarlıyoruz. Ardından aşağıdaki komut ile veritabanını oluşturmuş oluyoruz.

**Unutma** veritabanı ismini her zaman .env dosyasından değiştirebilirsin.

```bash 
  sqlx database create
```

# İlk migration
Veritabanında tablo oluşturma işlemlerini kolaylaştırmak ve ekip çalışmalarında güncel veritabanı yapısını korumak için migration yazarız.

Migration oluşrumak için aşağıdaki komutu kullanacağız.

```bash 
  sqlx migrate add tablo_adi
```

Örnek migration:
```bash 
    -- Add migration script here
    CREATE TABLE example (
        id SERIAL PRIMARY KEY,
        name TEXT NOT NULL
    );
```

Son olarak tabloyu veritabanına eklemek için aşağıdaki komutu kullanıyoruz.

```bash 
  sqlx migrate run
```

Eğer değişiklikleri geri almak istersek de aşağıdaki komutu kullanabiliriz.

```bash 
  sqlx migrate revert
```