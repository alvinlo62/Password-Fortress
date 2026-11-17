# Password Fortress｜密碼管理網站

網站：[Password Fortress](https://password-fortress.onrender.com)

> 本網站部署於 Render 免費方案，閒置一段時間後會休眠，第一次開啟需等待約數十秒。
> 此為課程專案展示用網站，請勿存放真實密碼。

## 功能介紹
- **帳號系統**：註冊、登入、登出；註冊時檢查使用者名稱與信箱是否重複
- **密碼管理**：依網站名稱與網站帳號儲存密碼，支援新增、檢視、編輯、刪除與一鍵複製；同一網站與帳號的組合不可重複新增
- **兩步驟驗證**：編輯、刪除與複製密碼前，須輸入寄到註冊信箱的 6 位數驗證碼（3 分鐘內有效）
- **密碼強度檢測**：依長度、字元種類計算熵值與多樣性分數，並比對常用密碼清單，給出強度等級與改善建議
- **密碼產生器**：依指定長度（8–16 字元）與數量產生密碼，只保留強度分數 80 以上的結果，可直接存入密碼庫
- **密碼加密器**：以「原始密碼 + 提示詞 + 使用者專屬鹽值」經 SHA-256 雜湊後轉成高強度密碼，同樣輸入永遠得到同一組密碼
- **儀表板**：顯示密碼總數、平均強度分數，並列出強度低於 50 分的弱密碼

## 安全設計
| 資料 | 處理方式 |
|---|---|
| 使用者登入密碼 | PBKDF2-SHA256 雜湊（Werkzeug），不儲存明文 |
| 密碼庫內的網站密碼 | Fernet 對稱式加密（cryptography），金鑰由環境變數提供 |
| 登入狀態 | Flask-Login 管理，session 20 分鐘後過期 |
| 敏感操作 | 編輯、刪除、複製密碼前須完成 Email 兩步驟驗證，且只能操作自己的資料 |

## 使用技術
- **後端**：Python、Flask、Flask-Login、Flask-Mail、Flask-SQLAlchemy
- **資料庫**：SQLite
- **前端**：HTML、CSS、JavaScript（Jinja2 模板）
- **加密與雜湊**：cryptography（Fernet）、Werkzeug（PBKDF2-SHA256）、hashlib（SHA-256）
- **部署**：Render、Gunicorn

## 本機執行

```bash
python -m venv venv
venv\Scripts\activate          # macOS / Linux：source venv/bin/activate
pip install -r requirements.txt
```

在專案根目錄建立 `.env`（**不要 commit 這個檔案**）：

```env
SECRET_KEY=任意的隨機字串
ENCRYPTION_KEY=Fernet 金鑰
MAIL_USERNAME=寄送驗證碼的 Gmail 帳號
MAIL_PASSWORD=該 Gmail 的應用程式密碼
```

產生 Fernet 金鑰：

```bash
python -c "from cryptography.fernet import Fernet; print(Fernet.generate_key().decode())"
```

啟動：

```bash
python app.py
```

資料庫（`instance/users.db`）會在第一次啟動時自動建立。

## 開發人員
- alvinlo62
- LCY000
- gino287
