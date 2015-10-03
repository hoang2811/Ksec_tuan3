### 1. Các khái niệm cơ bản về quản lý account của Linux và Unix
- Account trên Linux/Unix bao gồm nhiều thông tin trong đó hai phần liên quan đến việc sử dụng là username và userID:
  + **username**: khi sử dụng để login, gán quyền, v.v.. chúng ta thực hiện thông qua username, nhưng hệ thống lại hiểu và làm theo userID.
  + **userID**: Số đi kèm với username, hệ điều hành dùng số này để quản lý. Như vậy nếu có hai username khác nhau nhưng dùng chung một userID, thì hệ thống xem hai tên này chỉ là một.
- Quyền hạn: Linux và Unix chỉ phân biệt user làm hai loại
  + **User có quyền root**: Tất cả những user có userID=0, thường thì với một máy mới, chúng ta sẽ có ngay một user tên root và có userID=0. Nếu chúng ta tạo ra một user khác và sau đó sửa userID của nó thành 0, thì lúc này nó có quyền root y chang user tên là root của hệ thống.
  + **User thường**: Tất cả các user có userID khác 0 điều là người dùng thường.
- User và Group :Mỗi user trên linux bắt buộc phải thuộc một group nào đó (gọi là Primary Group), ngoài ra còn có thể lựa chọn tham gia vào các group khác (gọi là Secondary Group), theo một số tài liệu thì user có thể tham gia vào tối đa 16 Secondary Group.
- Trên Linux và Unix , tất cả thông tin về users và groups điều được lưu vào các tệp tin văn bản thường. Vì vậy thay vì bạn dùng lệnh để quản lý user, có thể mở các files này ra sửa trực tiếp. Tuy nhiên chỉ làm vậy khi thật cần thiết, và với mục đích học tập mà thôi. Trước khi sửa chữa các bạn nên backup lại. Theo tôi bạn chỉ nên "đọc" mà thôi!

###2. Quản lý các user
Thông tin về các user được lưu trữ trong các files: **/etc/passwd** và **/etc/shadow**.

**/etc/passwd**: File này chứa thông tin về user, điều khiển việc login của các user.
File này được lưu dưới dạng ASCII, mỗi dòng lưu thông tin của một user, và mỗi dòng lại phân thành các trường bằng dấu hai chấm. Như vậy thông tin đã được lưu dưới dạng một "bảng". Cấu trúc của nó như sau:
**UserName : Password : UserID : PrincipleGroup : Comments : HomeDirectory : Shell**

####Ý nghĩa của cụ thể của các trường:

  1-**usename**: tên đăng nhập, phân biệt Hoa/thường, nên dùng chữ thường.
  
  2-**password**: lưu chuỗi passwd đã hash, nếu có sử dụng /etc/shadow thì ở đây sẽ là chữ x
  
  3-**user ID**: hệ thống dùng user ID để phân biệt người này với người khác.
  
  4-**group ID**: Đây là Primary Group của user này.
  
  5-**comment**: mô tả cho user.
  
  6-**Home Directory**: Thư mục home của từng user, thường sẽ nằm trong /home/tenuser
  
  7-**Shell**: Tên chương trình sẽ thực thi ngay sau khi user login vào. Nếu không có shell user sẽ không thể login. Mặc nhiên trên Linux sẽ dùng bash shell ở đây.

Bạn xem nội dung của /etc/passwd bằng lệnh:

>**$ cat /etc/passwd**

>**root: x:0:0:root:/root:/bin/bash**

>**...**

**/etc/shadow** : Chứa chuỗi password đã mã hóa bằng hàm băm và thông tin về khác như Password Age của User.

- 1-usename: phân biệt Hoa/thường, nên dùng chữ thường.
- 2-password: lưu chuỗi passwd đã hash. Nếu nó chứa ! hay * thì người dùng không thể login vào tài khoản này
- 3-ngày sửa cuối: tính từ 1/1/1970
...
- 9-trường này để dành
Xem nội dung của **/etc/shadow** gần giống như cách ở trên nhưng lile này chứa các thông tin nhạy cảm nên nó chỉ có thể đọc được nếu bạn có quyền root. Chuyển sang root bằng lệnh 'su' nếu hệ điều hành cho phép root login, nếu không có thể dùng sudo như sau:

