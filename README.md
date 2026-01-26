# 🏠 Dự đoán Giá Nhà tại TP. Hồ Chí Minh Sử Dụng Machine Learning (2026)

**Project:** House Price Prediction in Ho Chi Minh City  
**Năm thực hiện:** 2026  
**Lĩnh vực:** Machine Learning • Real Estate Analytics • Regression  

## 1. Giới thiệu

Dự đoán giá nhà là bài toán quan trọng trong lĩnh vực bất động sản, hỗ trợ định giá tài sản, đầu tư và phân tích thị trường. Tại TP. Hồ Chí Minh – đô thị phát triển nhanh nhất Việt Nam – giá nhà chịu ảnh hưởng từ vị trí, diện tích, hạ tầng, tiện ích và xu hướng kinh tế.

Dự án xây dựng hệ thống dự đoán giá nhà dân dụng năm 2026 dựa trên dữ liệu listing thực tế từ **Rever.vn**, sử dụng machine learning để mang lại kết quả khách quan, chính xác hơn định giá thủ công.

## 2. Mục tiêu

- Thu thập và xây dựng dataset chất lượng cao từ nguồn thực tế.
- Tiền xử lý dữ liệu, xử lý outliers, missing values và feature engineering.
- Xây dựng, huấn luyện và so sánh Linear Regression (baseline) vs XGBoost Regressor.
- Đánh giá hiệu suất bằng các metrics hồi quy chuẩn (MAE, RMSE, R², MAPE).
- Cung cấp nền tảng hỗ trợ định giá tự động và ra quyết định đầu tư.

## 3. Mô hình sử dụng

### 3.1 Linear Regression (Baseline)
Mô hình tuyến tính đơn giản, dễ giải thích, dùng để tham chiếu.

### 3.2 XGBoost Regressor (Mô hình chính)
Gradient Boosting Decision Trees với regularization mạnh, xử lý tốt phi tuyến tính, tương tác feature và dữ liệu noisy – phù hợp với bài toán giá nhà thực tế.

## 4. Bộ dữ liệu

- **Nguồn:** Tự thu thập (crawl) từ https://rever.vn/s/ho-chi-minh.  
- **Kích thước ban đầu:** ~9,481 bản ghi (nhà phố, căn hộ, biệt thự tại TP.HCM).  
- **Sau xử lý outliers (log-price IQR):** Loại bỏ 660 rows (8.09%), giữ ~8,821 bản ghi sạch.  
- **Tiền xử lý:** log-transform giá (giảm skewness), xử lý missing, one-hot/target encoding cho categorical, scaling numerical, outlier filter trên log-price.

### 4.1 Mô tả chi tiết các cột (Features)

| Cột              | Tên tiếng Việt / Ý nghĩa                              | Kiểu dữ liệu | Mô tả chi tiết                                                                 |
|------------------|-------------------------------------------------------|--------------|--------------------------------------------------------------------------------|
| street           | Đường / Tên đường                                     | String       | Tên đường nơi bất động sản nằm (ví dụ: "Nguyễn Văn Trỗi", "Lê Văn Sỹ").       |
| ward             | Phường / Xã                                           | String       | Tên phường/xã (ví dụ: "Phường 8", "Phường 15", "Thảo Điền").                   |
| district         | Quận / Huyện                                          | String       | Tên quận/huyện tại TP.HCM (ví dụ: "Quận 1", "Quận 7", "Bình Thạnh", "Thủ Đức").|
| code_housing     | Mã nhà / Mã listing                                   | String       | Mã định danh duy nhất của listing trên Rever.vn (thường dùng để trace nguồn).  |
| date_update      | Ngày cập nhật listing                                 | Date/String  | Ngày listing được cập nhật lần cuối (ví dụ: "2025-12-15").                     |
| price            | Giá bán                                               | Float        | Giá bán tổng (tỷ VND), là target chính của mô hình.                            |
| price(m2)        | Giá/m²                                                | Float        | Giá trên mỗi mét vuông (tỷ VND/m²), tính từ price / area.                     |
| bedroom          | Số phòng ngủ                                          | Integer      | Số lượng phòng ngủ (ví dụ: 2, 3, 4). Có thể là 0 nếu là studio/căn hộ nhỏ.     |
| bathroom         | Số phòng tắm                                          | Integer      | Số lượng phòng tắm/vệ sinh (thường bằng hoặc ít hơn bedroom).                  |
| area(m2)         | Diện tích sử dụng                                     | Float        | Diện tích sàn sử dụng (m²), là một trong những feature quan trọng nhất.        |
| direction        | Hướng nhà                                             | String       | Hướng nhà chính (ví dụ: "Đông", "Tây", "Nam", "Bắc", "Đông Nam",...).         |
| discount         | Giảm giá / Khuyến mãi                                 | Float/String | Mức giảm giá (nếu có, đơn vị tỷ VND hoặc %). Có thể là 0 nếu không giảm.       |
| description      | Mô tả chi tiết listing                                | String       | Văn bản mô tả đầy đủ từ Rever (có thể dùng NLP để extract thêm feature như: nội thất, pháp lý, tiện ích gần kề...). |

