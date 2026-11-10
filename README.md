# AzerNova

🇻🇳 [Tiếng Việt](#vi) · 🇬🇧 [English](#en)

---

<a id="vi"></a>

## 🇻🇳 Tiếng Việt

**AzerNova** là trang web luyện tập trắc nghiệm cho lớp học, viết bằng HTML, CSS và JavaScript thuần trong **một file `index.html`**. Dữ liệu lưu trên Firebase Firestore, đăng miễn phí bằng GitHub Pages.

### Tính năng

- Tài khoản học sinh và quản trị, vào bài bằng mã 6 số.
- Nhập đề từ file Word (`.docx`): trắc nghiệm A–D, đúng/sai a–d, bảng số liệu, đoạn văn dùng chung. Đáp án được nhận từ chữ gạch chân, in đậm hoặc dấu `*`.
- Giới hạn thời gian làm bài, xem kết quả, lịch sử làm bài. Học sinh và quản trị xóa lịch sử độc lập, không ảnh hưởng nhau.
- Giao diện Hologram, màn mở đầu có hiệu ứng, chạy trên cả điện thoại và máy tính.

### Tự triển khai

1. Tải hoặc fork repository này.
2. Tạo dự án Firebase, bật **Firestore Database**, rồi dán luật bên dưới vào tab **Rules** và bấm **Publish**:
   ```
   rules_version = '2';
   service cloud.firestore {
     match /databases/{database}/documents {
       match /accounts/admin { allow read: if true; allow write: if false; }
       match /accounts/{u} { allow read, write: if true; }
       match /sets/{code} { allow read, write: if true; }
       match /results/{id} {
         allow read: if true;
         allow create: if request.resource.data.keys().hasAll(['u','code','score','at'])
                       && request.resource.data.score is number
                       && request.resource.data.score >= 0
                       && request.resource.data.score <= 10;
         allow update: if request.resource.data.diff(resource.data)
                          .affectedKeys().hasOnly(['hideStu','hideAdm']);
         allow delete: if false;
       }
     }
   }
   ```
3. Trong `index.html`, tìm `FIREBASE_CONFIG` và thay bằng cấu hình **của chính bạn**. **Không dùng lại cấu hình có sẵn trong repository này**, nếu không dữ liệu của bạn sẽ ghi vào kho của tác giả.
4. Tạo mật khẩu quản trị riêng: mở trang, bấm F12, vào tab **Console**, đổi `MAT_KHAU_CUA_BAN` rồi chạy đoạn dưới. Dán kết quả `{salt, hash}` vào `ADMIN_SEED` trong `index.html`.
   ```js
   (async()=>{const pw='MAT_KHAU_CUA_BAN',salt=[...crypto.getRandomValues(new Uint8Array(16))].map(x=>x.toString(16).padStart(2,'0')).join('');const k=await crypto.subtle.importKey('raw',new TextEncoder().encode(pw),'PBKDF2',false,['deriveBits']);const b=await crypto.subtle.deriveBits({name:'PBKDF2',salt:new TextEncoder().encode(salt),iterations:100000,hash:'SHA-256'},k,256);console.log(JSON.stringify({salt,hash:[...new Uint8Array(b)].map(x=>x.toString(16).padStart(2,'0')).join('')}))})()
   ```
5. Vào **Settings → Pages**, chọn nhánh `main`, thư mục `/ (root)`.

> ⚠️ Hệ thống đăng nhập chạy trong trình duyệt nên phù hợp **lớp học nội bộ và bài luyện tập**, chưa đủ cho bài kiểm tra tính điểm thật. Muốn bảo mật chặt hơn cần dùng Firebase Authentication.

### 🤝 Tôn trọng ý tưởng và bản quyền

Mã nguồn mở không có nghĩa là "không cần ghi công". Đây là sản phẩm của nhiều công sức sáng tạo, nên mình mong bạn:

- **Giữ nguyên** thông báo bản quyền và file `LICENSE` trong mọi bản sao hoặc phần đáng kể của mã (đây là **điều kiện bắt buộc** của giấy phép).
- **Ghi công tác giả** và dẫn link về repository gốc khi bạn dùng lại, fork hoặc phát triển tiếp, ví dụ trong README hoặc phần giới thiệu của trang.
- **Không nhận là của riêng mình**: không xóa tên tác giả, không đăng lại nguyên bản như sản phẩm của bạn, không bán lại nguyên bản.
- **Ghi rõ nếu có chỉnh sửa**, ví dụ "dựa trên AzerNova của [Tên tác giả]".
- Nếu bạn lấy **ý tưởng, bố cục hoặc hiệu ứng** làm cảm hứng, hãy ghi nguồn. Nếu định dùng cho **mục đích thương mại** (khóa học trả phí, sản phẩm bán), hãy mở một [Issue](../../issues) để trao đổi trước.

Hai điều mình *mong* (ghi công và trao đổi trước khi thương mại hóa) là lời đề nghị lịch sự. Điều kiện **bắt buộc về mặt pháp lý** chỉ là những gì ghi trong file `LICENSE`.

### 📦 Tài nguyên bên thứ ba (không nằm trong giấy phép của dự án)

- **Thiết kế mắt Mangekyō Sharingan** lấy cảm hứng từ *Naruto* (© Masashi Kishimoto / Shueisha) và **âm thanh hiệu ứng** lấy từ nguồn bên ngoài. Quyền thuộc về chủ sở hữu gốc, giấy phép MIT của dự án **không** áp dụng cho các phần này. Nếu đưa trang ra công khai rộng rãi hoặc dùng thương mại, hãy thay bằng tài nguyên của bạn.
- Các ảnh minh họa nhúng trong file thuộc quyền của chủ sở hữu tương ứng.
- Thư viện đang dùng: [Mammoth.js](https://github.com/mwilliamson/mammoth.js) (BSD-2-Clause), [Firebase JS SDK](https://github.com/firebase/firebase-js-sdk) (Apache-2.0), phông chữ Exo 2, Be Vietnam Pro, Share Tech Mono qua Google Fonts (SIL OFL 1.1).

### 📄 Giấy phép

Phát hành theo giấy phép [MIT](LICENSE) (không áp dụng cho các tài nguyên bên thứ ba ở trên). Bản quyền © 2026 **[Tên tác giả]**.

### 💬 Đóng góp và liên hệ

Góp ý, báo lỗi hoặc xin trao đổi: mở một [Issue](../../issues) trên GitHub.

---

<a id="en"></a>

## 🇬🇧 English

**AzerNova** is a classroom practice-quiz website built with plain HTML, CSS and JavaScript in a **single `index.html` file**. Data is stored in Firebase Firestore and it is hosted for free on GitHub Pages.

### Features

- Student and admin accounts, with 6-digit codes to join a quiz.
- Import quizzes from Word (`.docx`): multiple choice A–D, true/false a–d, data tables and shared reading passages. Correct answers are read from underlined text, bold text or a `*` mark.
- Time limits, results and attempt history. Students and admins can delete their own history independently without affecting each other.
- Hologram-style interface with an animated intro screen, working on both phones and desktops.

### Self-hosting

1. Download or fork this repository.
2. Create a Firebase project, enable **Firestore Database**, then paste the rules below into the **Rules** tab and press **Publish**:
   ```
   rules_version = '2';
   service cloud.firestore {
     match /databases/{database}/documents {
       match /accounts/admin { allow read: if true; allow write: if false; }
       match /accounts/{u} { allow read, write: if true; }
       match /sets/{code} { allow read, write: if true; }
       match /results/{id} {
         allow read: if true;
         allow create: if request.resource.data.keys().hasAll(['u','code','score','at'])
                       && request.resource.data.score is number
                       && request.resource.data.score >= 0
                       && request.resource.data.score <= 10;
         allow update: if request.resource.data.diff(resource.data)
                          .affectedKeys().hasOnly(['hideStu','hideAdm']);
         allow delete: if false;
       }
     }
   }
   ```
3. In `index.html`, find `FIREBASE_CONFIG` and replace it with **your own** configuration. **Do not reuse the configuration that ships in this repository**, or your data will be written to the author's database.
4. Create your own admin password: open the page, press F12, go to the **Console** tab, replace `YOUR_PASSWORD` and run the snippet below. Paste the resulting `{salt, hash}` into `ADMIN_SEED` in `index.html`.
   ```js
   (async()=>{const pw='YOUR_PASSWORD',salt=[...crypto.getRandomValues(new Uint8Array(16))].map(x=>x.toString(16).padStart(2,'0')).join('');const k=await crypto.subtle.importKey('raw',new TextEncoder().encode(pw),'PBKDF2',false,['deriveBits']);const b=await crypto.subtle.deriveBits({name:'PBKDF2',salt:new TextEncoder().encode(salt),iterations:100000,hash:'SHA-256'},k,256);console.log(JSON.stringify({salt,hash:[...new Uint8Array(b)].map(x=>x.toString(16).padStart(2,'0')).join('')}))})()
   ```
5. Go to **Settings → Pages**, choose the `main` branch and the `/ (root)` folder.

> ⚠️ Login runs in the browser, so this fits **internal classrooms and practice quizzes**, not high-stakes graded exams. Stronger security would require Firebase Authentication.

### 🤝 Respect for ideas and copyright

Open source does not mean "no credit needed". A lot of creative work went into this project, so I kindly ask you to:

- **Keep** the copyright notice and the `LICENSE` file in every copy or substantial portion of the code (this is a **legal requirement** of the license).
- **Credit the author** and link back to the original repository when you reuse, fork or build on this project, for example in your README or an "About" section of the site.
- **Do not claim it as your own**: do not remove the author's name, do not republish the original as your product, and do not resell it unchanged.
- **State clearly if you made changes**, for example "based on AzerNova by [Author name]".
- If you take **ideas, layout or visual effects** as inspiration, please credit the source. If you plan **commercial use** (paid courses, products for sale), please open an [Issue](../../issues) to talk first.

The two things I *ask* (credit, and talking before commercial use) are polite requests. What is **legally binding** is only what the `LICENSE` file says.

### 📦 Third-party material (not covered by this project's license)

- The **Mangekyō Sharingan eye design** is inspired by *Naruto* (© Masashi Kishimoto / Shueisha) and the **sound effect** comes from an external source. Rights belong to their original owners, and this project's MIT license does **not** apply to them. If you publish widely or use commercially, please replace them with your own assets.
- Illustrative images embedded in the file belong to their respective owners.
- Libraries in use: [Mammoth.js](https://github.com/mwilliamson/mammoth.js) (BSD-2-Clause), [Firebase JS SDK](https://github.com/firebase/firebase-js-sdk) (Apache-2.0), and the fonts Exo 2, Be Vietnam Pro and Share Tech Mono via Google Fonts (SIL OFL 1.1).

### 📄 License

Released under the [MIT License](LICENSE) (which does not extend to the third-party material above). Copyright © 2026 **[Author name]**.

### 💬 Contributing and contact

Feedback, bug reports or questions: please open an [Issue](../../issues) on GitHub.
