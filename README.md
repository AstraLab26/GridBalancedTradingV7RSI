# Grid Balanced Trading EA V7 RSI

## 📋 Mô tả

**Grid Balanced Trading EA V7 RSI** là một Expert Advisor (EA) cho MetaTrader 5, sử dụng chiến lược Grid Trading kết hợp với bộ lọc RSI (Relative Strength Index). EA chỉ khởi động khi RSI đủ điều kiện, lấy giá tại thời điểm đó làm gốc để tạo lưới giao dịch.

### ✨ Tính năng nổi bật

- **Bộ lọc RSI thông minh**: EA chỉ khởi động khi RSI đạt điều kiện (quá mua/quá bán hoặc crossover)
- **Grid Trading tự động**: Tự động tạo và quản lý lưới lệnh theo khoảng cách cố định
- **Cân bằng lưới**: Tự động cân bằng số lượng lệnh Buy/Sell tại mỗi mức
- **Martingale tùy chỉnh**: Hỗ trợ gấp thếp cho từng loại lệnh
- **Quản lý rủi ro**: Nhiều tính năng bảo vệ tài khoản (TP tổng, SL %, Trading Stop)
- **Thông báo điện thoại**: Gửi thông báo khi EA reset với thông tin tích lũy chi tiết

---

## 🚀 Cài đặt

1. Copy file `GridBalancedTradingV7RSI.mq5` vào thư mục `MQL5/Experts/`
2. Mở MetaTrader 5 → Navigator → Experts → Kéo EA vào biểu đồ
3. Cấu hình các tham số theo nhu cầu
4. Bật AutoTrading

---

## ⚙️ Cấu hình tham số

### 📊 Bộ lọc RSI

| Tham số | Mô tả | Mặc định |
|---------|-------|----------|
| **RSIMode** | Chế độ RSI: 0=Tắt, 1=Trên/dưới mức, 2=Cắt lên/xuống, 3=Cả 2 | `RSI_MODE_OFF` |
| **RSIPeriod** | Chu kỳ RSI | `14` |
| **RSIUpperLevel** | Mức RSI trên (quá mua) | `70.0` |
| **RSILowerLevel** | Mức RSI dưới (quá bán) | `30.0` |

**Cách hoạt động:**
- **Chế độ 1 (Trên/dưới mức)**: EA khởi động khi RSI > 70 hoặc RSI < 30
- **Chế độ 2 (Cắt lên/xuống)**: EA khởi động khi RSI cắt từ dưới lên 70 hoặc cắt từ trên xuống 30
- **Chế độ 3 (Cả 2)**: Kết hợp cả 2 chế độ trên

**Lưu ý**: Khi RSI đủ điều kiện, EA sẽ lấy giá hiện tại làm giá cơ sở để tạo lưới.

### 📐 Cài đặt lưới

| Tham số | Mô tả | Mặc định |
|---------|-------|----------|
| **GridDistancePips** | Khoảng cách giữa các mức lưới (pips) | `20.0` |
| **MaxGridLevels** | Số lượng mức lưới tối đa mỗi phía | `10` |
| **AutoRefillOrders** | Tự động bổ sung lệnh khi đạt TP | `true` |

### 💰 Cài đặt lệnh

EA hỗ trợ 4 loại lệnh, mỗi loại có cấu hình riêng:

#### Buy Limit / Sell Limit / Buy Stop / Sell Stop

Mỗi loại lệnh có các tham số:
- **EnableXXX**: Bật/tắt loại lệnh
- **LotSizeXXX**: Khối lượng lệnh ở mức 1
- **TakeProfitPipsXXX**: Take Profit (pips, 0=off)
- **EnableMartingaleXXX**: Bật gấp thếp
- **MartingaleMultiplierXXX**: Hệ số gấp thếp (mức 2=x2, mức 3=x4...)
- **MartingaleStartLevelXXX**: Bắt đầu gấp thếp từ bậc lưới nào

**Ví dụ gấp thếp:**
- Mức 1: 0.01 lot
- Mức 2: 0.02 lot (x2)
- Mức 3: 0.04 lot (x4)
- Mức 4: 0.08 lot (x8)

### 🎯 TP Tổng