> **Lưu ý:** Dữ liệu chỉ dùng cho nghiên cứu/học tập, không public raw data để tuân thủ chính sách nguồn.

## 5. Quy trình thực hiện

1. Crawl dữ liệu → 2. EDA & Cleaning → 3. Feature Engineering → 4. Train/Test split (80/20) → 5. Huấn luyện (Linear + XGBoost) → 6. Đánh giá & so sánh.

## 6. Kết quả đánh giá mô hình

### 6.1 Linear Regression (Baseline)

- MAE   : 2.7775 tỷ VND  
- RMSE  : 4.940 tỷ VND  
- R²    : 0.503 (50.3%)

### 6.2 XGBoost Regressor (sau outlier filter + log-transform)
 
- Metrics (trên tập test, inverse transform nếu cần):

  - MAE   : 1.011042 tỷ VND  
  - MSE   : 2.112387  
  - RMSE  : 1.453405 tỷ VND  
  - R²    : 0.8033 (80.33%)  
  - MAPE  : 17.0%

### 6.3 Bảng so sánh

| Metric       | Linear Regression | XGBoost     | Cải thiện                  |
|--------------|-------------------|-------------|-----------------------------|
| MAE (tỷ VND) | 2.7775            | 1.0110      | Giảm ~63.6%                 |
| RMSE (tỷ VND)| 4.940             | 1.4534      | Giảm ~70.6%                 |
| R²           | 0.503             | 0.8033      | Tăng ~59.7% (từ 50.3% → 80.3%) |
| MAPE         | –                 | 17.0%       | –                           |

### 6.4 Nhận xét tổng quan

- **XGBoost vượt trội rõ rệt** so với Linear Regression:  
  - R² tăng mạnh từ 50.3% → **80.3%** → mô hình giải thích hơn 80% biến thiên giá nhà (mức rất tốt cho bất động sản thực tế, noisy và phi tuyến).  
  - RMSE giảm ~70.6% (từ 4.94 → 1.45 tỷ VND) → sai số trung bình thấp hơn đáng kể, dự đoán sát giá thực tế hơn (giá nhà TP.HCM thường vài đến hàng chục tỷ).  
  - MAE giảm hơn 63% → sai số tuyệt đối trung bình chỉ ~1 tỷ VND (chấp nhận được cho định giá tham khảo).  
  - MAPE 17% là mức **tốt** (thường 10–25% trong các nghiên cứu giá nhà; dưới 20% coi là đáng tin cậy cho ứng dụng thực tế).

- **Lý do XGBoost hiệu quả hơn**: Xử lý tốt phi tuyến, tương tác feature (quận, diện tích, loại hình...), outlier filter + log-transform giúp ổn định phân phối.

- **Kết luận**: Với R² ~80% và MAPE 17%, mô hình XGBoost đạt hiệu suất **cao**, đủ dùng cho:  
  - Định giá tự động tham khảo  
  - Hỗ trợ nhà đầu tư sàng lọc  
  - Phân tích xu hướng quận/phường

## 7. Công nghệ

- Python 3.10+  
- Pandas, NumPy, Scikit-learn, XGBoost  
- Matplotlib, Seaborn (visualization)  
- Requests/BeautifulSoup (crawl dữ liệu)

## 8. Ứng dụng thực tế

- Công cụ định giá nhanh cho người mua/bán.  
- Hỗ trợ đầu tư: phân tích tiềm năng theo quận.  
- Dự báo xu hướng thị trường bất động sản TP.HCM.

## 9. Hướng phát triển

- Thêm geospatial features (khoảng cách metro, trường học, trung tâm).  
- Time-series analysis (nếu crawl nhiều thời điểm).  
- Kết hợp nguồn khác (Batdongsan, bảng giá đất chính thức).  
- Triển khai API/web app (FastAPI + Streamlit).
