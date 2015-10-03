###Thao tác trên tập tin và thư mục trong hệ thống:

Một số dòng lệch hữu ích giúp thao tác trên tập tin và thư mục trong Terminal.

**ls**: liệt kê tất cả các tập tin có thể nhìn thấy trong thư mục hiện hành.

` ls .*`

liệt kê tất cả các tập tin bao gồm cả tập tin ẩn bắt đầu bằng “.” như .bashrc, nếu “.” được thay bằng kí tự “a” bất kì, khi đó chỉ liệt kê các tập tin bắt đầu bằng kí tự “a”.

` ls */*`

liệt kê tất cả các tập tin trong thư mục con đầu tiên.

` ls path/`

liệt kê tất cả các tập tin trong thư mục mà đường dẫn (path) chỉ tới.

` ls -ltrh`

liệt kê tập tin với các thông số (l), nếu có (t) các tệp tin sắp xếp theo thứ tự ngày chỉnh sửa, nếu có (r) thứ tự các tập tin sẽ được đảo ngược, nếu có (h) các tập tin được liệt kê với dung lượng theo đơn vị (GB/MB/KB).

**cd**: Thay đổi thư mục.

` cd ~/`

di chuyển tới thư mục “home”.

` cd -`
quay về thư mục trước đó.

` cd ../`

di chuyển tới thư mục trên (thư mục bao gồm thư mục hiện hành).

` cd path/`

di chuyển tới thư mục mà đường dẫn (path) chỉ tới.

**mkdir**: Tạo thư mục mới.

**pwd**: Xem đường dẫn của thư mục hiện hành.

**ln**: Tạo liên kết mới cho thư mục.

Giả sử bạn có một đường dẫn dài để chỉ tới thư mục “intel” như sau: **/usr/local/source/file/compiler/bin/intel/**, bạn muốn tạo một đường dẫn ngắn gọn hơn để dễ thao tác trong như mục hiện hành. 

Dòng lệnh thực hiện như sau: **ln -s  /usr/local/source/file/compiler/bin/intel/ abc**. Khi đó bạn đã tạo ra một thư mục con abc/ trong thư mục hiện hành mà liên kết với thư mục intel. Khi đó đường dẫn abc/ là tương đương với đường dẫn **/usr/local/source/file/compiler/bin/intel/**. Muốn xóa đường dẫn này đi bạn sử dụng lệnh rm (bên dưới).

**df**: Xem có bao nhiêu dung lượng trống để có thể sử dụng.

` df -h`

xem có bao nhiêu dung lượng trống trong ổ đĩa (đơn vị GB/MB/KB).

**cp/mv**: Sao chép hoặc di chuyển tệp tin từ nơi này đến nơi khác.

`cp a1.txt path/` hoặc `mv a1.txt path/`

sao chép hoặc di chuyển tập tin a1.txt tới thư mục đường dẫn chỉ tới.

`cp -i a1.txt a2.txt` hoặc `mv -i a1.txt a2.txt`

nếu có (i), khi đó nếu tập tin a2.txt tồn tại, nó sẽ hỏi bạn có ghi đè lên không.

`cp -f a1.txt a2.txt` hoặc `mv -f a1.txt a2.txt`

ghi đè ngay cả tập tin a2.txt tồn tại.

`cp -r path1/ path2/` hoặc `mv -r path1/ path2/`

sao chép hoặc di chuyển tất cả các tập tin trong thư mục 1 tới tới thư mục 2.

**rm**: Xóa tệp tin hoặc thư mục.

- `rm -i a1.txt`

nếu có (i), sẽ hỏi có xóa hay không.

- `rm -f path/`

xóa toàn bộ thư mục (hãy chắc chắn rằng bạn tất cả các tập tin trong thu mục bạn muốn xóa).

> rm -f *`*find path/ -name "a"*`*

xóa toàn bộ file có tên là a được tìm thấy trong thư mục bạn chọn.

**vi/nano**: Chỉnh sửa hoặc xem tập tin.

- `nano a1.txt hoặc vi a1.txt`

(nano) được sử dụng khi cần chỉnh sửa nhanh chóng và đơn giản. Trong khi đó (vi) có nhiểu câu lệnh cho các chỉnh sửa phức tạp hơn. 

**tail**: Hiển thị phần cuối cùng của tập tin.

- `tail a1.txt`

**chmod**: Thay đổi quyền thao tác tập tin.

u: người đang dùng                      r: đọc
g: nhóm                                              w: viết
o: người dùng khác                       x: thực thi (execute permission)
a: cho tất cả người dùng

- `chmod u+x a1.txt`    hoặc    `chmod u-rx a1.txt`

file a1.txt thành file thực thi hoặc loại bỏ quyền đọc và thực thi của file a1.txt đối với người dùng.

- `chmod a+x a1.txt`   hoặc   `chmod +x a1.txt`

hai lệnh này là tương đương, cho phép file a1.txt được thực thi với tất cả người dùng

**locate**: Kiểm tra cơ sở dự liệu trong thư mục hiện hành.

- `locate ABC`

tìm kiếm và xem tất cả các đường dẫn và tập tin có tên bao gồm cụm từ đại diện “ABC”.

**tar**: Nén và giải nén tập tin.

- `tar zxvf a1.tgz`

giải nén tập tin a1.tgz.

- `tar cvzf a1.tgz a1.txt`

nén tập tin a1.txt thành tập tin a1.tgz.