| Tham số | Mô tả | Mặc định |
|---------|-------|----------|
| **TotalProfitTPOpen** | TP tổng lệnh đang mở (USD, 0=off) | `0.0` |
| **ActionOnTotalProfitOpen** | Hành động: 0=Dừng EA, 1=Reset EA | `TP_ACTION_RESET_EA` |
| **TotalProfitTPSession** | TP tổng phiên (USD, 0=off) | `0.0` |
| **ActionOnTotalProfitSession** | Hành động: 0=Dừng EA, 1=Reset EA | `TP_ACTION_RESET_EA` |
| **TotalProfitTPAccumulated** | TP tổng tích lũy (USD, 0=off) | `0.0` |

**Tích lũy** = Số dư hiện tại - Số dư ban đầu khi EA khởi động

### 🛡️ Trading Stop, Step Tổng (Gồng lãi)

| Tham số | Mô tả | Mặc định |
|---------|-------|----------|
| **EnableTradingStopStepTotal** | Bật Trading Stop, Step Tổng | `false` |
| **TradingStopStepMode** | Chế độ: 0=Theo lệnh mở, 1=Theo phiên, 2=Cả 2 | `TRADING_STOP_MODE_OPEN` |
| **TradingStopStepTotalProfit** | Lãi tổng lệnh mở để kích hoạt (USD) | `50.0` |
| **TradingStopStepSessionProfit** | Lãi tổng phiên để kích hoạt (USD) | `50.0` |
| **TradingStopStepReturnProfitOpen** | Lãi quay lại để tiếp tục (USD) | `20.0` |
| **TradingStopStepPointA** | Điểm A cách lệnh dương thấp nhất (pips) | `10.0` |
| **TradingStopStepSize** | Step pips để di chuyển SL (pips) | `5.0` |

**Cách hoạt động:**
1. Khi lãi đạt ngưỡng → Đóng lệnh âm, set SL tại điểm A
2. Khi giá di chuyển theo hướng có lợi → Di chuyển SL theo step
3. Nếu giá quay lại chạm SL → Thực hiện hành động (Dừng/Reset EA)

### 📊 SL % so với tài khoản

| Tham số | Mô tả | Mặc định |
|---------|-------|----------|
| **EnableAccountSLPercent** | Bật SL % so với tài khoản | `false` |
| **AccountSLPercent** | % lỗ so với tài khoản để kích hoạt (%) | `10.0` |
| **MaxLotForAccountSL** | Lot lớn nhất để kích hoạt (0=off) | `0.0` |
| **TotalLotForAccountSL** | Tổng lot để kích hoạt (0=off) | `0.0` |

### 🔔 Cài đặt chung

| Tham số | Mô tả | Mặc định |
|---------|-------|----------|
| **MagicNumber** | Magic Number để nhận diện lệnh | `123456` |
| **CommentOrder** | Comment cho lệnh | `"Grid Balanced V7RSI"` |
| **EnableResetNotification** | Bật thông báo về điện thoại khi reset | `false` |

---

## 📱 Thông báo điện thoại

Khi `EnableResetNotification = true`, EA sẽ gửi thông báo về điện thoại qua MT5 Mobile app khi EA reset.

### Ví dụ thông báo:

```
EA RESET
Biểu đồ: EURUSD
Chức năng: TP Tổng Phiên
Số dư: 10.01K$
Tích lũy lần 1: 0.00$ + 10.00$ = 10.00$
Lỗ lớn nhất: -500.00$ / 10.0K$ (5.00%)
Lot: 0.50 / 5.00
```

**Thông tin trong thông báo:**
- Biểu đồ: Cặp tiền đang giao dịch
- Chức năng: Lý do reset (TP Tổng Phiên, TP Tổng Lệnh Mở, v.v.)
- Số dư: Số dư hiện tại
- Tích lũy lần N: Tích lũy trước + Profit lần này = Tích lũy mới
- Lỗ lớn nhất: Số tiền lỗ lớn nhất / Vốn tại thời điểm đó (%)
- Lot: Lot lớn nhất / Tổng lot lớn nhất từng có

**Cách nhận thông báo:**
1. Cài đặt: Tools → Options → Notifications → Bật "Enable notifications"
2. Đăng nhập MT5 Mobile app trên điện thoại (cùng tài khoản)

---

## 🔄 Cách hoạt động

### 1. Khởi động EA

- Nếu **RSI = OFF**: EA đặt lưới ngay với giá hiện tại
- Nếu **RSI = ON**: EA chờ điều kiện RSI

