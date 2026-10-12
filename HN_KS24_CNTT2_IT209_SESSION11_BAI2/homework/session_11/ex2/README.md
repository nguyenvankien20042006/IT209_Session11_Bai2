# Hướng Dẫn Nộp Bài 2 (`homework/session_11/ex2/`)

## 1. Sản phẩm trong thư mục nộp bài
- Tệp kịch bản [`.github/workflows/ci.yml`](./.github/workflows/ci.yml) được nâng cấp từ Bài 1 và bổ sung 2 bước theo đúng yêu cầu đề bài:
  - **Đóng gói tệp JAR với Gradle**: Thực thi `./gradlew bootJar` để biên dịch và đóng gói ứng dụng Spring Boot thành tệp `.jar` trong thư mục `build/libs/`.
  - **Lưu trữ thành phẩm JAR Artifact**: Sử dụng `actions/upload-artifact@v4` với tên artifact là `app-jar` và đường dẫn `build/libs/*.jar`.

## 2. Hướng dẫn đẩy code và chụp ảnh màn hình GitHub Actions
1. Đẩy commit mới lên nhánh `main` của GitHub Repository:
   ```bash
   git init
   git add .
   git commit -m "feat(ex2): add bootJar build step and upload app-jar artifact"
   git branch -M main
   git remote add origin <URL_GITHUB_REPO_CUA_BAN>
   git push -u origin main
   ```
2. Truy cập tab **Actions** trên GitHub Repository -> Chọn luồng chạy mới nhất của workflow **Java Spring Boot CI (Gradle)**.
3. Sau khi Workflow chạy xong (tích xanh), mở mục **Summary** (ở góc trên bên trái của luồng chạy) và cuộn xuống phần **Artifacts** ở cuối trang.
4. Kiểm tra sự xuất hiện của tệp artifact **`app-jar`** cho phép tải về máy.
5. Chụp ảnh màn hình phần **Summary** hiển thị artifact `app-jar` và lưu vào thư mục `homework/session_11/ex2/` (ví dụ: `homework/session_11/ex2/artifact-summary.png`).
