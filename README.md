# Sample_01

# 1.Thông tin chung


![image](https://github.com/YeuPIPI/Sample_01/assets/72372550/b7fb13f9-cb76-40a3-85a4-13775f4ca9d8)


# 2.Phân tích chi tiết

Phân tích theo luồng thực thi của mã độc . Chia thành phân tích 3 stage chính 

## 2.1 Stage 1

![image](https://github.com/YeuPIPI/Sample_01/assets/72372550/cd38ebab-f7fb-4b4e-87bf-143f53d5f090)

Ban đầu nó lấy handle của console và ẩn nó đi . Tiếp theo nó get cái foder mà nó chỉ định create 1 directory là Explorer . Sau khi set attribute thì nó sẽ đi vào hàm Rename_and_CreateKey . 

![image](https://github.com/YeuPIPI/Sample_01/assets/72372550/e8b79e2c-5639-4fdf-8523-e952a537775b)


Ở hàm này nó sẽ copy file ban đầu là Explorer.exe đến foder mà nó tạo trước và rename thành Unikey.exe để ngụy trang . Sau đó nó sẽ create 1 key và set value key là Unikey NT nhằm mục đích persistant chương trình độc hại này

## 2.1 Stage 2










