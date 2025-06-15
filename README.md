# University Library gRPC Service

Bu proje, bir üniversite kütüphanesi için gRPC tabanlı bir servis sistemidir. Proje kitap, öğrenci ve ödünç alma işlemlerini yönetmek için üç ana servis içermektedir.

## 🏗️ Proje Yapısı

```
/
├── university.proto             # Protobuf tanımları
├── README.md                   # Bu dosya
├── grpcurl-tests.md            # Test dokümantasyonu
├── src/
│   ├── server/
│   │   ├── main.go             # Go server implementasyonu
│   │   ├── handlers/           # Servis handler'ları
│   │   └── models/             # Data modelleri
│   └── client/
│       └── main.go             # Go client implementasyonu
├── go.mod                      # Go modül dosyası
├── go.sum                      # Go dependencies
└── DELIVERY.md                 # Teslimat dosyası
```

## 🚀 Kurulum ve Çalıştırma

### Gereksinimler
- Go 1.19+
- Protocol Buffers compiler (protoc)
- grpcurl (test için)

### Kurulum
```bash
# Projeyi klonlayın
git clone <repo-url>
cd university-library-grpc

# Go bağımlılıklarını yükleyin
go mod tidy

# Protobuf dosyalarını derleyin
protoc --go_out=. --go_opt=paths=source_relative \
    --go-grpc_out=. --go-grpc_opt=paths=source_relative \
    university.proto
```

### Sunucuyu Çalıştırma
```bash
cd src/server
go run main.go
# Sunucu http://localhost:50051 adresinde çalışacak
```

### İstemciyi Çalıştırma
```bash
cd src/client  
go run main.go
```

## 📡 Servisler

### BookService
- `ListBooks`: Tüm kitapları listeler
- `GetBook`: Belirli bir kitabı getirir
- `CreateBook`: Yeni kitap ekler
- `UpdateBook`: Kitap bilgilerini günceller
- `DeleteBook`: Kitap siler

### StudentService
- `ListStudents`: Tüm öğrencileri listeler
- `GetStudent`: Belirli bir öğrenciyi getirir
- `CreateStudent`: Yeni öğrenci ekler
- `UpdateStudent`: Öğrenci bilgilerini günceller
- `DeleteStudent`: Öğrenci siler

### LoanService
- `ListLoans`: Tüm ödünç alma kayıtlarını listeler
- `GetLoan`: Belirli bir ödünç alma kaydını getirir
- `BorrowBook`: Kitap ödünç alma işlemi
- `ReturnBook`: Kitap iade işlemi

## 🧪 Test

grpcurl kullanarak servisleri test edebilirsiniz. Detaylı test komutları ve sonuçları için `grpcurl-tests.md` dosyasına bakın.

Örnek test komutu:
```bash
grpcurl -plaintext -d '{"page_size": 10}' localhost:50051 university.library.BookService/ListBooks
```

## 📊 Veri Modelleri

### Book
- id (string, UUID)
- title (string)  
- author (string)
- isbn (string, ISBN-13 format)
- publisher (string)
- page_count (int32)
- stock (int32)

### Student
- id (string, UUID)
- name (string)
- student_number (string)
- email (string, email format)
- is_active (bool)

### Loan
- id (string, UUID)
- student_id (string)
- book_id (string)
- loan_date (string, date format)
- return_date (string, date format, nullable)
- status (enum: ONGOING, RETURNED, LATE)

## 🛠️ Teknolojiler
- Protocol Buffers (proto3)
- gRPC
- Go
- UUID generation
- Mock data storage