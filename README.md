# Project: Phân tích thông tin chiều sâu của ảnh
## 1. Giới thiệu
- **Stereo Matching** là một bài toán lớn trong lĩnh vực Thị giác máy tính (Computer Vision), với mục tiêu phục hồi kiến trúc 3D thực tế từ một cặp ảnh 2D, gọi là ảnh stereo. Stereo Matching thường được ứng dụng trong các ứng dụng liên quan đến Xe tự hành (Autonomous Driving), Thực tế ảo (Augmented Reality)...
- Project này triển khai một số thuật toán tính Disparity Map từ cặp ảnh stereo cho trước. Với Disparity Map, ta có thể biết được thông tin chiều sâu (Depth Information) thực tế.
- Các cặp ảnh được sử dụng trong project gồm
    
    -  Ảnh Tsukuba: Sử dụng trong Problem 1. [Tải ảnh Tsukuba tại đây](https://drive.google.com/file/d/14gf8bcym_lTcvjZQmg8kwq3aXkENBxMQ/view?usp=sharing)
    - Ảnh Aloe: Sử dụng trong Problem 2, 3, 4. [Tải ảnh Aloe](https://drive.google.com/file/d/1wxmiUdqMciuTOs0ouKEISl8-iTVXdOWn/view?usp=sharing)

## 2. Problem 1:
- Xây dựng hàm tính disparity map của hai ảnh stereo đầu vào (ảnh bên trái(L) và ảnh bên phải (R)) theo phương thức ***pixel-wise matching.***
- Các bước để xây dựng
1. Đọc ảnh chụp bên trái (left) và ảnh chụp bên phải (right)dưới dạng ảnh grayscale đồng thời ép kiểu ảnh về np.float32.
2. Khởi tạo hai biến height, width có giá trị bằng chiều cao,chiều rộng của ảnh trái.
3. Khởi tạo một ma trận không-zeromatrix(depth) với kích thước bằng height,width.
4. Với mỗi pixel tại vị trí(h,w) (duyệt từ trái qua phải, trên xuống dưới)thực hiện các bước sau:
    - (a) Tính cost (L1 hoặc L2) giữa các cặp pixel left[h,w] và right[h,w- d] (trong đó d ∈ [0,disparity_range]). Nếu ***(w-d) < 0*** thì gán giá trị ***cost = max_cost (max_cost = 255 nếu dùng L1 hoặc $255^2$ nếu dùng L2).***
    - (b) Với danh sách cost tính được, chọn giá trị d (doptimal)mà ở đó cho giá trị cost là nhỏ nhất.
    - (c) Gán ***depth[h,w]=doptimal × scale***. Trong đó, $scale = \frac{255}{disparity_range}$ (Ở Problem 01, giá trị scale = 16)

## 3. Problem 2:
1. Đọc ảnh chụp bên trái (left) và ảnh chụp bên phải (right) dưới dạng ảnh grayscale (ảnh mức xám) đồng thời ép kiểu ảnh về np.float32.
2. Khởi tạo hai biến height, width có giá trị bằng chiều cao, chiều rộng của ảnh trái.
3. Khởi tạo một ma trận không- zero matrix (depth) với kích thước bằng height, width.
4. Tính nửa kích thước của window tính từ tâm đến cạnh của window (có kích thước k x k) theo công thức $kernel_half = \frac{k−1}{2}$ (lấy nguyên).
5. Với mỗi pixel tại vị trí (h, w) (h ∈ [kernel_half, height- kernel_half], w ∈ [kernel_half,
 width- kernel_half]; duyệt từ trái qua phải, trên xuống dưới), thực hiện các bước sau

## 4. Problem 3:
- Khi sử dụng hàm tính disparity map đã xây dựng ở Problem 2 cho cặp ảnh
 Aloe_left_1.png và Aloe_right_2.png với tham số đầu vào disparity_range = 64 và kernel_size = 5 ở cả hai hàm cost, kết quả disparity map đã phần nào tệ đi (bị nhiễu). ***(Giải thích ở file depth_image_estimation_problem3.ipynb)***

## 5. Problem 4: 
- Để giải quyết vấn đề của Problem 3 (2 ảnh khác nhau về độ sáng), ta sử dụng cosine_similariry.
