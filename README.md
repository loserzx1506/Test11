![image](https://github.com/user-attachments/assets/f5c891be-e0f0-4b11-b87c-c531d9aa211e)
# Helios Testnet Auto Bot

# HELIOS TESTNET AUTO BOT

## Giới thiệu
Đây là một bot tự động dành cho Helios Testnet, được phát triển để thực hiện các tác vụ như cầu nối (bridge), stake, đăng ký tài khoản mới, và nhận token từ faucet. Bot được trang bị tính năng tích hợp Tor để thay đổi IP, giúp mỗi giao dịch hoặc tài khoản mới được xử lý với một địa chỉ IP riêng biệt.

## Tính năng
- **Tự động Bridge HLS:** Cầu nối token HLS giữa các chuỗi testnet được hỗ trợ.
- **Tự động Stake HLS:** Stake token HLS vào các trình xác thực có sẵn.
- **Tự động Đăng ký Tài khoản:** Đăng ký tài khoản Helios mới sử dụng private key và mã mời.
- **Tự động Faucet Tokens:** Nhận token HLS từ faucet cho các tài khoản đã đăng ký.
- **Tích hợp Tor:** Thay đổi địa chỉ IP sau mỗi hành động đăng ký/faucet để tránh bị giới hạn.
- **Giao diện người dùng CLI tương tác (TUI):** Dễ dàng cấu hình và theo dõi hoạt động của bot.
- **Lưu cấu hình:** Lưu và tải cấu hình hoạt động hàng ngày từ `config.json`.

## Cài đặt

### Yêu cầu
- Node.js (phiên bản 18 trở lên)
- Git (tùy chọn, để clone repository)
- Tor (để sử dụng tính năng thay đổi IP)

### Các bước cài đặt

1.  **Clone Repository (Nếu chưa có):**
    ```bash
    https://github.com/MrBips/HeliosChain-Auto-Bot.git
    cd Helios-Testnet-Auto-Bot
    ```
    (Lưu ý: Nếu bạn đã có tệp dự án, bạn có thể bỏ qua bước này và chuyển đến thư mục dự án của mình.)

2.  **Cài đặt phụ thuộc:**
    ```bash
    npm install
    ```

3.  **Cấu hình Tor (Quan trọng):**
    Bot này sử dụng Tor để thay đổi địa chỉ IP. Bạn cần cài đặt và cấu hình Tor Browser hoặc một client Tor độc lập để Tor chạy trên cổng điều khiển (control port) 9051 và cổng SOCKS 9050.

    -   **Cài đặt Tor Browser:** Tải xuống và cài đặt Tor Browser từ trang web chính thức của Tor Project. Sau khi cài đặt, chạy Tor Browser để đảm bảo dịch vụ Tor đang chạy.
    -   **Cấu hình Cổng điều khiển (Control Port):**
        Mở tệp `torrc` của Tor (thường nằm trong thư mục cài đặt Tor Browser, ví dụ: `Browser\TorBrowser\Data\Tor\torrc` trên Windows, hoặc `/etc/tor/torrc` trên Linux).
        Thêm hoặc bỏ ghi chú các dòng sau:
        ```
        ControlPort 9051
        CookieAuthentication 0
        HashedControlPassword 16:C0C46B18D37F2F1139C77561884391C5C7A5989B484218D1BB3D27E6A9
        ```
        Trong file `index.js`, bạn cần đặt biến `TOR_CONTROL_PASSWORD` thành `nchien96`:
        ```javascript
        const TOR_CONTROL_PASSWORD = 'nchien96'; // Set Tor control password here
        ```
        Nếu bạn muốn tạo một mật khẩu khác, bạn có thể tạo `HashedControlPassword` bằng lệnh:
        ```bash
        tor --hash-password "mypassword"
        ```
        Và thay thế chuỗi đã hash trong tệp `torrc` và biến `TOR_CONTROL_PASSWORD` trong `index.js`.

    -   **Kiểm tra Tor:** Đảm bảo rằng Tor đang chạy và có thể truy cập qua cổng 9050 (SOCKS) và 9051 (Control).

    muốn đổi mk tor theo mk mình tự tạo thì Crirl+F tìm nchien96 đổi thành mk của mình

## Chuẩn bị Tệp Private Keys

Bot sử dụng các tệp `.txt` để đọc private key và mã mời. Bạn cần tạo các tệp này trong thư mục gốc của dự án:

1.  **`pk.txt` (Dành cho Bridge và Stake):**
    Tạo một tệp có tên `pk.txt` và đặt mỗi private key trên một dòng. Đây là các private key của các ví bạn muốn thực hiện bridge và stake tự động.
    Ví dụ:
    ```
    0x1234567890abcdef1234567890abcdef12345678934567890abcdef
    0xabcdef1234567890abcdef1234567890ab67890abcdef1234567890
    ```

2.  **`register.txt` (Dành cho Đăng ký tài khoản):**
    Tạo một tệp có tên `register.txt` và đặt mỗi private key trên một dòng. Đây là các private key của các ví bạn muốn đăng ký tài khoản Helios mới. Nếu tệp này trống hoặc không tồn tại, bot sẽ tự động tạo ví mới và lưu vào `ref_pk.txt`.
    Ví dụ:
    ```
    0xaaaabbbbccccddddeeeeffff000011112556666777788889999
    ```

3.  **`ref_code.txt` (Dành cho Mã mời):**
    Tạo một tệp có tên `ref_code.txt` và đặt mỗi mã mời trên một dòng. Bot sẽ sử dụng các mã này để đăng ký tài khoản.
    Ví dụ:
    ```
    YOUR_INVITE_CODE_1
    YOUR_INVITE_CODE_2
    ```
    Nếu bạn không có mã mời, bạn có thể để tệp này trống. Bot sẽ cố gắng đăng ký mà không có mã mời, hoặc bạn có thể tìm kiếm mã mời trên các cộng đồng.

## Cách sử dụng

Để chạy bot, mở terminal trong thư mục dự án và chạy:

```bash
npm start
```

Sau khi chạy, giao diện CLI (TUI) sẽ xuất hiện. Bạn có thể điều hướng bằng các phím mũi tên và Enter.

### Menu chính

-   **Start Auto Daily Activity:** Bắt đầu chu trình bridge và stake tự động cho tất cả các tài khoản trong `pk.txt`.
-   **Stop Activity:** Dừng các hoạt động tự động đang chạy.
-   **Set Manual Config:** Chuyển đến menu cấu hình các thông số hoạt động hàng ngày.
-   **Clear Logs:** Xóa lịch sử giao dịch hiển thị trên giao diện.
-   **Refresh:** Cập nhật thông tin ví.
-   **Register an account:** Bắt đầu quá trình đăng ký tài khoản mới hoặc xử lý các private key trong `register.txt`. Nếu `register.txt` trống, bot sẽ tự động tạo ví và đăng ký.
-   **Faucet Tokens:** Yêu cầu token từ faucet cho tất cả các tài khoản trong `pk.txt`.
-   **Exit:** Thoát ứng dụng.

### Menu cấu hình thủ công (Set Manual Config)

-   **Set Bridge Repetitions:** Đặt số lần lặp lại hành động bridge cho mỗi tài khoản mỗi ngày.
-   **Set HLS Range For Bridge:** Đặt phạm vi HLS tối thiểu và tối đa cho mỗi giao dịch bridge.
-   **Set Stake Repetitions:** Đặt số lần lặp lại hành động stake cho mỗi tài khoản mỗi ngày.
-   **Set HLS Range For Stake:** Đặt phạm vi HLS tối thiểu và tối đa cho mỗi giao dịch stake.
-   **Back to Main Menu:** Quay lại menu chính.

### Log giao dịch

-   Khu vực "Transaction Logs" sẽ hiển thị các thông báo về trạng thái hoạt động, lỗi, và các giao dịch.

## Khắc phục sự cố

-   **`Failed to initialize provider: ...` hoặc `Network chain ID mismatch`:** Đảm bảo rằng `RPC_URL` và `CHAIN_ID` trong `index.js` là chính xác và mạng Helios Testnet đang hoạt động.
-   **`Failed to load private keys: No valid private key found in pk.txt`:** Đảm bảo tệp `pk.txt` tồn tại và chứa các private key hợp lệ (có tiền tố `0x` hoặc không, độ dài 64 ký tự hex).
-   **`Faucet claim failed! Reason: ...` hoặc `Registration failed: ...` (liên quan đến Tor hoặc mạng):**
    -   Kiểm tra cấu hình Tor của bạn (cổng 9050 và 9051).
    -   Đảm bảo rằng dịch vụ Tor đang chạy.
    -   Thử khởi động lại Tor Browser hoặc máy tính của bạn.
    -   Đôi khi Tor cần thời gian để thiết lập kết nối mới. Hãy kiên nhẫn.
    -   Kiểm tra kết nối internet của bạn.
-   **Lỗi "Uncaught Exception":** Ghi lại thông báo lỗi và stack trace, sau đó kiểm tra lại mã nguồn hoặc báo cáo lỗi.
-   **"Invalid invite code":** Đảm bảo mã mời trong `ref_code.txt` là hợp lệ và chưa được sử dụng hết.

## Ghi chú khác

-   Luôn sử dụng trên môi trường Testnet. Không sử dụng private key của ví chính.
-   Các giá trị `gasLimit` được đặt sẵn trong mã. Nếu mạng Helios thay đổi yêu cầu gas, bạn có thể cần điều chỉnh các giá trị này.
-   Thực hiện các thay đổi cấu hình qua giao diện bot hoặc chỉnh sửa trực tiếp `config.json` (sau khi dừng bot). 