### 2. Khi RSI đủ điều kiện

1. EA lấy giá hiện tại làm giá cơ sở (`basePrice`)
2. Tạo lưới với các mức cách đều nhau
3. Đặt toàn bộ lệnh theo cài đặt
4. Bắt đầu quản lý lệnh

### 3. Quản lý lệnh

- Tự động đặt lệnh tại các mức lưới
- Cân bằng số lượng Buy/Sell tại mỗi mức
- Tự động bổ sung lệnh khi đạt TP (nếu bật AutoRefillOrders)
- Theo dõi và quản lý rủi ro

### 4. Reset EA

Khi đạt điều kiện reset (TP tổng, v.v.):
1. Đóng tất cả lệnh
2. Tính tích lũy mới
3. Reset giá cơ sở tại giá mới
4. Khởi tạo lại lưới
5. Gửi thông báo (nếu bật)

---

## 📈 Ví dụ cấu hình

### Cấu hình cơ bản (RSI OFF)

```
RSIMode = RSI_MODE_OFF
GridDistancePips = 20
MaxGridLevels = 10
EnableBuyLimit = true
EnableSellLimit = true
LotSizeBuyLimit = 0.01
LotSizeSellLimit = 0.01
```

### Cấu hình với RSI Crossover

```
RSIMode = RSI_MODE_CROSSOVER
RSIPeriod = 14
RSIUpperLevel = 70
RSILowerLevel = 30
GridDistancePips = 20
MaxGridLevels = 10
```

### Cấu hình với Martingale

```
EnableBuyLimit = true
LotSizeBuyLimit = 0.01
EnableMartingaleBuyLimit = true
MartingaleMultiplierBuyLimit = 2.0
MartingaleStartLevelBuyLimit = 2
```

→ Mức 1: 0.01, Mức 2: 0.02, Mức 3: 0.04, Mức 4: 0.08...

### Cấu hình với TP Tổng

```
TotalProfitTPSession = 50.0
ActionOnTotalProfitSession = TP_ACTION_RESET_EA
EnableResetNotification = true
```

→ EA sẽ reset khi đạt 50$ lãi phiên và gửi thông báo

---

## ⚠️ Lưu ý quan trọng

1. **Rủi ro**: Grid Trading có thể tạo nhiều lệnh, cần quản lý rủi ro cẩn thận
2. **Vốn**: Đảm bảo đủ vốn để chịu được drawdown khi giá đi ngược
3. **Spread**: Chú ý spread của broker, ảnh hưởng đến hiệu quả
4. **Test**: Luôn test trên demo trước khi dùng thật
5. **RSI**: Khi dùng RSI, EA chỉ khởi động khi đủ điều kiện, cần kiên nhẫn chờ

---

## 🔧 Xử lý sự cố

### EA không đặt lệnh

- Kiểm tra RSI có đủ điều kiện không (nếu bật RSI)
- Kiểm tra AutoTrading đã bật chưa
- Kiểm tra đủ vốn không
- Kiểm tra Magic Number có trùng không

### EA không reset

- Kiểm tra điều kiện TP tổng có đạt không
- Kiểm tra log trong MT5 để xem lý do

### Không nhận được thông báo

- Kiểm tra `EnableResetNotification = true`
- Kiểm tra MT5 đã bật notifications chưa
- Kiểm tra đã đăng nhập MT5 Mobile chưa

---

## 📝 Changelog

### Version 7.0
- ✅ Thêm bộ lọc RSI với 3 chế độ
- ✅ EA chỉ khởi động khi RSI đủ điều kiện
- ✅ Lấy giá tại thời điểm RSI đủ điều kiện làm gốc
- ✅ Thêm thông báo tích lũy với số lần reset
- ✅ Cải thiện tính toán tích lũy (dựa trên số dư)

---

## 📞 Hỗ trợ

Nếu có vấn đề hoặc câu hỏi, vui lòng kiểm tra:
1. Log trong MT5 (View → Toolbox → Experts)
2. Cài đặt tham số có đúng không
3. Điều kiện thị trường có phù hợp không

---

## 📄 License

Copyright © Grid Balanced Trading

---

**Lưu ý**: EA này chỉ dùng cho mục đích giáo dục và nghiên cứu. Giao dịch có rủi ro, cần thận trọng và quản lý vốn hợp lý.
