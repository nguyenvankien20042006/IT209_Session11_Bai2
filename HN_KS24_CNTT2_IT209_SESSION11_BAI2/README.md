# Bài 2: Biên dịch đóng gói tệp ứng dụng .jar với Gradle và lưu trữ Artifacts

## 1. Mục tiêu
- Thực thi câu lệnh biên dịch và đóng gói ứng dụng **Spring Boot** thành tệp thực thi `.jar` bằng **Gradle** (`./gradlew bootJar`).
- Lưu trữ thành phẩm biên dịch (`.jar`) từ thư mục `build/libs/` lên **GitHub Actions** bằng `actions/upload-artifact@v4`.

---

## 2. Cấu trúc thư mục dự án

```text
SESSION11_BAI2/
├── .github/
│   └── workflows/
│       └── ci.yml                  # Kịch bản GitHub Actions CI (Test + bootJar + Upload Artifact)
├── gradle/
│   └── wrapper/
│       ├── gradle-wrapper.jar
│       └── gradle-wrapper.properties
├── homework/
│   └── session_11/
│       └── ex2/                    # Thư mục sản phẩm nộp bài theo đường dẫn gợi ý
│           ├── .github/
│           │   └── workflows/
│           │       └── ci.yml
│           └── README.md           # Hướng dẫn kiểm tra & chụp ảnh nộp bài
├── src/
│   ├── main/java/com/example/
│   │   ├── Application.java
│   │   └── Calculator.java
│   └── test/java/com/example/
│       └── CalculatorTest.java     # Bộ kiểm thử JUnit 5 cho Spring Boot
├── .gitattributes                  # Đảm bảo gradlew giữ chuẩn LF trên Linux runner
├── .gitignore
├── build.gradle
├── gradlew
├── gradlew.bat
├── settings.gradle
└── README.md
```

---

## 3. Nội dung tệp kịch bản `.github/workflows/ci.yml`

```yaml
name: Java Spring Boot CI (Gradle)

on:
  push:
    branches: [ "main" ]

jobs:
  test:
    name: Run Unit Tests
    runs-on: ubuntu-latest

    steps:
      - name: Checkout mã nguồn
        uses: actions/checkout@v5

      - name: Thiết lập môi trường Java JDK 17
        uses: actions/setup-java@v5
        with:
          java-version: '17'
          distribution: 'temurin'

      - name: Cấp quyền cho Gradle Wrapper
        run: chmod +x gradlew

      - name: Thực thi JUnit Test với Gradle
        run: ./gradlew test

      - name: Đóng gói tệp JAR với Gradle
        run: ./gradlew bootJar

      - name: Lưu trữ thành phẩm JAR Artifact
        uses: actions/upload-artifact@v4
        with:
          name: app-jar
          path: build/libs/*.jar
```

---

## 4. Giải thích các bước bổ sung trong Bài 2

| Bước (Step) | Cấu hình | Ý nghĩa |
| :--- | :--- | :--- |
| **Đóng gói tệp JAR với Gradle** | `run: ./gradlew bootJar` | Thực thi task `bootJar` của Spring Boot Gradle Plugin để biên dịch mã nguồn và đóng gói thành tệp thực thi `.jar` trong thư mục `build/libs/`. |
| **Lưu trữ thành phẩm JAR Artifact** | `uses: actions/upload-artifact@v4` | Tải tệp thành phẩm sau khi build lên hệ thống lưu trữ Artifacts của GitHub Actions. |
| `with.name` | `app-jar` | Đặt tên định danh cho gói artifact hiển thị trong phần **Summary** của Workflow. |
| `with.path` | `build/libs/*.jar` | Chỉ định đường dẫn tất cả các tệp `.jar` được sinh ra trong thư mục `build/libs/` để đính kèm lên GitHub Actions. |

---

## 5. Hướng dẫn kiểm tra & nộp bài trên GitHub Actions

### Bước 1: Đẩy commit mới lên nhánh `main`
Chạy các lệnh sau tại thư mục `SESSION11_BAI2` (hoặc trên repository từ EX1):

```bash
git init
git add .
git commit -m "feat(ex2): package Spring Boot JAR with bootJar and upload app-jar artifact"
git branch -M main
git remote add origin <URL_GITHUB_REPO_CUA_BAN>
git push -u origin main
```

### Bước 2: Kiểm tra Artifact trên GitHub Actions
1. Truy cập vào **GitHub Repository** -> Chọn tab **Actions**.
2. Nhấn vào luồng chạy mới nhất của workflow **Java Spring Boot CI (Gradle)**.
3. Chọn mục **Summary** của luồng chạy (menu bên trái), kéo xuống phần **Artifacts** ở cuối trang.
4. Đảm bảo xuất hiện tệp artifact **`app-jar`** (được đóng gói từ `build/libs/*.jar`) cho phép tải về máy.
5. **Chụp ảnh màn hình** phần **Summary** trên tab Actions hiển thị tệp artifact **`app-jar`** và lưu vào thư mục `homework/session_11/ex2/` để hoàn tất nộp bài.
