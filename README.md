# eda-ecommerce-data

Dataset lấy từ [Kaggle](https://www.kaggle.com/datasets/carrie1/ecommerce-data) bao gồm các thông tin sau: InvoiceNo, StockCode, Description, Quantity, InvoiceDate, UnitPrice, CustomerID, Country

Yêu cầu: Phân tích dữ liệu mua hàng của khách hàng đưa ra các đề xuất để tối ưu hóa hoạt động kinh doanh và đề xuất các giải pháp để tăng doanh thu.

___
## **Ý tưởng triển khai**
1. Imports Libraries and Packages, Import Dataset
2. Data Preprocessing
3. Exploratory Data Analysis
4. RFM Analysis

___
## Tổng kết một số ý từ dataset này:

- **Bộ dữ liệu** có khá nhiều vấn đề như:
  - Có khoảng **3% đơn hàng** có 2 sản phẩm giống nhau trong 1 đơn, có thể bị trùng. Cần xem xét xem đây có phải là tính năng của hệ thống hay không.
  - **15% đơn hàng** không có thông tin của khách hàng và **25% số dữ liệu records** không có thông tin khách hàng. Số lượng đơn hàng không có thông tin khách hàng theo từng ngày cũng không đồng đều, có ngày rất nhiều đơn không có khách hàng.
    - Giả thuyết là hệ thống lưu trữ dữ liệu này có vấn đề, có thể do sự cố hệ thống vào những ngày này => cần kiểm tra lại hệ thống.
    - Hoặc là do khách hàng khi thanh toán không cung cấp thông tin nên không lưu trữ được => cần thêm các chương trình khuyến khích như thẻ thành viên, tích điểm giảm giá sản phẩm để khách cung cấp thông tin khi mua hàng.
  - Thuộc tính **tên sản phẩm (Description)** bị thiếu rất nhiều. Có trường hợp một mã sản phẩm ghi nhận đến cả 10 cái tên khác nhau (ví dụ: '20713') => những tên bị trùng này cũng ghi không có quy luật gì cả, khả năng trường thông tin này do người dùng tự nhập => cần kiểm tra lại logic khi thu ngân quét sản phẩm để thanh toán => hệ thống có tự động truy vấn đến thuộc tính sản phẩm để lấy Description hay không => cần sửa lại từ bước này để dữ liệu được đồng bộ.
- **Sau khi phân tích**, có một số nhận xét:
  - **Dữ liệu hoàn toàn thiếu các đơn hàng của ngày thứ 7** trong tuần => trong khi bình thường thì doanh số các ngày cuối tuần thường sẽ là cao điểm nhất. Việc thiếu dữ liệu nếu không được khắc phục sẽ ảnh hưởng nhiều đến chất lượng phân tích.
  - **Thời gian mua hàng chính trong ngày** là vào lúc 12h trưa => có khả năng các sản phẩm này hướng đến đối tượng là dân văn phòng, mua hàng vào lúc nghỉ trưa => cần xem xét mở rộng cửa hàng ở trong những tòa nhà văn phòng lớn.
  - **Hiện có khách hàng từ gần 40 quốc gia đã mua hàng**, tuy nhiên phần lớn tập trung ở khu vực châu Âu. Các quốc gia đang có sự tăng trưởng mạnh mẽ (trừ Anh) là Đức, Pháp, Bỉ => có thể nghiên cứu sâu hơn về các thị trường này để mở rộng quy mô?
  - **Kiểm tra top 20 cặp sản phẩm thường được mua cùng nhau nhất**, có đến 7 cặp có sản phẩm 'JUMBO BAG RED RETROSPOT' => đây có thể là một sản phẩm phễu, có thể đề xuất tạo ra những combo chứa sản phẩm này để tăng doanh số bán các sản phẩm khác.
 
- **Phân tích theo mô hình RFM**:
  - Nhãn 1-1-1 và 3-3-3 chiếm tỷ lệ cao nhất: Nhóm 1-1-1 đại diện cho những khách hàng tốt nhất (tần suất mua hàng cao, giá trị giao dịch lớn và gần đây) trong khi nhóm 3-3-3 có thể là những khách hàng ít hoạt động (mua hàng không thường xuyên, giá trị giao dịch thấp và giao dịch gần đây ít).

  => Doanh nghiệp nên tập trung vào việc duy trì và cải thiện trải nghiệm của các nhóm khách hàng có giá trị cao (nhóm 1-1-1) và tìm cách kích hoạt lại các khách hàng trong nhóm ít tương tác (nhóm 3-3-3).  
  - Ví dụ nhóm 1-1-1: Nhóm khách hàng này có thể sử dụng các chương trình tiếp thị dành riêng => với nhóm này có thể dành tặng họ những phiếu giảm giá khi họ giới thiệu người thân mua hàng, áp dụng cho 1 số mặt hàng mà trước đây họ đã từng mua nhiều lần. Như vậy vừa giữ thể hiện sự quan tâm đến họ mà vừa có thể thu hút thêm những khách hàg mới
  - Nhóm 3-3-3 cũng khá nhiều: cho thấy rất nhiều khách chỉ đến mua 1 lần và rời bỏ. Vì vậy với các khách hàng mới có hệ số là 3-x-x thì cần có ngay chương trình tặng voucher áp dụng cho lần mua thứ 2
