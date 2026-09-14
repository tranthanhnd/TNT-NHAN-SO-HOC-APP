# Hướng dẫn build file .exe cho T&T Nhân Số Học App

Khung dự án này em đã chuẩn bị sẵn và **kiểm thử thật** (build + chạy thử thành công trên Linux) trước khi gửi anh — đảm bảo cấu hình đúng, không phải đoán mò. Sáng mai anh chỉ cần làm đúng các bước dưới đây, không cần sửa code gì thêm.

## Cấu trúc thư mục (đã có sẵn trong file zip)

```
TT-NHAN-SO-HOC-APP/
├── dist/
│   └── index.html          ← chính là app V1.9 (bản đầy đủ, đã nhúng logo)
├── src-tauri/
│   ├── icons/               ← icon app lấy từ logo T&T (vương miện vàng)
│   ├── tauri.conf.json       ← cấu hình app: tên, phiên bản, cửa sổ, icon
│   ├── Cargo.toml
│   └── src/ (main.rs, lib.rs) ← phần lõi Tauri, không cần đụng vào
├── .github/workflows/build.yml  ← quy trình tự động build ra .exe
├── package.json
└── HUONG-DAN-BUILD-EXE.md    ← chính là file này
```

## Bước 1 — Tạo repo GitHub mới

1. Đăng nhập GitHub (tài khoản anh vẫn dùng cho Gia Phả Việt).
2. Tạo repo mới, đặt tên ví dụ: `TT-NHAN-SO-HOC-APP` — chọn **Private**.
3. **Không** tick tạo sẵn README/gitignore (để trống, mình sẽ đẩy code có sẵn lên).

## Bước 2 — Đưa code vào máy qua GitHub Desktop

1. Mở GitHub Desktop → File → Clone repository → chọn đúng repo vừa tạo.
2. Chọn nơi lưu **ngoài OneDrive** (ví dụ `C:\GIAPHAVIET\TT-NHAN-SO-HOC-APP` hoặc thư mục tương tự) — bài học từ lần làm Gia Phả Việt: để trong OneDrive dễ bị lỗi Git không nhận diện thay đổi file.
3. Giải nén toàn bộ nội dung file zip anh nhận vào đúng thư mục vừa clone (đè lên, giữ nguyên cấu trúc thư mục).

## Bước 3 — Bật quyền cho GitHub Actions

Trên trang GitHub của repo: vào **Settings → Actions → General → Workflow permissions**, chọn **"Read and write permissions"** rồi bấm Save. (Bước này bắt buộc, thiếu là Action sẽ báo lỗi không tạo được Release — đúng lỗi đã gặp hồi làm Gia Phả Việt.)

## Bước 4 — Đẩy code lên GitHub

Trong GitHub Desktop: viết summary bất kỳ (ví dụ "Khởi tạo dự án") → **Commit to main** → **Push origin**.

## Bước 5 — Chạy build

Vào tab **Actions** trên trang GitHub của repo → chọn workflow **"build-windows"** ở cột trái → bấm **"Run workflow"** → chọn nhánh `main` → Run workflow.

Chờ khoảng 10–20 phút (GitHub Actions cài Rust + Node + build từ đầu nên hơi lâu ở lần đầu). ⚠️ Việc này dùng phút chạy máy ảo (GitHub Actions minutes) trong hạn mức tài khoản anh — nếu là tài khoản Free thì có 2.000 phút/tháng, một lần build kiểu này thường tốn 15-25 phút, không đáng lo nhưng anh nên biết trước.

## Bước 6 — Tải file .exe

Khi build xong (dấu tích xanh ✅), vào tab **Releases** của repo (thường xuất hiện ở cột phải trang chủ repo) → sẽ có 1 bản nháp (Draft) tên "T&T Nhân Số Học App v1.9.0" → mở ra, tải file **`...-setup.exe`** về máy, cài thử để kiểm tra.

Nếu chạy tốt, vào lại bản Release đó bấm "Publish release" để chính thức hoá (bỏ trạng thái Draft).

## Lưu ý quan trọng cho lần build sau (rất hay gặp lỗi nếu quên)

Mỗi lần muốn build lại (kể cả chỉ sửa 1 dòng nội dung app), **phải tăng số phiên bản** ở cả 2 chỗ sau, nếu không GitHub sẽ báo lỗi "already_exists" vì tên bản phát hành bị trùng:

- `src-tauri/tauri.conf.json` → dòng `"version": "1.9.0"`
- `src-tauri/Cargo.toml` → dòng `version = "1.9.0"`
- `package.json` → dòng `"version": "1.9.0"` (không bắt buộc nhưng nên đồng bộ)

Ví dụ lần sau sửa app thì đổi cả 3 chỗ thành `"1.9.1"` hoặc `"1.10.0"` rồi mới đẩy code + chạy lại Actions.

## Muốn có cả bản macOS (.dmg) sau này?

File `.github/workflows/build.yml` hiện tại **chỉ build Windows** (đúng yêu cầu hiện tại của anh). Khi nào cần thêm bản Mac, chỉ cần báo em, em sẽ mở rộng thêm 1 dòng cấu hình (matrix runner `macos-latest`) — không cần làm lại từ đầu.
