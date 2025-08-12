# Rust + SQLx (PostgreSQL) — Minimal Öğrenme Projesi

Bu repoda **yalnızca** SQLx kullanarak PostgreSQL ile bağlantı kurma ve **migration** yönetimini öğreniyoruz.  
API, sorgu vb. kodlar **bilerek yok**; onları sonraki adımlarda ekleyeceğiz.

## Amaç
- `sqlx-cli` ile veritabanı oluşturmak/silmek  
- Migration eklemek, çalıştırmak ve geri almak  
- Basit ve taşınabilir bir proje iskeleti

## Önkoşullar
- Rust (güncel stable toolchain)
- PostgreSQL (lokalde kurulu)
- `~/.cargo/bin` klasörü PATH’e ekli (Windows’ta özellikle)

## Kurulum

### 1) sqlx-cli yükle
`sqlx-cli`, veritabanı ve migration komutlarını sağlar.
```bash
cargo install sqlx-cli --no-default-features --features rustls,postgres
```
> `rustls` = OpenSSL’e bağımlı kalmadan TLS desteği. Yerelde TLS kullanmasan bile sorun çıkarmaz.

### 2) Bağlantı ayarı (`.env`)
Proje kökünde `.env.example` dosyasını `.env` olarak kopyala ve gerekirse düzenle:
```env
# örnek:
DATABASE_URL=postgres://postgres:postgres@localhost:5432/sqlx_min
```

> **Not:** Veritabanı adını/şifresini yalnızca `.env` üzerinden değiştirmek yeterli.

## Veritabanı Komutları

### Oluştur
```bash
sqlx database create
```

### Sil (gerekirse)
```bash
sqlx database drop -y
```

## Migration Akışı

### Yeni migration ekle
```bash
sqlx migrate add init
```
Bu komut `migrations/` klasöründe zaman damgalı bir SQL dosyası üretir.  
Örnek içerik:
```sql
-- migrations/2025xxxxxx_init.sql
CREATE TABLE example (
    id SERIAL PRIMARY KEY,
    name TEXT NOT NULL
);
```

### Migration’ı çalıştır
```bash
sqlx migrate run
```

### Geri al (son migration)
```bash
sqlx migrate revert
```

## Sık Kullanılan Komutlar (Özet)
```bash
# DB oluştur / sil
sqlx database create
sqlx database drop -y

# Migration ekle / uygula / geri al / durum
sqlx migrate add <ad>
sqlx migrate run
sqlx migrate revert
sqlx migrate info
```

## Yapı (Minimal)
```
.
├─ .env                 # DATABASE_URL burada
├─ .env.example         # örnek bağlantı ayarları
├─ migrations/          # SQL migration dosyaları
├─ Cargo.toml           # yalnızca sqlx bağımlılığı
└─ src/
   └─ main.rs           # (boş veya minimal)
```

**Örnek `Cargo.toml`** (yalnız SQLx + Postgres + Tokio + rustls):
```toml
[package]
name = "sqlx-min"
version = "0.1.0"
edition = "2024"

[dependencies]
sqlx = { version = "0.8", default-features = false, features = ["postgres", "runtime-tokio-rustls"] }
```

**Örnek `src/main.rs`** (şimdilik boş):
```rust
fn main() {}
```

## Sık Karşılaşılan Sorunlar

- **`sqlx` komutu bulunamadı:** `~/.cargo/bin` dizinini PATH’e ekle (Windows’ta “Ortam Değişkenleri”nden).
- **Postgres’e bağlanamıyor:** `DATABASE_URL` formatını ve kullanıcı/şifre/portu kontrol et.
- **OpenSSL hataları:** `sqlx-cli`’yi `rustls` ile kurduğun bu projede tipik olarak gerekmez; yine de eski kurulum kalıntıları varsa kaldırıp tekrar yükle.

## Lisans
MIT
