# Phần 1 - Phân tích logic
# Tại sao Line Coverage cao nhưng vẫn còn nhiều bug logic?
Line Coverage chỉ đo:
Có bao nhiêu dòng code đã được chạy khi test.
Nó không đảm bảo:
Tất cả điều kiện logic đã được kiểm tra
Tất cả nhánh true/false đã được chạy
Tất cả tình huống lỗi đã được test

# 2. Phân biệt Line Coverage và Branch Coverage
   | Tiêu chí            | Line Coverage     | Branch Coverage     |
   | ------------------- | ----------------- | ------------------- |
   | Kiểm tra gì         | Dòng code đã chạy | Tất cả nhánh logic  |
   | Quan tâm if/else    | Không đầy đủ      | Có                  |
   | Phát hiện lỗi logic | Kém hơn           | Tốt hơn             |
   | Độ tin cậy          | Trung bình        | Cao hơn             |
   | Mục tiêu            | Bao phủ code      | Bao phủ luồng logic |

# 3. Vì sao Branch Coverage quan trọng hơn?
Branch Coverage đảm bảo:
cả nhánh true và false đều được test
các case lỗi được kiểm tra
các tình huống biên được kiểm tra
Điều này cực kỳ quan trọng với:
if/else
switch/case
try/catch
validation
kiểm tra null
nghiệp vụ nhiều điều kiện
Nếu chỉ nhìn Line Coverage:
90%
thì vẫn có thể tồn tại:
lỗi validation
lỗi điều kiện biên
lỗi phân quyền
lỗi tính toán


# Ví dụ 1: Line Coverage cao nhưng thiếu Branch Coverage
Mã nguồn
public String login(String username, String password) {

    if (username.equals("admin")
            && password.equals("123")) {

        return "SUCCESS";
    }

    return "FAILED";
}
Test hiện tại
login("admin", "123");
Kết quả
Line Coverage
gần như 100%
tất cả dòng đều chạy
Nhưng Branch Coverage thấp
Nhánh chưa test:
return "FAILED";
Bug tiềm ẩn
Nếu code sai:
return null;
thì Line Coverage vẫn cao nhưng bug vẫn lọt.
Branch Coverage sẽ yêu cầu test thêm:
login("admin", "wrong");


# Ví dụ 2: Điều kiện biên
Mã nguồn
public double calculateDiscount(double total) {

    if (total >= 1000) {
        return total * 0.1;
    }

    return 0;
}
Test hiện tại
calculateDiscount(1500);
Kết quả
Line Coverage
rất cao
toàn bộ dòng chạy
Nhưng thiếu Branch Coverage
Nhánh chưa test:
return 0;
Điểm biên chưa test:
999
1000
1001
Bug tiềm ẩn
Nếu dev viết sai:
if (total > 1000)
thì:
total = 1000
sẽ lỗi.
Line Coverage không phát hiện vì chưa test boundary.
Branch Coverage + Boundary Testing sẽ phát hiện ngay.