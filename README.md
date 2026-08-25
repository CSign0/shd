# SHD - SỐ HÓA HỒ SƠ ĐẢNG VIÊN V1.0

Ứng dụng Windows desktop **Số hóa hồ sơ đảng viên** — offline-first, WPF/.NET.

## Phiên bản 1.0

- **Sản phẩm:** số hóa hồ sơ đảng viên theo Công văn 1361 — quét/nhập, phân loại catalog 01–104, xuất PDF + `manifest.json`, duyệt và lưu vào thư mục đảng viên đã khóa.
- **Nhãn:** sản phẩm / UI / tag git = **v1.0** (hiển thị **1.0**). `FileVersion` Windows = `1.0.0.0`.
- **Runtime:** `net9.0` (ADR-013); đích dài hạn .NET 10 (`scripts/retarget-net10.ps1`).
- **Hướng dẫn người dùng:** [`docs/ops/USER-GUIDE.md`](docs/ops/USER-GUIDE.md) (snapshot V1.0: [`docs/ops/v1/USER-GUIDE.md`](docs/ops/v1/USER-GUIDE.md)).
- **Changelog:** [`CHANGELOG.md`](CHANGELOG.md).
- **Quy ước ver:** [`docs/ops/VERSIONING.md`](docs/ops/VERSIONING.md).
- **Checklist phát hành:** [`docs/ops/RELEASE-1.0.md`](docs/ops/RELEASE-1.0.md).

### Có trong V1.0

| Nhóm | Nội dung |
|---|---|
| Workflow | Tạo hồ sơ → Bước 1 quét/nhập/cắt-ghép → Bước 2 gán + PDF → Bước 3 duyệt → **Lưu hồ sơ** |
| Công cụ ảnh | Cắt & ghép, Ghép sổ A5, Ghép A3, Ép files, chuyển mục, cấu hình quét WIA |
| Quản trị | Danh mục (xem), Nhật ký, Sao lưu/phục hồi/gom, Health, Cài đặt, DataRoot migrate |
| Vận hành | Windows identity + `role-map.json`, four-eyes, license, Cấu hình ban đầu |
| Ngoài app | Ký số Ban Cơ yếu và CSDL đảng viên **không** nằm trong V1.0 |

### Ngoài phạm vi V1.0

TWAIN (optional sau V1), ký số, đồng bộ CSDL đảng viên.

## Phát hành V1.0

Xem checklist đầy đủ: [`docs/ops/RELEASE-1.0.md`](docs/ops/RELEASE-1.0.md).

Tóm tắt:

1. Đồng bộ nhãn **1.0** (đã ghim trong `Directory.Build.props`).
2. `./scripts/ci-build.ps1` + smoke 1 hồ sơ end-to-end.
3. Commit freeze → tag **`v1.0`**.
4. `publish-win-x64.ps1` → (optional) `sign-win-x64.ps1` → `package-win-x64-zip.ps1`.
5. Ghi SHA-256 ZIP vào `RELEASE-1.0.md`; giao kèm USER-GUIDE + LicenseAdmin.
6. Khóa OQ hoặc điền `PILOT-OQ-WAIVER.md`.

## Phát triển sau V1.0

Xem [`docs/ops/VERSIONING.md`](docs/ops/VERSIONING.md).

- Nhánh **`release/1.0`**: chỉ hotfix **1.0.x**.
- Nhánh **`main`**: sau tag, bump **1.1-dev** (hoặc **1.1**); CHANGELOG chỉ **append** dưới `Unreleased`.
- Snapshot HDSD V1.0 giữ tại `docs/ops/v1/` — không ghi đè khi sửa guide trên `main`.
- Backlog gợi ý: **1.0.x** hotfix pilot → **1.1+** UX/scanner/TWAIN → **2.0** ký số/CSDL.

## Pilot / máy đơn vị

1. Khóa OQ hoặc ký `docs/ops/PILOT-OQ-WAIVER.md` (baseline `docs/ops/OQ-PILOT-BASELINE.md`).
2. `./scripts/publish-win-x64.ps1` rồi `./scripts/install-local.ps1`.
3. Sửa `%LOCALAPPDATA%\PartyMemberDossier\appsettings.local.json` (tên đơn vị, DataRoot, four-eyes).
4. Sửa `%LOCALAPPDATA%\PartyMemberDossier\role-map.json` (tài khoản Windows → Operator/Reviewer/Admin).
5. Máy scan WIA: `docs/ops/SPIKE-01-SCANNER-MATRIX.md` và 30 hồ sơ: `docs/ops/PILOT-30-DOSSIER-PROTOCOL.md`.
6. Hướng dẫn người dùng: `docs/ops/USER-GUIDE.md`.
7. License (ADR-022): cổng `src/PartyMemberDossier.LicenseAdmin` (http://localhost:5088). Máy production cần file `%LOCALAPPDATA%\PartyMemberDossier\license.lic`. Máy dev: `App:LicenseBypass=true` trong Development.

Chạy từ source (máy dev, simulator bật khi debugger gắn):

```powershell
dotnet restore
dotnet build -c Release
dotnet run --project src/PartyMemberDossier.App.Wpf -c Release
```

## Kiểm tra chuẩn

```powershell
./scripts/ci-build.ps1
```

hoặc:

```powershell
dotnet restore
dotnet build -c Release
dotnet test -c Release --no-build
```

## Tài liệu vận hành liên quan

- `docs/ops/` — OQ, pilot, scanner matrix, Authenticode, VirusTotal, RELEASE-1.0, VERSIONING
- `docs/adr/` — quyết định kỹ thuật (WIA, SQLite, PDF, license…)
- `installer/README.md` — publish/cài đặt
- `src/PartyMemberDossier.LicenseAdmin/README.md` — cổng cấp license