>**$ sudo cat /etc/shadow**

Bạn có thể đọc 'man 5 shadow' để có thông tin đầy đủ về nó.

####Nhóm lệnh quản lý user
- **Tạo user:**
  $ useradd [options]
  
các tùy chọn:
- **-u UID**
- **-g** GID
- **-G** GID : Danh sách các Secondary group có thể cách nhau bằng dấu phẩy ","
- **-c** ghi chú
- **-d** directory
- **-m** Nếu như thư mục của -d chưa có, thì tự động tạo mới.
- **-s** shell : mặc nhiên sẽ dùng bash shell

-**Sửa user**: 

usermod [options] [-l ]

options: hầu hết giống như của lệnh useradd

-**Đổi password**:
  + Mỗi user có khả năng tự đổi passwd của chính họ, với điều kiện họ nhớ passwd cũ và phải tuân theo nguyên tắc đặt passwd của Linux. root được phép đổi passwd của tất cả cả các user mà không cần biết passwd cũ, cũng như không cần tuân theo nguyên tắc đặt passwd!

>$ passwd []

- **Xóa user**:

>$ userdel [-r]

**-r** : xóa luôn home directory

Xem thêm các lệnh khác như: chfn (thay đổi tên người dùng và các thông tin), chsh (Change login shell) v.v..

Để biết cú pháp cũng như các tùy chọn, hãy sử dụng tùy chọn --help hay đọc trong man.

###3. Quản lý nhóm (group).
Thông tin về nhóm cũng tương tự như user được lưu trong : **/etc/groups** và **/etc/gshadows**.

**/etc/groups** : Chứa thông tin về các groups

Cấu trúc của nó như sau:
**GroupName : Password : GroupID : User1,User2,..., Usern**

Mô chi tiết các trường:
- 1-groupname: tên nhóm
- 2-passwd: lưu chuỗi passwd đã băm, trong trường hợp có dùng /etc/gshadow thì chỗ này được ghi là x
- 3-Group ID: ID của nhóm
- 4-users: Danh sách các user nhận group này là secondary, ngăn cách nhau bằng dấu phẩy

**/etc/gshadows** : Chứa thông tin password của groups.

- 1-groupname: tên nhóm.
- 2-passwd: chuỗi passwd đã mã hóa bằng các hàm băm.
- 3-admins: danh sách các user có quyền admin trên group này.
- 4-users: các user

#####Nhóm lệnh quản lý group
- **Tạo group**:
>$ groupadd [-g groupid]

- **Sửa group**:
>$ groupmod [-n New name] [-g new goupid]

- **Đổi group password**:
>gpasswd []

- **Xóa group**:
>$ groupdel

Linux cho phép quản lý users/groups theo cả kiểu cũ và mới. Nhưng bạn nên sử dụng theo mặc định khi cài, thường là kiểu mới.
  + Kiểu cũ: chuỗi password được lưu luôn trong /etc/passwd, và không có file /etc/shadow.
  + Kiểu mới: Để tránh việc chuỗi passwd bị dòm ngó dù đã được mã hóa, giờ đây chuỗi passwd được di dời vào trong /etc/shadow mà chỉ có root được xem. Đồng thời cho phép lưu thêm các thông tin về Password Age. Chuyện tương tự với group!

###4. Thay đổi thông số mặc định
- Khi sử dụng lệnh useradd hoặc groupadd, nếu chúng chúng ta không liệt kê đầy đủ các thông số cần thiết thì hệ thống sẽ lấy theo giá tri mặc nhiên đã được định nghĩa.
Chúng ta có thể thay đổi định nghĩa những giá trị này trong file sau:
  + **/etc/login.defs** : file chứa thông số mặc định khi tạo user hoặc tạo group.
  + **/etc/skel/** : Tất cả những file là thư mục con trong này sẽ được copy sang HOME của user mới.
