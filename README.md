<!-- Title -->
<h1 align="center"><b>KEYPOINTS DETECTORS & DESCRIPTOR METHODS</b></h1>
<h1 align="center"><b>~~~~~~~</b></h1>

<!-- Title -->
<h1 align="center"><b>A/ Keypoints Detectors Only</b></h1>

## 1. HARRIS 

Harris là một thuật toán phát hiện góc sử dụng trong Thị giác máy tính được giới thiệu lần đầu vào 1988. Kể từ đó, nó đã được cải tiến và áp dụng trong nhiều thuật toán để tiền xử lý hình ảnh cho các ứng dụng tiếp theo. Cùng tìm hiểu xem nó hoạt động như thế nào ?

Hình bên trái là ảnh gốc, giữa là ảnh biểu thị cường độ Gradient theo chiều x, tiếp theo là biểu thị cường độ Gradient theo chiều y, cuối cùng là biểu thị cường độ Gradient theo cả chiều x và y.

![image](https://user-images.githubusercontent.com/81065789/148052662-081da42b-0686-48bd-9072-3db08f1df2a7.png)

Để xác định được một điểm là Corner (góc), chúng ta sẽ lấy tổng các giá trị Gradient theo chiều x và theo chiều y trong vùng đó:

![image](https://user-images.githubusercontent.com/81065789/148052875-50da8d89-8968-4e6e-b222-de27cdbb6fc9.png)

Nếu cả 2 giá trị này đều đủ lớn, thì chúng ta có thể định nghĩa vùng đó là một góc. Quá trình này được thực hiện cho mọi pixel của hình ảnh. Phương phá này hoạt động vì các vùng được gọi là góc sẽ có số lượng lớn Gradient theo chiều ngang và Gradient theo chiều dọc.

Để mở rộng phương thức này sang các góc tùy ý, trước tiên chúng ta cần tính toán biểu diễn cường độ Gradient của hình ảnh, và sau đó sử dụng cường độ Gradient này để xây dựng lên một ma trận M:

![image](https://user-images.githubusercontent.com/81065789/148053166-04e534d7-f447-4f39-899e-b14ebd7bffc7.png)

Sau khi ma trận M được định nghĩa, chúng ta sẽ sử dụng công thức sau để tính giá trị điểm số biểu thị mức độ của các góc.

![image](https://user-images.githubusercontent.com/81065789/148053429-2dcdf575-42b3-4160-8b9f-7f5659e8badf.png)

Sau đó, chúng ta có thể xem xét điều kiện sau:

      - Nếu |R| nhỏ thì đây là một khu vực phẳng, có nghĩa là không phải keypoint

      - Nếu R < 0 thì đây là một cạnh, và đây cũng không phải keypoint

      - Nếu |R| lớn thì đây chính là một keypoint
      
Với Harris, ngay cả khi hình ảnh được xoay, chúng ta vẫn có thể tìm thấy các góc giống nhau. Đó là điều hiển nhiên vì các góc trong hình ảnh xoay cũng vẫn là các góc trong hình ảnh ban đầu, hai hình sau minh họa cho điều này:

![1](https://user-images.githubusercontent.com/81065789/148065886-43e17743-85bb-4073-adf7-b7a7b75e25e7.jpg)
![2](https://user-images.githubusercontent.com/81065789/148065896-b450c892-39ea-40b7-aa8b-8c0f9efecfd1.jpg)

## 2. FAST

FAST keypoints detector được giới thiệu đầu tiên vào 2006, được sử dụng để phát hiện các góc trong hình ảnh, được triển khai trong thư viện OpenCV và được áp dụng nhiều nhất cho các ứng dụng thời gian thực hoặc các thiết bị bị hạn chế tài nguyên, nơi không có nhiều thời gian tính toán hoặc sức mạnh để sử dụng các kĩ thuật dò tìm keypoints tiên tiến hơn. 

Cụ thể: FAST không tính và lưu trữ ma trận descriptor cho hình ảnh như thuật toán keypoints detector trong SIFT và SURF, nên không tốn dung lượng lưu trữ ma trận này, phù hợp cho bộ nhớ của các thiết bị di dộng.

Ý tưởng của FAST là để một pixel được coi là một góc phải có _**ít nhất n**_ các pixel liền kề dọc theo chu vi hình tròn có bán kính r, _**tất cả**_ đều sáng hơn hoặc tối hơn pixel trung tâm bởi một ngưỡng t nào đó.

Hãy cùng xem một ví dụ:

![3](https://user-images.githubusercontent.com/81065789/148164855-90fbdaba-9e20-4b7a-89bc-851a779ed2bb.png)

Ở đây, chúng ta muốn xem xét liệu pixel trung tâm có nên được coi là một điểm then chốt hay không? Ví dụ: pixel trung tâm = 32, để pixel này được coi là một keypoint, nó phải có n = 12 pixel liền kề dọc theo đường biên của vòng tròn có giá trị đều lớn hơn 32 + t hoặc nhỏ hơn 32 - t. Giả sử rằng t = 16 cho ví dụ này.

Như chúng ta có thể thấy, chỉ có 8 pixel liền kề tối hơn (được đánh dấu bằng hình chữ nhật màu xanh lá cây, tất cả các pixel khác là hình chữ nhật màu đỏ) so với pixel trung tâm - do đó, pixel trung tâm không phải là keypoint.

Một trường hợp khác:

![4](https://user-images.githubusercontent.com/81065789/148164868-c0765261-0227-40cd-af8a-6d43c9330469.png)

Chúng ta thấy có 14 pixel liền kề nhau và sáng hơn pixel trung tâm. Vì thế, pixel trung tâm này được coi là 1 keypoint.

Chúng ta có thể thấy được là thuật toán bên trên tuy rất đơn giản, nhưng lại được sử dụng rất nhiều trong các ứng dụng Computer Vision hằng ngày cũng như các ứng dụng real-time, đòi hỏi tốc độ tính toán cao. Nhược điểm là nó không hiệu quả với mức độ nhiễu cao và có phụ thuộc vào một giá trị ngưỡng.

Hình ảnh qua thuật toán FAST cho kết quả như sau:

![FAST_nonmaxSuppression](https://user-images.githubusercontent.com/81065789/148171386-21dac5d3-80d4-4b27-9957-63e4de482c0f.jpg)

## 3. STAR

Star Feature Detector có nguồn gốc từ máy dò CenSurE. Trong khi CenSurE sử dụng các đa giác (như hình vuông, hình lục giác và hình bát giác) như một sự thay thế dễ tính toán hơn cho hình tròn, thì Star sử dụng hình tròn tạo bởi 2 hình vuông chồng lên nhau: 1 thẳng đứng và 1 xoay 45 độ. Các đa giác này có thể được xem như là đa giác với đường viền dày. 

![STAR](https://user-images.githubusercontent.com/81065789/152470088-6c5517ee-3985-4ad0-ac74-c176c9b8fc3d.jpg)


<!-- Title -->
<h1 align="center"><b></b></h1>
<h1 align="center"><b>B/ Keypoints Detectors & Descriptors</b></h1>

# I. Floating-point Descriptors:

## 1. SIFT (NON-FREE trước kia, nhưng available ở 2021)

Với Harris Keypoints Detector, hình ảnh xoay cho ra kết quả tập keypoints y hệt ảnh gốc ban đầu. Nhưng còn đối với phép Scaling thì sao? Ở tỉ lệ này có 1 góc nhưng ở tỷ lệ khác có còn là 1 góc nữa hay không? 

Ví dụ, hãy kiểm tra một hình ảnh đơn giản bên dưới:

![image](https://user-images.githubusercontent.com/81065789/148063926-e5373a0f-92ec-49f0-877e-c70016563d80.png)

Ảnh ban đầu cho ta một góc, nhưng ở tỉ lệ lớn hơn, nó lại có nhiều hơn là một góc. Đó là lí do SIFT Keypoints Detector ra đời (1999)!

Với SIFT, 2 hình với 2 tỉ lệ khác nhau sẽ cho ra 1 tập keypoints khác nhau:

917 keypoints: ![SIFTbig](https://user-images.githubusercontent.com/81065789/148076055-997693f2-80da-4c3d-9220-e764f42b8de8.jpg)

263 keypoints: ![SIFTsmall](https://user-images.githubusercontent.com/81065789/148076069-2b8f9c13-8924-404a-a8a8-c0077d65c433.jpg)

Chúng ta dùng SIFT để phát hiện và mô tả keypoints nhưng tốc độ của nó tương đối chậm vì sử dụng vector **128 chiều** cho bộ mô tả, người ta cần một phiên bản nâng cấp hơn, vì thế, vào năm 2006, SURF ra đời, cải thiện tốc độ cho SIFT.

## 2. SURF (NON-FREE ở 2021)

SURF bổ sung rất nhiều tính năng để cải thiện tốc độ trong từng bước thực hiện, do đó nó nhanh hơn SIFT. Bộ mô tả của SURF sử dụng 1 vector **64 chiều** thay vì 128 như SIFT. Phân tích cho thấy nó nhanh hơn _3 lần_ so với SIFT trong khi hiệu suất tương đương với SIFT. SURF xử lý tốt hình ảnh bị mờ và xoay, nhưng không tốt trong việc xử lý thay đổi điểm nhìn và thay đổi độ sáng.

Vì nó không miễn phí trong OpenCV tại thời điểm 2021 nên tôi không thể đánh giá hiệu suất và trình bày kết quả ở đây được. :(

# II. Binary Descriptors:

## 1. BRIEF

Như chúng ta đã biết, SIFT sử dụng số dấu phẩy động nên cần sử dụng vector 128 chiều cho các bộ mô tả, tương đương 512 bytes. Tương tự, SURF cũng chiếm tối thiểu 256 bytes (cho vector 64 chiều). Việc tạo một vecto như vậy cho hàng nghìn đặc trưng sẽ tốn rất nhiều bộ nhớ, điều này không khả thi đối với các ứng dụng hạn chế tài nguyên, đặc biệt là đối với các hệ thống nhúng. Bộ nhớ càng lớn thì thời gian matching càng lâu.

Chúng ta có thể sử dụng các phương pháp giảm chiều dữ liệu (PCA, LDA,...) hay các phương pháp băm LSH để chuyển đổi bộ mô tả SIFT từ dạng dấu phẩy động sang dạng chuỗi nhị phân. Các chuỗi nhị phân này được sử dụng để so khớp các đặc trưng bằng cách sử dụng khoảng cách Hamming, điều này giúp tăng tốc độ tốt hơn. Nhưng với cách này, trước tiên chúng ta cần tìm các bộ mô tả, sau đó áp dụng các phương pháp băm, và điều này không giải quyết được vấn đề ban đầu của chúng ta về bộ nhớ.

BRIEF đi vào hình ảnh ngay và luôn. Nó cung cấp một lối tắt để tìm các chuỗi nhị phân trực tiếp mà không cần tìm bộ mô tả.

      Bước 1: chọn ra các pixel trong khu vực xung quanh keypoint;

      Bước 2: các pixel chọn được từ bước #1 sẽ được ghép thành N cặp để so sánh với nhau;

      Bước 3: bước này nhằm đảm bảo rằng "Vector đặc trưng nhị phân" không bị phụ thuộc vào góc quay của đối tượng trong bản vá (rotation invariance);

      Bước 4: với N cặp pixel có được ở bước #2, chúng ta lần lượt so sánh cường độ của hai pixel trong từng cặp (hình 1), từ đó xây dựng vector đặc trưng N-dims (hình 2).

Hình 1: ![image](https://user-images.githubusercontent.com/81065789/152501837-5be3ab16-b32a-41dd-8a04-4a184e4a80f4.png)

Hình 2: ![Screenshot 2022-02-04 160749](https://user-images.githubusercontent.com/81065789/152502033-0be08f39-ba7c-4b23-ab07-d61e8f3271bc.png)

Việc so sánh giữa hai "Vector đặc trưng nhị phân" cũng rất đơn giản, chúng ta sẽ sử dụng Hamming Distance để tính toán sự khác biệt giữa hai vector (hình 3).

Hình 3: ![image](https://user-images.githubusercontent.com/81065789/152502129-7caf737f-76b2-4c77-942b-ead8f3455a48.png)

Một điểm quan trọng là BRIEF là một bộ mô tả đặc trưng, nó không cung cấp bất kỳ phương pháp nào để tìm các đặc trưng. Vì vậy, chúng ta sẽ phải sử dụng bất kỳ công cụ dò tính năng nào khác như SIFT, SURF, v.v. Các bài báo khuyến nghị sử dụng CenSurE là một công cụ dò nhanh và BRIEF hoạt động tốt hơn một chút với CenSurE so với các cách khác.

Kết quả của BRIEF được hiển thị dưới đây:

235 keypoints: ![BRIEF](https://user-images.githubusercontent.com/81065789/152488012-3b231184-dc62-43eb-867f-24ebb9ba3681.jpg)

Và không phải cái gì nhanh thì đều tốt :). BRIEF không cho độ chính xác cao bằng SIFT và SURF. Cần cân nhắc khi lựa chọn bộ mô tả đặc trưng phù hợp cho bối cảnh bài toán.

## 2. ORB:

Thuật toán này được đưa ra bởi **Ethan Rublee, Vincent Rabaud, Kurt Konolige và Gary R. Bradski** trong bài báo của họ: "ORB: Một giải pháp thay thế hiệu quả cho SIFT hoặc SURF" vào năm 2011. Như tiêu đề đã nói, nó là một giải pháp thay thế tốt cho SIFT và SURF trong tính toán chi phí, hiệu suất phù hợp cho matching và chủ yếu là vấn đề bằng sáng chế, trong khi SIFT và SURF đã được cấp bằng sáng chế và bạn phải trả tiền cho họ để sử dụng nó. Nhưng ORB thì không!

ORB cũng sử dụng FAST để detect ra các keypoints sau đó dùng phép đo góc Harris để tìm N điểm cao nhất trong số đó và dùng BRIEF để mô tả các đặc trưng. Tuy nhiên, FAST và BRIEF được nhóm tác giả tinh chỉnh lại để khắc phục nhược điểm không bất biến khi xoay ảnh của phiên bản nguyên bản.

Kết quả hiển thị dưới đây:

500 keypoints: ![ORB](https://user-images.githubusercontent.com/81065789/152498091-ca25eabe-6fa3-4187-b9b1-28de3d36d7a6.jpg)

Bài báo cho biết ORB nhanh hơn nhiều so với SURF và SIFT và bộ mô tả ORB hoạt động tốt hơn SURF. ORB là một lựa chọn tốt trong các thiết bị tiêu thụ điện năng thấp để ghép ảnh panorama,... 

<!-- Footer -->
<p align='center'>Copyright © 2021 - Duong Hai Nguyen, Thanh Trung Nguyen</p>
