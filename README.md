# CẢI THIỆN HIỆU SUẤT CỦA THUẬT TOÁN GOM NHÓM DỮ LIỆU KHÔNG GIAN NS-DBSCAN

> Bài tập lớn môn học Hệ thống thông tin địa lý (GIS) - Khoa Công nghệ Thông tin, Trường Đại học Nha Trang.

## 📋 Thông tin chung

* **Giảng viên hướng dẫn:** ThS. Nguyễn Thủy Đoan Trang
* **Lớp học phần:** 65.CNTT-3
* **Thực hiện bởi nhóm sinh viên:**
    1.  **Lê Văn Hòa** - MSSV: 65131052
    2.  **Nguyễn Gia Khánh** - MSSV: 65131461
    3.  **Nguyễn Thành Đạt** - MSSV: 65139021
* **Thời gian thực hiện:** Tháng 01/2026

---

## 📖 Giới thiệu đề tài

Trong phân tích dữ liệu không gian đô thị, các thuật toán phân cụm truyền thống (như DBSCAN) thường sử dụng khoảng cách Euclid (đường chim bay). Tuy nhiên, phương pháp này bỏ qua các ràng buộc vật lý thực tế như sông ngòi, tòa nhà hay mạng lưới giao thông, dẫn đến sai lệch về mặt ngữ nghĩa.

Thuật toán **NS-DBSCAN** (Network Space DBSCAN) ra đời để giải quyết vấn đề này bằng cách sử dụng khoảng cách đường đi ngắn nhất trên đồ thị. Tuy nhiên, nhược điểm lớn nhất của NS-DBSCAN là **chi phí tính toán rất cao**, gây khó khăn khi xử lý dữ liệu lớn.

**Mục tiêu của dự án:** Nghiên cứu và đề xuất thuật toán cải tiến **iNS-DBSCAN** nhằm tối ưu hóa hiệu năng, giảm thời gian thực thi mà vẫn đảm bảo độ chính xác của kết quả phân cụm.

---

## 🚀 Các giải pháp cải tiến (Proposed Improvements)

Dự án đề xuất 3 chiến lược tối ưu hóa chính dựa trên thuật toán gốc:

### 1. Chiến lược lược bỏ cạnh (Edge Pruning)
* **Vấn đề:** Thuật toán gốc kiểm tra tất cả các cạnh kề, kể cả những cạnh rất dài không thể thuộc vùng lân cận.
* **Giải pháp:** Áp dụng ràng buộc độ dài dựa trên định lý: *"Một đường đi hợp lệ trong lân cận Eps không thể chứa cạnh đơn lẻ có trọng số lớn hơn Eps"*.
* **Thực hiện:** Tiền xử lý loại bỏ các cạnh có $W(e) > Eps$ và ngắt sớm quá trình mở rộng nếu khoảng cách tích lũy vượt quá giới hạn.

### 2. Tối ưu hóa bảng trật tự mật độ (Density Ordering Optimization)
* **Vấn đề:** Việc sắp xếp lại hàng đợi (re-sorting) liên tục và lưu trữ các điểm dữ liệu thưa thớt (rác) gây lãng phí tài nguyên.
* **Giải pháp:**
    * Sử dụng cơ chế **Chèn bảo toàn** (giữ thứ tự giảm dần khi chèn thay vì sort lại).
    * Áp dụng ngưỡng Heuristic $\delta \approx \ln(n)$ để lọc bỏ các điểm mật độ quá thấp ngay từ đầu.

### 3. Tích hợp xác định nhiễu ngầm định (Implicit Noise Identification)
* **Vấn đề:** Thuật toán gốc tốn thời gian kiểm tra từng điểm xem có phải là Nhiễu (Noise) hay không trong các vòng lặp.
* **Giải pháp:** Định nghĩa Nhiễu theo phương pháp loại trừ. Chỉ tập trung tìm cụm, tất cả các điểm còn lại sau khi thuật toán kết thúc mặc định là Nhiễu.

---

## 📊 Kết quả thực nghiệm

Nhóm đã tiến hành thực nghiệm trên dữ liệu thực tế từ **OpenStreetMap (OSM)** với các tham số đầu vào khác nhau.

* **Chỉ số đánh giá:** Thời gian thực thi (Time) và Mức độ chênh lệch hiệu năng (Diff %).
* **Kết quả nổi bật:**
    * Thuật toán cải tiến (iNS-DBSCAN) chạy nhanh hơn thuật toán gốc trong đa số các kịch bản.
    * Hiệu quả rõ rệt nhất ở các tác vụ nặng (Eps lớn, MinPts nhỏ), với mức cải thiện hiệu suất lên đến **~19-20%**.
    * Khả năng chịu tải (Scalability) tốt hơn khi quy mô bài toán tăng lên.

---

## 🛠️ Công nghệ sử dụng

* **Ngôn ngữ lập trình:** Python (hoặc ngôn ngữ bạn dùng, dựa trên tài liệu tham khảo [1] trong báo cáo).
* **Dữ liệu:** OpenStreetMap (OSM).
* **Thuật toán:** LSPD, NS-DBSCAN (Custom implementation).

---

## 📚 Tài liệu tham khảo chính

1.  Tianfu Wang et al., *NS-DBSCAN: A Density-Based Clustering Algorithm in Network Space*, 2019.
2.  Trang T.D.Nguyen et al., *A method for efficient clustering of spatial data in network space*.

---

## FILE CHÍNH NẰM TRONG THƯ MỤC FINAL, NHỮNG CÁI CÒN LẠI LÀ KẾT QUẢ CỦA NHỮNG LẦN THỬ NGHIỆM THUẬT TOÁN !!!
