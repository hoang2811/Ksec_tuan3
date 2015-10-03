###Quyền hạn

Trong Linux có 3 dạng đối tượng :

- Owner (người sở hữu).
- Group owner (nhóm sở hữu).
- Other users (những người khác).

Các quyền hạn :

- Read – r – 4  : cho phép đọc nội dung.
- Write – w – 2  : dùng để tạo, thay đổi hay xóa.
- Execute – x – 1  : thực thi chương trình.

Vídụ : Với lệnh ls –l ta thấy :

`[root@task ~]# ls -l` 

`total 32`

`-rw-------. 1 root root 1416 Jan 10 14:06 anaconda-ks.cfg`

`-rw-r--r--. 1 root root 15522 Jan 10 14:06 install.log` 

`-rw-r--r--. 1 root root 5337 Jan 10 14:06install.log.syslog` 

`drwxr-xr-x 6 root root 4096 Feb 9 10:02 softs`

Ngoài ra, chúng ta có thể dùng số.

Vídụ : quyền r, w, x : 4+2+1 = 7

*Tổ hợp 3 quyền trên có giá trị từ 0 đến 7.*

###Các lệnh liên quan đến quyền hạn

- **Lệnh Chmod** : dùng để cấp quyền hạn.

Cú pháp : **#chmod**  <specification> <file>

Ví dụ: #chmod 644 baitap.txt   //cấp quyền cho owner có thể ghi các nhóm các chỉ có quyền đọc với file taptin.txt

- **Lệnh Chown** : dùng thay đổi người sở hữu.

 Cú pháp : **#chown**  <owner>  <filename>
 
- **Lệnh Chgrp** : dùng thay đổi nhóm sở hữu.

Cú pháp : **#chgrp**  <group>  <filename>

####Quyền hạn
#####1. Sơ lựơc về quyền

Trên Linux, quyền trên files hoặc thư mục được ghi vào trong inode của file hoặc thư mục đó. Để dễ quản lý, Linux xem mọi thứ như là file.

Để xem quyền, chúng ta có thể sử dụng lệnh “ls -l” như trong hình sau:
![](http://imgur.com/6jjNrTd.png)

Cột đầu tiên trong kết quả của lệnh ls -l thể hiện cho quyền hạn. Phần này bao gồm 10 bit:

- bit 1: thể hiện kiểu file (vd: “-” file thường, “d” thư mục,…)
- 9 bit còn lại chia làm 3 nhóm, mỗi nhóm có 3 bit:
  + owner: Quyền của user mà owner của file này.
  + group: Quyền của những users thuộc group mà owner của file này.
  + other: Quyền của tất cả các user khác trên máy.
  
Trong mỗi nhóm (3 bit) thể hiện cho các quyền đọc (r), ghi (w), thực thi (x). Nếu nơi nào không có quyền sẽ được ghi là denied (-).

|Ký hiệu|	file	|thư mục|
|-------|----------------------------------------|---------------------------------------------|
|-	|Không có quyền	|Không có quyền|
|r	|Có thể đọc file, ví dụ dùng lệnh cat, more, ….	|Có thể xem nội dung trong thư mục, ví dụ dùng lệnh ls.|
|w	|Có thể thêm, bớt nội dung trong file.	|Có thể tạo thêm hoặc xóa đi file hoặc thư mục con của thư mục này.|
|x	|Có thể thực thi file này, chỉ dùng trong trường hợp đây là program hoặc script.	|Có thể “đứng” trong thư mục được, vi dụ cd vào thư mục này.|

#####2. Thay đổi quyền

Chỉ có user có quyền root, hoặc user là owner của file mới có thể thay đổi quyền của file đó. Lệnh chuyển quyền:

`chmod mode file`

Trong đó “mode” có thể được viết theo 2 cách: symbolic hoặc octal mode

**a. symbolic mode**

![](http://imgur.com/kHyAaQ0.png)

Trong mode này chúng ta có thể thêm (+), bớt (-), gán (=) các quyền (r w x) cho từng nhóm (u g o) hoặc cho cả 3 nhóm (a).

Ưu điểm của mode này là chúng ta có thể kế thừa quyền củ.

Vd:

`chmod g-w myfile` <=bỏ quyền write trên group.

`chmod u+x,go+r` <=thêm quyền thực thi cho owner, quyền đọc cho group và other.

**b. octal mode**

Trong mode này, mổi quyền được thể hiện bằng 1 số tương ứng:

`- : 0`

`x : 1`

`w : 2`

`r : 4`

Quyền sẽ được tính tổng trên từng nhóm, vd: r(4)+w(2)+x(1)=7.

Khi gán quyền phải gán cho cả 3 nhóm.

ví dụ quyền và số octal tương ứng:

`644 rw-r–r–`

`751 rwxr-x–x`

`775 rwxrwxr-x`

`777 rwxrwxrwx`

Dùng lệnh: chmod

`chmod 644 myfile` <=gán quyền 644 trên file.

Với cách sử dụng như trên, khi dùng octal mode chúng ta không kế thừa được quyền củ, nhưng bù lại chúng ngắn gọn dễ xài. :D

####3. Quyền mặc định

Khi chúng ta tạo ra file hoặc thư mục, mặc nhiên hệ thống sẽ gán cho 1 quyền mặc định, trong đó:

`File: 666 (rw-rw-rw-)`

`Thư mục: 777 (rwxrwxrwx)`

Để thay đổi quyền mặc định khi tạo file và thư mục, hệ thống cung cấp cho chúng ta một công cụ đó là umask.

Như vậy, khi tạo ra file hoặc thư mục, thì quyền mặc định được tính như sau:

`File: 666 – umask`

`Thư mục: 777 – umask`

Ví dụ, nếu umask=022 thì quyền mặc định trên file và thư mục sẽ là (644) và (755)

![](https://dacchieu.files.wordpress.com/2012/12/quyenhan3.gif?w=226&h=259.png)

**Lưu ý**: ví dụ như umask=123, thì quyền mặc định cho file sẽ là (644) chứ không phải (543) 

Chúng có thể xem hoặc thay đổi giá trị umask như trong ví dụ sau:

`$ umask`

`022`

`$ umask 027`

`$ umask`

`027`

###### Thay đổi Owner

Chúng ta có thể thay đổi chủ sở hữu của file (owner) bằng lệnh sau:

`chown -R [user.group] files`

**-R** : đổi tất cả files và thư mục con.

Lệnh này cũng cho phép thay đổi owner chỉ riêng user hoặc group hoặc cả hai.

vd:

`# ls -l`

`total 0`

`-rw-r–r– 1 nhsang nhsang 0 Nov 23 16:46 file1`

`-rw-r–r– 1 nhsang nhsang 0 Nov 23 16:46 file2`

`-rw-r–r– 1 nhsang nhsang 0 Nov 23 16:46 file3`

`# chown oravn file1` <=đổi user owner

`# chown .dba file2` <=đổi group owner

`# chown oravn.dba file3` <=đổi cả user và group owner


>`# ls -l`

>`total 0`

>`-rw-r–r– 1 oravn nhsang 0 Nov 23 16:46 file1`

>`-rw-r–r– 1 nhsang dba 0 Nov 23 16:46 file2`

>`-rw-r–r– 1 oravn dba 0 Nov 23 16:46 file3`

Lưu ý là bạn chỉ có thể dùng lệnh này khi bạn là user mà owner của file này hoặc user có quyền root.

