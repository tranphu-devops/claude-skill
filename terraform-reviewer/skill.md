# Terraform Code Review Checklist

## 1. Cấu trúc & Mô-đun (Modularization)
- [ ] Code đã được chia thành các module nhỏ, tách biệt theo logic chức năng chưa? (Tránh `main.tf` khổng lồ).
- [ ] Các module có thể tái sử dụng cho các môi trường dev/test/prod không?
- [ ] Có xuất bản module nội bộ hoặc sử dụng registry hợp lệ không?

## 2. Quản lý State (State Management)
- [ ] State file (`terraform.tfstate`) **không** được commit lên git.
- [ ] Đã cấu hình Remote Backend (GCS bucket trên GCP) để chia sẻ state an toàn chưa?
- [ ] Có cơ chế locking (state locking) để tránh xung đột khi nhiều người cùng apply không?

## 3. Phiên bản & Phụ thuộc (Version Pinning)
- [ ] Provider và Module version đã được "pin" (cố định) cụ thể chưa? (Tránh dùng `~> x.y` quá rộng hoặc không có version).
- [ ] Có đảm bảo tính xác định (deterministic) khi build lại infra không?

## 4. Quản lý Môi trường (Environment Handling)
- [ ] Việc phân tách môi trường (dev/test/prod) được thực hiện như thế nào? (Workspaces, thư mục riêng, hay biến môi trường?).
- [ ] Nếu dùng Workspaces, có đảm bảo không bị overuse cho các hạ tầng quá lớn không?

## 5. Quy trình Deploy (Deployment Process)
- [ ] Luôn chạy `terraform plan` trước khi `apply` để xem trước thay đổi.
- [ ] Có quy trình CI/CD tự động chạy `plan` và yêu cầu review/approve trước khi apply không?
- [ ] Có tích hợp Terraform Cloud/Atlantis để quản lý workflow team chưa?

## 6. Tối ưu hóa Code (Code Optimization)
- [ ] Có sử dụng `locals` và `for_each` để giảm bớt code lặp (DRY principle) không?
- [ ] Code đã được format chuẩn (`terraform fmt`) và lint (`tflint`) chưa?

## 7. Bảo mật & Bí mật (Security & Secrets)
- [ ] **KHÔNG** có credential, key, secret nào nằm trong file `.tf` hoặc `.tfvars`.
- [ ] Secrets được quản lý qua Secret Manager (GCP Secret Manager) và inject runtime không?
- [ ] Các resource nhạy cảm có được bảo vệ (sensitive = true) trong output không?

## 8. Giám sát & Drift Detection (Monitoring & Drift)
- [ ] Có quy trình tự động chạy `terraform plan` định kỳ (ví dụ: tuần) để phát hiện drift không?
- [ ] Cảnh báo được kích hoạt khi có sự khác biệt giữa code và thực tế trên GCP không?

## 9. Quản lý Chi phí & Tags (Tagging & Cost)
- [ ] **TẤT CẢ** tài nguyên trên GCP đều có Tag/Label (Project, Owner, Environment, CostCenter).
- [ ] Tags có nhất quán để phục vụ việc báo cáo chi phí (billing) và quản lý tài sản không?

## 10. Tài liệu & Comment
- [ ] Các biến đầu vào (variables) và đầu ra (outputs) có được mô tả rõ ràng (description) không?
- [ ] Có tài liệu hướng dẫn cách deploy và rollback cho module không?
