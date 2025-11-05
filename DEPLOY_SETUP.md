# GitHub Actions - Auto Deploy Setup

## 📋 Cấu hình GitHub Secrets

Vào repository trên GitHub → **Settings** → **Secrets and variables** → **Actions** → **New repository secret**

Thêm các secrets sau:

### 1. `SERVER_HOST`
- **Value**: IP hoặc domain của server LuongOceanDigital
- Ví dụ: `123.45.67.89` hoặc `server.luongocean.com`

### 2. `SERVER_USER`
- **Value**: Username SSH để đăng nhập server
- Ví dụ: `root` hoặc `ubuntu` hoặc `luong`

### 3. `SERVER_SSH_KEY`
- **Value**: Private SSH key để kết nối server
- Lấy từ file `~/.ssh/id_rsa` hoặc key bạn dùng để SSH
```bash
cat ~/.ssh/id_rsa
```
Copy toàn bộ nội dung (bao gồm `-----BEGIN ... KEY-----` và `-----END ... KEY-----`)

### 4. `SERVER_PORT` (Optional)
- **Value**: Port SSH của server (mặc định: `22`)
- Chỉ cần thêm nếu server dùng port khác 22

### 5. `SERVER_REPO_PATH`
- **Value**: Đường dẫn tới thư mục code trên server
- Ví dụ: `/home/luong/WorkspaceOceanDigital` hoặc `/var/www/project`

---

## 🔑 Tạo SSH Key cho GitHub Actions (nếu chưa có)

**Trên server LuongOceanDigital:**

```bash
# Tạo SSH key mới (hoặc dùng key có sẵn)
ssh-keygen -t rsa -b 4096 -C "github-actions@deploy" -f ~/.ssh/github_deploy

# Copy public key vào authorized_keys
cat ~/.ssh/github_deploy.pub >> ~/.ssh/authorized_keys

# Copy private key (paste vào GitHub Secret: SERVER_SSH_KEY)
cat ~/.ssh/github_deploy
```

---

## 🚀 Workflow hoạt động

1. **Push code** từ local → GitHub (dùng `ocean-sync.bat`)
2. **GitHub Actions** tự động trigger
3. **SSH vào server** LuongOceanDigital
4. **Pull code mới nhất** từ GitHub về server
5. **Hoàn tất** - Code đã sync về server với môi trường đầy đủ

---

## 🧪 Test Deployment

Sau khi setup xong secrets, chạy:

```bash
.\ocean-sync.bat
```

Vào GitHub → **Actions** tab → Xem workflow chạy

---

## 📝 Notes

- Server cần có **git** đã cài đặt
- Server cần đã **clone repo** về folder `SERVER_REPO_PATH`
- Đảm bảo server có quyền **pull từ GitHub** (thêm SSH key hoặc dùng HTTPS)

### Setup Git trên server (nếu chưa có):

```bash
# SSH vào server
ssh user@server

# Clone repo (chạy 1 lần đầu)
cd ~
git clone git@github.com:luongtrz/SyncOceanDigital.git WorkspaceOceanDigital

# Hoặc dùng HTTPS
git clone https://github.com/luongtrz/SyncOceanDigital.git WorkspaceOceanDigital
```
