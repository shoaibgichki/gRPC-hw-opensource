# grpcurl Test Dokümantasyonu

Bu dosya, University Library gRPC servislerinin grpcurl aracı ile test edilmesi için gerekli komutları ve beklenen sonuçları içermektedir.

## 🚀 Test Ortamı Kurulumu

```bash
# Sunucuyu başlatın
cd src/server
go run main.go

# Yeni terminal açın ve testleri çalıştırın
```

## 📚 BookService Testleri

### 1. Kitap Listesi Alma
```bash
grpcurl -plaintext -d '{"page_size": 10}' localhost:50051 university.library.BookService/ListBooks
```

**Beklenen Çıktı:**
```json
{
  "books": [
    {
      "id": "550e8400-e29b-41d4-a716-446655440001",
      "title": "Introduction to Algorithms",
      "author": "Thomas H. Cormen",
      "isbn": "9780262033848",
      "publisher": "MIT Press",
      "pageCount": 1312,
      "stock": 5
    }
  ],
  "nextPageToken": ""
}
```

### 2. Tekil Kitap Getirme
```bash
grpcurl -plaintext -d '{"id": "550e8400-e29b-41d4-a716-446655440001"}' localhost:50051 university.library.BookService/GetBook
```

**Beklenen Çıktı:**
```json
{
  "book": {
    "id": "550e8400-e29b-41d4-a716-446655440001",
    "title": "Introduction to Algorithms",
    "author": "Thomas H. Cormen",
    "isbn": "9780262033848",
    "publisher": "MIT Press",
    "pageCount": 1312,
    "stock": 5
  }
}
```

### 3. Yeni Kitap Ekleme
```bash
grpcurl -plaintext -d '{
  "book": {
    "title": "Clean Code",
    "author": "Robert C. Martin",
    "isbn": "9780132350884",
    "publisher": "Prentice Hall",
    "pageCount": 464,
    "stock": 3
  }
}' localhost:50051 university.library.BookService/CreateBook
```

**Beklenen Çıktı:**
```json
{
  "book": {
    "id": "generated-uuid-here",
    "title": "Clean Code",
    "author": "Robert C. Martin",
    "isbn": "9780132350884",
    "publisher": "Prentice Hall",
    "pageCount": 464,
    "stock": 3
  }
}
```

### 4. Kitap Güncelleme
```bash
grpcurl -plaintext -d '{
  "book": {
    "id": "550e8400-e29b-41d4-a716-446655440001",
    "title": "Introduction to Algorithms - 4th Edition",
    "author": "Thomas H. Cormen",
    "isbn": "9780262046305",
    "publisher": "MIT Press",
    "pageCount": 1376,
    "stock": 8
  }
}' localhost:50051 university.library.BookService/UpdateBook
```

### 5. Kitap Silme
```bash
grpcurl -plaintext -d '{"id": "550e8400-e29b-41d4-a716-446655440001"}' localhost:50051 university.library.BookService/DeleteBook
```

**Beklenen Çıktı:**
```json
{
  "success": true,
  "message": "Book deleted successfully"
}
```

## 👤 StudentService Testleri

### 1. Öğrenci Listesi Alma
```bash
grpcurl -plaintext -d '{"page_size": 10}' localhost:50051 university.library.StudentService/ListStudents
```

**Beklenen Çıktı:**
```json
{
  "students": [
    {
      "id": "660e8400-e29b-41d4-a716-446655440001",
      "name": "Ahmet Yılmaz",
      "studentNumber": "2019001001",
      "email": "ahmet.yilmaz@university.edu.tr",
      "isActive": true
    }
  ],
  "nextPageToken": ""
}
```

### 2. Tekil Öğrenci Getirme
```bash
grpcurl -plaintext -d '{"id": "660e8400-e29b-41d4-a716-446655440001"}' localhost:50051 university.library.StudentService/GetStudent
```

