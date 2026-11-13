QUẢN LÝ LÁN TRẠI — PWA + NGUỒN WRAPPER APK

NỘI DUNG
- index.html: ứng dụng gốc được giữ phần lớn cấu trúc/chức năng, thêm OCR chữ in thử nghiệm và gia cố lưu dữ liệu.
- manifest.webmanifest, sw.js, icon-192.png, icon-512.png: PWA.
- capacitor-source/: nguồn wrapper để tự biên dịch APK bằng Capacitor/Android Studio. Đây là mã nguồn wrapper, KHÔNG phải APK đã biên dịch.

PWA — TRIỂN KHAI
1. Tải ZIP, giải nén toàn bộ thư mục lantrai_pwa_package.
2. Đưa index.html, manifest.webmanifest, sw.js, icon-192.png, icon-512.png lên cùng một thư mục trên hosting HTTPS (Netlify có thể dùng). Không mở trực tiếp bằng file://.
3. Mở URL HTTPS bằng Chrome trên Android/Xiaomi khi có mạng; đợi tải xong, mở lại ít nhất một lần để Service Worker cache giao diện.
4. Menu Chrome ⋮ > Cài đặt ứng dụng/Thêm vào màn hình chính.
5. Thử ở chế độ máy bay; kiểm tra riêng Excel, QR, OCR, camera và sao lưu.

APK — BIÊN DỊCH TRÊN WINDOWS
1. Cài Node.js LTS và Android Studio, mở Android Studio một lần để cài Android SDK/JDK theo hướng dẫn của Capacitor.
2. Mở thư mục capacitor-source trong Terminal/PowerShell.
3. Chạy: npm install
4. Chạy: npx cap add android
5. Chạy: npx cap sync android
6. Chạy: npx cap open android
7. Android Studio > Build > Build Bundle(s) / APK(s) > Build APK(s). APK debug thường nằm trong android/app/build/outputs/apk/debug/.
8. Cài APK lên máy thử, cấp quyền camera, kiểm tra lưu/khôi phục và xuất Excel.
9. Sau khi sửa HTML, chép bản mới vào capacitor-source/www/ rồi chạy npx cap sync android và build lại.

QR VÀ OCR
- QR: ưu tiên đọc QR CCCD và tự điền trường khi nội dung QR theo cấu trúc hỗ trợ.
- OCR chữ in: nút “Đọc chữ OCR” dùng Tesseract.js ngôn ngữ vie+eng để gợi ý số CCCD 12 số, họ tên, ngày sinh và giới tính; kết quả phụ thuộc ảnh, ánh sáng, góc chụp, chất lượng chữ. BẮT BUỘC đối chiếu thủ công trước khi lưu. OCR có thể nhận nhầm số/ngày/tên; không dùng kết quả OCR như xác minh danh tính.
- GIỚI HẠN OFFLINE QUAN TRỌNG: môi trường tạo gói không có kết nối để tải và đóng gói đầy đủ Tesseract worker/core và dữ liệu ngôn ngữ vie+eng. Do đó OCR lần đầu cần mạng và có thể không hoạt động khi hoàn toàn offline; Service Worker chỉ thử cache script chính, không thể đảm bảo cache toàn bộ engine/dữ liệu mà worker tải. QR/Excel cũng còn phụ thuộc thư viện CDN nếu chúng chưa được cache. Muốn cam kết offline hoàn toàn, cần đóng gói tất cả thư viện + traineddata vào cùng gói hoặc tích hợp OCR native Android on-device.

DỮ LIỆU VÀ BẢO MẬT
- Dữ liệu lưu cục bộ trên trình duyệt/thiết bị hiện tại; không đồng bộ giữa cán bộ hoặc thiết bị. Xóa dữ liệu trình duyệt có thể làm mất dữ liệu; xuất sao lưu JSON/Excel thường xuyên và thử phục hồi trên bản sao.
- Không đưa dữ liệu CCCD thật lên hosting công khai; khóa màn hình thiết bị và bảo vệ tệp sao lưu. Mã phê duyệt phía client trong mã gốc không phải cơ chế bảo mật mạnh.
- PWA không bảo đảm chạy nền liên tục khi Android đóng app hoặc tiết kiệm pin.

GIỚI HẠN KIỂM THỬ
- Đã kiểm tra cú pháp JavaScript và cấu trúc tệp ở môi trường dựng gói; chưa chạy kiểm thử thực địa trên camera/Android/Xiaomi.
- Chưa biên dịch APK trong môi trường này; cần build bằng Android Studio theo hướng dẫn.
- Chưa xác minh OCR trên ảnh CCCD thực tế và chưa thể chứng nhận độ chính xác. Không dùng CCCD thật để thử nếu chưa có quy trình bảo vệ dữ liệu.
- Trước triển khai: thử thêm/sửa/xóa, trùng CCCD, đầy/khóa phòng, điểm danh, chuyển phòng, nghỉ việc, nhập/xuất Excel, sao lưu/khôi phục, camera và chế độ máy bay.