### 3. Yeni Öğrenci Ekleme
```bash
grpcurl -plaintext -d '{
  "student": {
    "name": "Mehmet Demir",
    "studentNumber": "2020001002",
    "email": "mehmet.demir@university.edu.tr",
    "isActive": true
  }
}' localhost:50051 university.library.StudentService/CreateStudent
```

### 4. Öğrenci Güncelleme
```bash
grpcurl -plaintext -d '{
  "student": {
    "id": "660e8400-e29b-41d4-a716-446655440001",
    "name": "Ahmet Yılmaz",
    "studentNumber": "2019001001",
    "email": "ahmet.yilmaz@university.edu.tr",
    "isActive": false
  }
}' localhost:50051 university.library.StudentService/UpdateStudent
```

### 5. Öğrenci Silme
```bash
grpcurl -plaintext -d '{"id": "660e8400-e29b-41d4-a716-446655440001"}' localhost:50051 university.library.StudentService/DeleteStudent
```

## 🔄 LoanService Testleri

### 1. Ödünç Alma Kayıtları Listesi
```bash
grpcurl -plaintext -d '{"page_size": 10}' localhost:50051 university.library.LoanService/ListLoans
```

**Beklenen Çıktı:**
```json
{
  "loans": [
    {
      "id": "770e8400-e29b-41d4-a716-446655440001",
      "studentId": "660e8400-e29b-41d4-a716-446655440001",
      "bookId": "550e8400-e29b-41d4-a716-446655440001",
      "loanDate": "2024-01-15",
      "returnDate": "",
      "status": "ONGOING"
    }
  ],
  "nextPageToken": ""
}
```

### 2. Tekil Ödünç Alma Kaydı Getirme
```bash
grpcurl -plaintext -d '{"id": "770e8400-e29b-41d4-a716-446655440001"}' localhost:50051 university.library.LoanService/GetLoan
```

### 3. Kitap Ödünç Alma
```bash
grpcurl -plaintext -d '{
  "studentId": "660e8400-e29b-41d4-a716-446655440001",
  "bookId": "550e8400-e29b-41d4-a716-446655440001"
}' localhost:50051 university.library.LoanService/BorrowBook
```

**Beklenen Çıktı:**
```json
{
  "loan": {
    "id": "generated-uuid-here",
    "studentId": "660e8400-e29b-41d4-a716-446655440001",
    "bookId": "550e8400-e29b-41d4-a716-446655440001", 
    "loanDate": "2024-01-20",
    "returnDate": "",
    "status": "ONGOING"
  }
}
```

### 4. Kitap İade Etme
```bash
grpcurl -plaintext -d '{"loan_id": "770e8400-e29b-41d4-a716-446655440001"}' localhost:50051 university.library.LoanService/ReturnBook
```

**Beklenen Çıktı:**
```json
{
  "loan": {
    "id": "770e8400-e29b-41d4-a716-446655440001",
    "studentId": "660e8400-e29b-41d4-a716-446655440001",
    "bookId": "550e8400-e29b-41d4-a716-446655440001",
    "loanDate": "2024-01-15",
    "returnDate": "2024-01-20",
    "status": "RETURNED"
  }
}
```

## 🔍 Servis Keşfi

Mevcut servisleri listelemek için:
```bash
grpcurl -plaintext localhost:50051 list
```

Belirli bir servisin metodlarını görmek için:
```bash
grpcurl -plaintext localhost:50051 list university.library.BookService
```

## 📝 Test Notları

- Tüm testler localhost:50051 portunda çalışan sunucu ile yapılmıştır
- UUID'ler sunucu tarafından otomatik olarak üretilmektedir
- Tarih formatları YYYY-MM-DD şeklindedir
- Enum değerleri büyük harfle yazılmalıdır (ONGOING, RETURNED, LATE)
- Pagination token'ları opsiyoneldir ve boş bırakılabilir