# Sổ tay lệnh Linux \& Bash (từ cơ bản đến nâng cao)

> Tổng hợp từ quá trình chơi OverTheWire Bandit. Các mục đánh dấu \*\*\[+]\*\* là phần \*\*bổ sung\*\* ngoài ghi chú gốc của bạn.
> Thứ tự: làm quen terminal → file/thư mục → xem và sửa file → quyền → tìm kiếm/xử lý text → luồng dữ liệu → nén/mã hóa → tiến trình/dịch vụ → mạng → tự động hóa → Bash scripting → thủ thuật.

## Mục lục

1. [Làm quen với terminal](#1-làm-quen-với-terminal)
2. [Thao tác với file và thư mục](#2-thao-tác-với-file-và-thư-mục)
3. [Xem nội dung file](#3-xem-nội-dung-file)
4. [Soạn thảo file: nano và vi](#4-soạn-thảo-file-nano-và-vi)
5. [Người dùng, quyền truy cập và sudo](#5-người-dùng-quyền-truy-cập-và-sudo)
6. [Tìm kiếm và xử lý văn bản](#6-tìm-kiếm-và-xử-lý-văn-bản)
7. [Luồng dữ liệu, chuyển hướng và pipe](#7-luồng-dữ-liệu-chuyển-hướng-và-pipe)
8. [Mã hóa, nén và giải nén](#8-mã-hóa-nén-và-giải-nén)
9. [Phần mềm, tiến trình, dịch vụ và log](#9-phần-mềm-tiến-trình-dịch-vụ-và-log)
10. [Mạng](#10-mạng)
11. [Tự động hóa: file script và cron](#11-tự-động-hóa-file-script-và-cron)
12. [Bash scripting](#12-bash-scripting)
13. [Thủ thuật: thoát shell bị hạn chế](#13-thủ-thuật-thoát-shell-bị-hạn-chế)
14. [Những chỗ đã sửa so với ghi chú gốc](#14-những-chỗ-đã-sửa-so-với-ghi-chú-gốc)

\---

## 1\. Làm quen với terminal

|Lệnh|Ý nghĩa|
|-|-|
|`pwd`|(print working directory) hiển thị đường dẫn thư mục hiện tại mà bạn đang đứng trong shell.|
|`ls`|Liệt kê file và thư mục.|
|`ls -a`|Xem toàn bộ file kể cả file ẩn (tên bắt đầu bằng dấu `.`).|
|`ls -l`|Xem dạng chi tiết (quyền, chủ sở hữu, nhóm, kích thước, ngày sửa).|
|`ls -ld`|Xem quyền của chính thư mục hoặc file đó (không liệt kê bên trong).|
|`ls -lh` **\[+]**|Như `ls -l` nhưng kích thước dễ đọc (K, M, G).|
|`cd <thư mục>`|Chuyển sang thư mục khác.|
|`cd ..` / `cd \~` / `cd -` **\[+]**|Lên thư mục cha / về thư mục home / quay lại thư mục trước đó.|
|`man <lệnh>`|Xem hướng dẫn sử dụng của các lệnh trong Linux (thoát bằng `q`).|
|`id`|Hiển thị UID (User ID), GID (Group ID) và các nhóm mà bạn thuộc về (của bạn hoặc của user chỉ định).|
|`whoami` **\[+]**|In ra tên user hiện tại.|
|`hostname`, `uname -a` **\[+]**|Tên máy và thông tin hệ điều hành/kernel.|
|`clear` **\[+]**|Xóa màn hình terminal.|
|`history` **\[+]**|Xem lại các lệnh đã gõ.|
|`which <lệnh>` **\[+]**|Cho biết lệnh đó nằm ở đường dẫn nào.|
|`exit` **\[+]**|Thoát shell hiện tại (nếu đang trong phiên SSH thì đăng xuất khỏi phiên đó).|

**Ký hiệu đường dẫn đặc biệt**

* `.` → thư mục hiện tại (current directory).
* `..` → thư mục cha (parent directory).

**Phím tắt hay dùng \[+]**

|Phím|Tác dụng|
|-|-|
|`Tab`|Tự động hoàn thành tên lệnh, file, thư mục.|
|`Ctrl + C`|Dừng lệnh đang chạy.|
|`Ctrl + D`|Kết thúc đầu vào (tương đương `exit` khi dòng lệnh trống).|
|`Ctrl + L`|Xóa màn hình.|
|`Ctrl + R`|Tìm kiếm trong lịch sử lệnh.|
|`↑` / `↓`|Duyệt lại các lệnh trước đó.|

\---

## 2\. Thao tác với file và thư mục

> `mk` không phải một lệnh chuẩn, mình hiểu bạn muốn nói `mkdir`.

|Lệnh|Ý nghĩa|Ví dụ|
|-|-|-|
|`mkdir` **\[+]**|Tạo thư mục. `-p` tạo cả chuỗi thư mục lồng nhau.|`mkdir -p duan/src/test`|
|`rmdir` **\[+]**|Xóa thư mục **rỗng**.|`rmdir thumuc`|
|`touch` **\[+]**|Tạo file rỗng, hoặc cập nhật thời gian sửa đổi nếu file đã có.|`touch a.txt`|
|`cp` **\[+]**|Sao chép file. `-r` để sao chép cả thư mục.|`cp a.txt b.txt`, `cp -r dir1 dir2`|
|`mv` **\[+]**|Di chuyển hoặc đổi tên file/thư mục.|`mv a.txt /tmp/`, `mv cu.txt moi.txt`|
|`rm` **\[+]**|Xóa file. `-r` xóa cả thư mục, `-i` hỏi lại trước khi xóa.|`rm a.txt`, `rm -r dir1`|
|`mktemp`|Tạo file tạm có tên ngẫu nhiên. `-d` tạo **thư mục** tạm (ví dụ `/tmp/tmp.VCwCEhs0GF`).|`mktemp -d`|
|`du` **\[+]**|Xem dung lượng thư mục/file. `-h` dễ đọc, `-s` chỉ in tổng.|`du -sh thumuc`|
|`df -h` **\[+]**|Xem dung lượng trống của các ổ đĩa.|`df -h`|

> \*\*Cảnh báo:\*\* `rm` xóa là mất hẳn, không có thùng rác. Đặc biệt đừng chạy `rm -rf` khi chưa chắc về đường dẫn, nhất là với `sudo`.

**Mẹo từ Bandit**

* Khi copy file thì nên làm việc trên **bản sao trong thư mục tạm** (`mktemp -d` rồi `cp`) để giữ an toàn cho file gốc tránh trường hợp lỗi dẫn đến mất file.

\---

## 3\. Xem nội dung file

### `cat`

* `cat filename`: đọc file. `cat f1 f2`: nối 2 file lại với nhau (cat dùng để ghép file hoặc đọc file).
* `cat` không đọc được file có tên là dấu gạch ngang (`-`) vì bị hiểu là tùy chọn. Cần thêm tiền tố `./` vào (VD: `cat ./-`).
* Dấu `--` thông báo là tất cả các nội dung phía sau đó là tên file hoặc dữ liệu chứ không phải tùy chọn (VD: `cat -- -`). `--` và `./` cùng giải quyết việc tên file bị hiểu nhầm là tùy chọn.

### Các lệnh xem file khác

|Lệnh|Ý nghĩa|
|-|-|
|`less <file>` **\[+]**|Xem file theo từng trang, cuộn lên xuống, `/từ\_khóa` để tìm, `q` để thoát.|
|`more <file>`|Xem nội dung file theo từng trang thay vì hiển thị hết một lần. Tiện khi file dài. `more` sẽ tự thoát khi xuất văn bản xong.|
|`head -n 10 <file>` **\[+]**|Xem 10 dòng đầu.|
|`tail -n 10 <file>` **\[+]**|Xem 10 dòng cuối. `tail -f` theo dõi file đang được ghi thêm (rất hữu ích khi đọc log).|
|`wc -l <file>` **\[+]**|Đếm số dòng. `-w` đếm từ, `-c` đếm byte.|
|`file <tên>`|Kiểm tra xem đó là tệp hay thư mục. Nếu là tệp thì cho biết nó thuộc thể loại gì.|
|`strings <file>`|Kiểm tra các ký tự có thể đọc được trong file (thường dùng với file nhị phân).|
|`xxd <file>`|Hiển thị dữ liệu dạng hex (hex dump). Xem thêm ở phần [8](#8-mã-hóa-nén-và-giải-nén).|

**Về dấu `\*` và file ẩn:** nếu dùng `file ./\*` thì có 1 vấn đề là dấu `\*` được shell mở rộng thành tất cả file trong thư mục hiện tại **mà không bắt đầu bằng dấu chấm**, nên sẽ bỏ sót file ẩn. Vì vậy hãy dùng `ls -a` để xem toàn bộ.

\---

## 4\. Soạn thảo file: nano và vi

### `nano` **\[+]**

Trình soạn thảo đơn giản, dễ dùng cho người mới.

|Phím|Tác dụng|
|-|-|
|`nano <file>`|Mở (hoặc tạo) file.|
|`Ctrl + O` rồi `Enter`|Lưu file.|
|`Ctrl + X`|Thoát.|

### `vi` / `vim`

`vi` là một trình soạn thảo văn bản cổ điển trên Linux/Unix, chạy ngay trong terminal. Nó thường được dùng để chỉnh sửa file cấu hình, script, hoặc bất kỳ file text nào.

Trong vi/vim, dấu `:` chính là cách bạn chuyển sang **Command-line mode** (chế độ dòng lệnh). Đây là cơ chế quan trọng giúp bạn nhập các lệnh điều khiển thay vì soạn thảo văn bản.

|Lệnh|Ý nghĩa|
|-|-|
|`:w`|Lưu|
|`:q`|Thoát|
|`:wq`|Lưu và thoát|
|`:q!`|Thoát không lưu|
|`:set number`|Hiện số dòng|
|`:set shell=/bin/bash`|Đặt shell mặc định (`:set shell` để xem hoặc thay đổi shell mà vi sẽ gọi khi chạy lệnh bên ngoài)|
|`:!ls`|Chạy lệnh `ls` trong shell|
|`:!bash`|Mở một phiên Bash tạm thời|
|`:shell`|Tạm thời thoát ra ngoài để mở một shell con ngay bên trong phiên làm việc|

**\[+]** Mẹo cho người mới: bấm `i` để vào chế độ soạn thảo, bấm `Esc` để quay lại chế độ lệnh.

\---

## 5\. Người dùng, quyền truy cập và sudo

### Cơ chế phân quyền

Quyền trong Linux được biểu diễn bằng 3 nhóm:

* **Owner (u)** → người sở hữu file
* **Group (g)** → nhóm của file
* **Others (o)** → người dùng khác

Mỗi nhóm có 3 quyền cơ bản:

|Quyền|Giá trị|Ý nghĩa|
|-|-|-|
|`r` (read)|4|đọc|
|`w` (write)|2|ghi|
|`x` (execute)|1|thực thi|

Ta cộng các giá trị này lại để ra số cho từng nhóm. Ví dụ `645 file.txt`:

* Owner: 6 = 4 (r) + 2 (w) → `rw-`
* Group: 4 = 4 (r) → `r--`
* Others: 5 = 4 (r) + 1 (x) → `r-x`
* Kết quả: `rw-r--r-x`

Dùng `ls -l` để xem quyền:

```
-rw-r-----   1 bandit14 bandit13 2602 Sep 26 21:52 sshkey.private
```

(Ký tự đầu: `-` là file thường, `d` là thư mục. Tiếp theo là quyền của owner, group, others. Sau đó là user sở hữu, nhóm sở hữu.)

### `chmod` (change mode)

Dùng để thay đổi quyền truy cập (permissions) của file hoặc thư mục.

* **Dạng số:** `chmod 600 file.txt` (chỉ nhóm Owner có quyền đọc và ghi).
* **Dạng chữ:** `u` = user (owner), `g` = group, `o` = others, `a` = all.

|Toán tử|Ý nghĩa|Ví dụ|
|-|-|-|
|`+`|thêm quyền|`chmod u+x file.txt` (thêm quyền thực thi cho owner)|
|`-`|xóa quyền|`chmod g-w file.txt` (xóa quyền ghi của group)|
|`=`|chỉ có quyền|`chmod o=r file.txt` (others chỉ có quyền đọc)|

**Lưu ý:** private key SSH chỉ chấp nhận quyền truy cập dành riêng cho user, group và others không được có quyền, nên phải `chmod 600 sshkey.private`.

### `chown` và `chgrp` **\[+]**

|Lệnh|Ý nghĩa|Ví dụ|
|-|-|-|
|`chown user file`|Đổi chủ sở hữu file.|`sudo chown nhan a.txt`|
|`chown user:group file`|Đổi cả chủ sở hữu và nhóm.|`sudo chown nhan:dev a.txt`|
|`chown -R user dir`|Đổi cho cả thư mục và mọi thứ bên trong.|`sudo chown -R nhan duan/`|
|`chgrp group file`|Đổi nhóm sở hữu file.|`sudo chgrp dev a.txt`|

### `umask`

`umask` dùng để xem hoặc đặt "mặt nạ quyền" mặc định khi tạo file/thư mục mới. Nó không thay đổi quyền của file đã có, mà chỉ ảnh hưởng đến những file/thư mục được tạo sau đó. Mỗi terminal (mỗi tiến trình shell) có thể có umask riêng, nhưng trong một tiến trình thì chỉ có một giá trị umask đang hiệu lực.

Khi tạo file mới, hệ thống thường cấp quyền mặc định: file `666` (rw-rw-rw-), thư mục `777` (rwxrwxrwx). umask sẽ trừ đi các quyền được chỉ định trong mặt nạ để ra quyền thực tế. Ví dụ `umask 022`:

* File mới: 666 − 022 = 644 → `rw-r--r--`
* Thư mục mới: 777 − 022 = 755 → `rwxr-xr-x`

### `setuid`

`setuid` là một cờ quyền đặc biệt trong hệ thống Linux/Unix. Nó cho phép một chương trình chạy với quyền của **chủ sở hữu file** thay vì quyền của người dùng đang thực thi. Dùng `ls -l` để nhận biết (chữ `s` ở vị trí quyền của owner):

```
-rwsr-xr-x 1 root root 12345 Jul 21 11:00 myprog
```

### `sudo` và `su` **\[+]**

`sudo` cho phép chạy một lệnh với quyền của user khác (mặc định là `root`) nếu bạn được cấp phép.

|Lệnh|Ý nghĩa|
|-|-|
|`sudo <lệnh>`|Chạy lệnh với quyền root.|
|`sudo -l`|Xem bạn được phép chạy những lệnh nào với sudo.|
|`sudo -u <user> <lệnh>`|Chạy lệnh dưới quyền một user khác.|
|`sudo -i`|Mở shell root (tương tự đăng nhập root).|
|`sudo !!`|Chạy lại lệnh vừa gõ nhưng có thêm `sudo`.|
|`su - <user>`|Chuyển sang user khác (cần mật khẩu của user đó).|
|`exit`|Quay về user trước đó.|

> Chỉ dùng `sudo` khi thật sự cần. Sai lệnh với quyền root có thể làm hỏng hệ thống.

\---

## 6\. Tìm kiếm và xử lý văn bản

### `find`

* `find .` = tìm trong thư mục hiện tại.
* `find /` = tìm trong toàn bộ hệ thống.

```bash
find . -type f -size 1033c
```

Duyệt qua toàn bộ thư mục, chỉ xem các file có kích thước 1033 bytes. Đơn vị: `c` → bytes, `k` → kilobytes (1024 bytes), `M` → megabytes, `G` → gigabytes.

```bash
find . -type f -exec file {} \\;
```

Xem loại file của tất cả các file trong các thư mục (`{}` được thay bằng đường dẫn file tìm được, `\\;` kết thúc lệnh `-exec`).

**Các tùy chọn thường dùng \[+]:**

|Tùy chọn|Ý nghĩa|
|-|-|
|`-type f` / `-type d`|Tìm file / thư mục.|
|`-name "\*.txt"`|Tìm theo tên.|
|`-user bandit7 -group bandit6`|Tìm theo chủ sở hữu và nhóm.|
|`! -executable`|`!` nghĩa là không: file không có quyền thực thi.|
|`2>/dev/null`|Giấu các thông báo lỗi `Permission denied`.|

Ví dụ: `find / -type f -user bandit7 -group bandit6 -size 33c 2>/dev/null`

### `grep`

Tìm kiếm chuỗi hoặc mẫu (pattern) trong văn bản, file, hoặc đầu ra của lệnh khác.

Cấu trúc: `grep \[OPTIONS] PATTERN \[FILE...]`

|Option|Ý nghĩa|
|-|-|
|`-i`|Không phân biệt hoa thường.|
|`-n`|Hiển thị số dòng chứa kết quả.|
|`-r`|Tìm kiếm đệ quy trong thư mục.|
|`-w`|Chỉ khớp toàn bộ từ.|
|`-l`|Chỉ hiển thị tên file có kết quả.|
|`-v`|Hiển thị các dòng không khớp.|
|`-F`|So khớp theo chuỗi cố định (không regex).|
|`-x`|Khớp toàn bộ dòng.|
|`-f file1.txt`|Dùng nội dung file `file1.txt` để làm mẫu so sánh.|
|`--color=auto`|Tô màu kết quả để dễ nhìn.|
|`-E` **\[+]**|Extended Regex (cho phép dùng toán tử mở rộng như `+`, `?`).|
|`-C <N>` **\[+]**|Hiển thị N dòng ngữ cảnh trước và sau dòng kết quả.|

* **PATTERN:** `grep -E "error|warning" logfile.log` tìm kiếm theo regex (error hoặc warning).
* **FILE:** `grep "main" \*.c` tìm trong tất cả các file có đuôi `.c`.

### `sort`

Sắp xếp các dòng văn bản từ file hoặc đầu vào chuẩn (stdin). Mặc định sắp xếp theo thứ tự chữ cái tăng dần, nhưng có thể tùy chỉnh để sắp xếp theo số, đảo ngược, theo cột cụ thể, hoặc loại bỏ trùng lặp.

|Lệnh|Ý nghĩa|
|-|-|
|`sort file.txt`|Sắp xếp theo thứ tự chữ cái.|
|`sort -r file.txt`|Sắp xếp ngược (descending).|
|`sort -n file.txt`|Sắp xếp theo số (numeric).|
|`sort -nr file.txt`|Sắp xếp số theo thứ tự giảm dần.|
|`sort -k2 file.txt`|Sắp xếp theo cột thứ 2.|
|`sort -t: -k3 -n /etc/passwd`|Sắp xếp theo UID (cột 3, phân cách bằng dấu `:`).|
|`sort -u file.txt`|Loại bỏ dòng trùng lặp sau khi sắp xếp.|
|`sort -h sizes.txt`|Sắp xếp theo kích thước dạng người đọc (K, M, G).|
|`sort -V versions.txt`|Sắp xếp theo phiên bản (ví dụ: 1.9 < 1.10).|

### `uniq`

Loại bỏ các dòng trùng lặp **liên tiếp**. Thường dùng kết hợp với `sort`, vì `uniq` chỉ phát hiện được các dòng trùng nhau khi chúng nằm liền kề (`sort file.txt | uniq -u`).

Cấu trúc: `uniq \[OPTIONS] \[INPUT \[OUTPUT]]`

|Lệnh|Ý nghĩa|
|-|-|
|`uniq file.txt`|Loại bỏ dòng trùng lặp liên tiếp.|
|`uniq -c file.txt`|Hiển thị số lần xuất hiện của mỗi dòng.|
|`uniq -d file.txt`|Chỉ hiển thị các dòng bị trùng lặp.|
|`uniq -u file.txt`|Chỉ hiển thị các dòng duy nhất (không trùng).|
|`uniq -i file.txt`|Bỏ qua phân biệt hoa thường khi so sánh.|

### `cut` **\[+]**

Cắt/trích xuất một phần dữ liệu của mỗi dòng.

|Option|Ý nghĩa|
|-|-|
|`-c`|Chọn theo số ký tự (an toàn với tiếng Việt).|
|`-b`|Chọn theo số byte (tiếng Việt có dấu hoặc emoji chiếm 2–4 byte nên dễ lỗi hiển thị).|
|`-f`|Cắt theo cột/trường (phân tách mặc định là Tab).|
|`-d`|Đổi ký tự phân tách cột.|

Ví dụ: `echo "a b c" | cut -d' ' -f 1` cho ra `a`.

### `tr`

Thay thế, nén hoặc xóa các ký tự theo mong muốn. Cú pháp: `tr \[tùy chọn] SET1 \[SET2]`

|Option|Ý nghĩa|
|-|-|
|`-c`|Đảo ngược tập ký tự.|
|`-d`|Xóa ký tự.|
|`-s`|Nén chuỗi ký tự lặp (VD aaa → a).|
|`-t`|Cắt SET1 cho bằng SET2.|

Ví dụ giải ROT13: `tr 'A-Za-z' 'N-ZA-Mn-za-m'`. Xem phần [8](#8-mã-hóa-nén-và-giải-nén).

### `diff` **\[+]**

So sánh hai file, in ra các dòng khác nhau: `diff passwords.new passwords.old`

|Option|Ý nghĩa|
|-|-|
|`-q`|Chỉ báo có khác nhau hay không.|
|`-s`|Báo cả khi hai file giống hệt nhau.|
|`-u`|Hiển thị dạng "unified diff" (dùng trong patch, Git).|
|`-y`|Hiển thị song song hai file.|
|`-r`|So sánh đệ quy cả thư mục.|
|`-i`|Bỏ qua khác biệt chữ hoa/thường.|
|`-w`|Bỏ qua khác biệt khoảng trắng.|

\---

## 7\. Luồng dữ liệu, chuyển hướng và pipe

### Ba luồng chính

Trong Linux, mỗi chương trình có ba "luồng":

|Số|Tên|Ý nghĩa|
|-|-|-|
|0|stdin|đầu vào.|
|1|stdout|đầu ra chuẩn (thường là màn hình).|
|2|stderr|đầu ra lỗi (các thông báo lỗi).|

### In dữ liệu ra: `echo` và `printf`

* `echo "Xin chào"` **\[+]** in một dòng ra màn hình. `-n` không xuống dòng, `-e` cho phép dùng ký tự đặc biệt như `\\n`, `\\t`.
* `printf` in có định dạng (xem chi tiết ở phần [12](#12-bash-scripting)).

### Các toán tử chuyển hướng

|Toán tử|Ý nghĩa|
|-|-|
|`>`|Lưu đầu ra chuẩn (stdout) vào 1 file. Nếu file chưa tạo thì tạo file mới; nếu file đã có thì xóa dữ liệu cũ, lưu dữ liệu mới vào.|
|`>>`|Thêm đầu ra chuẩn (stdout) vào cuối file (không xóa dữ liệu cũ).|
|`<`|Lấy dữ liệu trong file làm đầu vào (stdin).|
|`<<<`|**Here-String**: cú pháp đặc biệt của Bash giúp truyền thẳng một chuỗi văn bản làm đầu vào cho một lệnh, thay vì dùng `echo` kết hợp với `\|`. Lưu ý `<<<` tự động thêm một ký tự xuống dòng (`\\n`) vào cuối chuỗi.|
|`2>`|Lưu đầu ra lỗi (stderr). Có thể dùng số để chỉ đến luồng mình muốn.|
|`2>\&1`|Chuyển hướng luồng lỗi (2) sang cùng nơi với luồng chuẩn (1), nên lưu được cả stdout và stderr vào cùng 1 file.|
|`\&>` **\[+]**|Cách viết tắt trong Bash hiện đại để lưu cả stdout và stderr.|
|`/dev/null`|"Thiết bị rỗng" đặc biệt, mọi thứ ghi vào đó sẽ bị xóa bỏ. `2>/dev/null` nghĩa là không in luồng stderr ra màn hình.|

### Pipe `|`

Chuyển đầu ra của chương trình này thành đầu vào cho chương trình khác. Đầu ra của lệnh 1 là đầu vào của lệnh 2.

```bash
sort data.txt | uniq -u
cat data.txt | tr 'N-ZA-Mn-za-m' 'A-Za-z'
```

\---

## 8\. Mã hóa, nén và giải nén

### `base64`

`base64` là phương thức chuyển dạng mã hóa 2 chiều từ binary sang string để có thể gửi đi được trong network một cách dễ dàng. Các binary lúc này sẽ được thể hiện bằng các ký tự mã ASCII.

**Cơ chế:** chuyển các ký tự ra bit nhị phân, sau đó chia các bit nhị phân thành nhóm 6 bit. Nếu không đủ thì thêm các bit 0 sao cho đủ 6 bit. Sau đó dùng bảng mã của base64 chuyển thành chữ cái tương ứng. Đoạn mã nào có thêm bit bổ sung thì ta bổ sung bằng dấu `=` để nhận biết. Để biết cần thêm bao nhiêu dấu `=` ta lấy tổng số bit chia cho 24 để xem còn dư bao nhiêu byte:

* Dư 1 byte (8 bit) → tạo ra 2 ký tự thật + 2 ký tự đệm → thêm `==`.
* Dư 2 byte (16 bit) → tạo ra 3 ký tự thật + 1 ký tự đệm → thêm `=`.

Cú pháp: `base64 \[OPTION] \[filename]`

|Option|Ý nghĩa|
|-|-|
|`-d` / `--decode`|Giải mã Base64|
|`-i` / `--ignore-garbage`|Bỏ qua ký tự không hợp lệ khi giải mã|
|`-w 0`|Ngăn ngắt dòng khi mã hóa (giải mã không cần)|

### ROT13 và `tr`

ROT13 là dịch vị trí đi 13 lần so với vị trí ban đầu. Có thể giải bằng lệnh `tr` (thay thế từng ký tự theo cặp tương ứng):

```bash
tr 'A-Za-z' 'N-ZA-Mn-za-m'
```

`'A-Za-z'` là tập hợp tất cả chữ cái tiếng Anh (hoa và thường). `'N-ZA-Mn-za-m'` là tập hợp chữ cái đã dịch chuyển 13 vị trí trong bảng chữ cái.

### Hex dump, `xxd` và magic number

* **Hex dump** là cách hiển thị nội dung của một file hoặc vùng nhớ dưới dạng mã hex (hệ 16), kèm theo phần biểu diễn ASCII để dễ quan sát dữ liệu nhị phân. Thường dùng để kiểm tra dữ liệu thô, debug, hoặc phân tích file.
* **`xxd`** dùng để tạo hoặc đọc file hex dump, hoặc dùng độc lập để xem dữ liệu nhị phân dưới dạng hex. Option `-r` chuyển hex dump ngược lại thành file gốc (`xxd -r data.txt > data.gz`).
* **Magic number** là một chuỗi byte đặc biệt nằm ở đầu file (header) dùng để nhận diện định dạng của file.

### `gzip` (.gz)

Công cụ nén dữ liệu dùng thuật toán DEFLATE để giảm kích thước file, giúp tiết kiệm dung lượng và tăng tốc độ truyền tải. **Magic number:** bắt đầu bằng `1F 8B`.

|Lệnh|Ý nghĩa|
|-|-|
|`gzip file`|Nén file thành `file.gz` (file gốc bị thay thế).|
|`gzip -d file.gz` hoặc `gunzip file.gz`|Giải nén.|
|`gzip -c file.txt > file.txt.gz`|Nén và **giữ lại file gốc** (`-c` ghi kết quả ra stdout rồi dùng `>` lưu thành file).|
|`zcat file.gz` hoặc `zless file.gz`|Xem nội dung file `.gz` mà không cần giải nén.|

### `bzip2` (.bz2)

Công cụ nén tương tự gzip nhưng dùng thuật toán Burrows–Wheeler transform (BWT) kết hợp Huffman coding. Có tỷ lệ nén tốt hơn gzip, nhưng tốc độ nén/giải nén có thể chậm hơn. **Magic number:** bắt đầu bằng `42 5A` (chữ "BZ").

|Lệnh|Ý nghĩa|
|-|-|
|`bzip2 file`|Nén file.|
|`bzip2 -k file`|Nén và giữ file gốc.|
|`bunzip2 file.bz2` hoặc `bzip2 -d file.bz2`|Giải nén.|

### `tar`

Công cụ để gom nhiều file/thư mục lại thành một gói duy nhất (archive).

|Option|Ý nghĩa|
|-|-|
|`-c` (create)|Tạo gói mới.|
|`-x` (extract)|Giải nén gói.|
|`-f` (file)|Chỉ định tên file archive. **Luôn phải có** `-f` để chỉ định tên file.|
|`-v` (verbose)|Hiển thị chi tiết quá trình.|
|`-z` (gzip)|Nén/giải nén bằng gzip.|
|`-j` (bzip2)|Nén/giải nén bằng bzip2.|
|`-J` (xz)|Nén/giải nén bằng xz.|

```bash
tar -cjvf archive.tar.bz2 dir/     # nén nhiều file cùng 1 lúc thành .tar.bz2
tar -xjvf archive.tar.bz2          # giải nén gói .tar.bz2
tar -xf data                       # giải nén (tự nhận dạng loại nén)
```

### Quy trình giải một file nén nhiều lớp (từ Bandit)

1. `file <tên>` để xem file thuộc loại gì.
2. Đổi đuôi cho đúng (`mv data data.gz`) rồi giải nén bằng công cụ tương ứng (`gzip -d`, `bzip2 -d`, `tar -xf`).
3. Lặp lại cho đến khi `file` báo là `ASCII text`.

### `md5sum` **\[+]**

Tính mã băm MD5 của file hoặc chuỗi: `md5sum file.txt`, `md5sum <<<"chuỗi"`.

\---

## 9\. Phần mềm, tiến trình, dịch vụ và log

### Cài đặt phần mềm với `apt` **\[+]** (Debian/Ubuntu/Kali)

|Lệnh|Ý nghĩa|
|-|-|
|`sudo apt update`|Cập nhật danh sách gói.|
|`sudo apt upgrade`|Nâng cấp các gói đã cài.|
|`sudo apt install <gói>`|Cài đặt (ví dụ `sudo apt install dirb`).|
|`sudo apt remove <gói>`|Gỡ cài đặt.|
|`apt search <từ khóa>`|Tìm gói.|

### Tiến trình chạy ngầm

|Lệnh|Ý nghĩa|
|-|-|
|`lệnh \&`|Dấu `\&` đưa tiến trình xuống chạy ngầm (background), hiện ví dụ `\[1] 217`.|
|`jobs`|Liệt kê các tiến trình đang chạy ngầm trong session hiện tại.|
|`fg %1`|Đưa tiến trình số 1 từ background trở lại foreground.|
|`bg %1`|Tiếp tục chạy tiến trình số 1 ở background nếu nó đang bị tạm dừng.|
|`kill %1` hoặc `kill PID`|Dừng tiến trình (hữu ích nếu mở nhầm port hoặc port bị chiếm).|
|`ps aux` **\[+]**|Liệt kê tất cả tiến trình đang chạy (kèm PID).|
|`top` **\[+]**|Theo dõi tiến trình và mức dùng CPU/RAM theo thời gian thực (thoát bằng `q`).|
|`kill -9 PID` **\[+]**|Buộc dừng tiến trình không chịu thoát.|

### `tmux`

Terminal multiplexer, giống `screen` nhưng mạnh mẽ và tiện hơn. Cho phép mở nhiều "cửa sổ" trong cùng một phiên SSH, giữ tiến trình chạy khi thoát ra, và quản lý bằng phím tắt.

* **Quản lý nhiều phiên:** mở nhiều cửa sổ, chia màn hình ngang/dọc.
* **Giữ tiến trình chạy:** detach rồi reattach lại sau khi thoát SSH.
* **Tùy chỉnh mạnh:** cấu hình màu sắc, phím tắt, thanh trạng thái.

|Lệnh|Ý nghĩa|
|-|-|
|`tmux`|Khởi tạo phiên mới.|
|`tmux new -s mysession`|Khởi tạo phiên và đặt tên.|
|`Ctrl + B`, sau đó `D`|Thoát tạm (detach). Phiên vẫn chạy ngầm.|
|`tmux ls`|Liệt kê phiên đang chạy.|
|`tmux attach -t mysession`|Quay lại phiên.|
|`exit` (trong phiên) hoặc `Ctrl + B` rồi `:` và nhập `kill-session`|Thoát hẳn.|

### Dịch vụ và log **\[+]**

|Lệnh|Ý nghĩa|
|-|-|
|`systemctl status ssh`|Xem trạng thái một dịch vụ.|
|`sudo systemctl start ssh` / `stop` / `restart`|Bật / tắt / khởi động lại dịch vụ.|
|`sudo systemctl enable ssh` / `disable`|Cho dịch vụ tự chạy / không tự chạy khi khởi động máy.|
|`journalctl -u ssh`|Xem log của một dịch vụ.|
|`journalctl -xe`|Xem các log lỗi gần nhất.|
|`tail -f /var/log/auth.log`|Theo dõi log đăng nhập theo thời gian thực (Debian/Ubuntu).|

Khi đọc log, kết hợp `grep`, `sort`, `uniq -c` để đếm số lần xuất hiện của một dòng (ví dụ tìm IP xuất hiện nhiều nhất).

\---

## 10\. Mạng

### `ssh`

SSH dùng cặp khóa: **public key** được lưu trên server (trong `\~/.ssh/authorized\_keys`), **private key** giữ ở máy bạn. Khi đăng nhập bằng private key, server dùng public key tương ứng để xác minh bạn nếu đúng thì cho đăng nhập thay thế cho mật khẩu.

Cấu trúc: `ssh \[option] user@host \[lệnh]`

|Thành phần|Ý nghĩa|
|-|-|
|`-p`|Chọn cổng của server.|
|`-i`|Chỉ định private key.|
|`-l`|Chỉ định user (tương ứng với `user@`).|
|`-J host`|Nhảy qua máy trung gian.|
|`-4` / `-6`|Chỉ định IPv4 hoặc IPv6.|
|`-A` / `-a`|Bật / tắt chuyển tiếp ssh-agent.|
|`lệnh` (tùy chọn)|Chạy lệnh trên máy từ xa rồi thoát.|

```bash
ssh -p 2220 bandit0@bandit.labs.overthewire.org
ssh -p 2220 -i sshkey.private bandit14@bandit.labs.overthewire.org
ssh -p 2220 bandit18@bandit.labs.overthewire.org cat /home/bandit18/readme
```

### `scp`

Viết tắt của Secure Copy Protocol, dùng để sao chép file hoặc thư mục giữa máy cục bộ và máy chủ từ xa qua kết nối SSH. Giống lệnh `cp`, nhưng hoạt động qua mạng và được mã hóa nhờ SSH.

```bash
scp localfile user@hostname:/path/to/destination        # từ máy mình lên server
scp user@hostname:/path/to/file localfile               # từ server về máy mình
scp -r localdir user@hostname:/path/to/destination      # sao chép cả thư mục
scp -P 2220 user@host:/path/file ./file                 # chỉ định cổng: chữ P hoa
```

### `nc` (netcat)

Công cụ dòng lệnh được gọi là "dao Swiss Army của mạng" vì có nhiều chức năng liên quan đến kết nối TCP/UDP: tạo kết nối TCP/UDP tới một host/port, lắng nghe (listen) trên một port, truyền file qua mạng, kiểm tra dịch vụ mạng (giống telnet nhưng mạnh hơn).

|Tùy chọn|Ý nghĩa|
|-|-|
|`-l`|Listen (lắng nghe)|
|`-p`|Chỉ định port|
|`-v`|Verbose (hiển thị chi tiết)|
|`-z`|Chế độ scan, không gửi dữ liệu|
|`-u`|Dùng UDP thay vì TCP|

```bash
echo "mật\_khẩu" | nc localhost 30000
```

### `ncat`

Phiên bản nâng cấp của `nc`, phát triển bởi dự án Nmap. Giữ nguyên sự linh hoạt của `nc` nhưng bổ sung nhiều tính năng hiện đại và bảo mật hơn.

**Khác biệt chính giữa nc và ncat**

* **Bảo mật:** `ncat` hỗ trợ SSL/TLS để mã hóa dữ liệu, trong khi `nc` chỉ truyền dữ liệu thô.
* **Tính năng nâng cao:** có thể hoạt động như proxy, relay, hoặc hỗ trợ nhiều kết nối cùng lúc.
* **Tích hợp với Nmap:** đi kèm trong bộ công cụ Nmap, thường dùng để kiểm thử bảo mật.
* **Tương thích:** vẫn hỗ trợ cú pháp cơ bản giống `nc`.

Cấu trúc: `ncat \[tùy chọn] <host> <port>`

```bash
echo 'pass' | ncat --ssl localhost 30001                       # gửi dữ liệu qua SSL/TLS
ncat --ssl -l -p 1234                                          # lắng nghe với SSL
ncat --proxy-type http --proxy 127.0.0.1:8080 example.com 80   # dùng như proxy
ncat -l -p 2026 -k                                             # -k (keep-alive): giữ lắng nghe không bị ngắt giữa chừng
```

### `socat`

Công cụ rất mạnh, thường được gọi là "Swiss Army Knife" của kết nối mạng (SOcket CAT). Giống một phiên bản nâng cấp của netcat/ncat, nhưng linh hoạt hơn vì có thể kết nối mọi loại socket với nhau.

* **Chức năng:** tạo cầu nối (relay) giữa hai luồng dữ liệu (streams).
* **Điểm mạnh:** hỗ trợ nhiều loại socket (TCP, UDP, UNIX domain socket, SSL, file, pipe...).
* **Ứng dụng:** debug mạng, tạo proxy, chuyển tiếp port, kết nối bảo mật, hoặc giả lập thiết bị.

Cú pháp: `socat <điểm đầu A> <điểm đầu B>` → mọi dữ liệu từ A sẽ được chuyển sang B và ngược lại.

```bash
socat TCP-LISTEN:8080,fork TCP:example.com:80    # proxy đơn giản: kết nối tới 8080 được chuyển tiếp tới example.com:80
socat - OPENSSL:localhost:30001                  # kết nối SSL
socat - OPENSSL:host:port,verify=0               # bỏ qua kiểm tra chứng chỉ
```

* `-` nghĩa là đầu vào/ra từ stdin/stdout (bàn phím và màn hình).
* `OPENSSL:localhost:30001` tạo kết nối SSL tới localhost port 30001.
* `verify=0` bỏ qua việc kiểm tra chứng chỉ (vì server dùng self-signed certificate, nếu không bỏ qua sẽ báo lỗi).

### `telnet`

Công cụ dòng lệnh cổ điển dùng để kết nối tới một host qua giao thức Telnet (TCP/IP), mở một phiên làm việc với máy chủ từ xa qua port nhất định. Cú pháp: `telnet host port`

* Telnet **không mã hóa** dữ liệu, nên không an toàn khi dùng trên mạng thật.
* Ngày nay hầu như chỉ dùng để test port hoặc trong các bài học bảo mật.
* Để kết nối an toàn, người ta dùng SSH thay thế.

### `openssl`

Bộ công cụ và thư viện mã nguồn mở rất quan trọng, dùng để thực hiện các thao tác liên quan đến mã hóa, chứng chỉ và bảo mật (hỗ trợ SSL/TLS và nhiều thuật toán mật mã).

```bash
openssl s\_client -connect host:port                                  # kết nối và xem quá trình bắt tay TLS
openssl s\_client -connect example.com:443                            # xem thông tin chứng chỉ SSL của website
openssl genrsa -out private.key 2048                                 # tạo khóa riêng RSA 2048-bit
openssl req -new -x509 -key private.key -out cert.pem -days 365      # tạo chứng chỉ tự ký (self-signed) để thử nghiệm
openssl enc -aes-256-cbc -in file.txt -out file.enc                  # mã hóa file
openssl enc -d -aes-256-cbc -in file.enc -out file.txt               # giải mã file
```

### `nmap`

Công cụ cực kỳ mạnh để quét mạng và port: kiểm tra các dịch vụ đang chạy trên một máy chủ, phát hiện hệ điều hành, và thậm chí tìm lỗ hổng bảo mật. Cú pháp: `nmap \[options] target`

|Tùy chọn|Ý nghĩa|
|-|-|
|`-p`|Chỉ port hoặc dãy port|
|`-p-`|Quét tất cả port|
|`-sV`|Phát hiện phiên bản dịch vụ|
|`-O`|Phát hiện hệ điều hành|
|`-A`|Quét nâng cao (dịch vụ + OS + script)|
|`-T4`|Tăng tốc độ quét|
|`-Pn`|Bỏ qua ping, quét trực tiếp port|
|`-sC`|Chạy script mặc định (tương đương `--script=default`), chỉ muốn có thông tin cơ bản|
|`--script=ssl-cert`|Chạy script cụ thể để xem chứng chỉ SSL|

```bash
nmap -sV -p 31000-32000 --script=ssl-cert localhost
```

### `netstat` và `ss`

* **`netstat`** hiển thị thông tin về các kết nối mạng và các cổng đang mở trên máy: danh sách kết nối TCP/UDP, cổng đang lắng nghe, bảng định tuyến (routing table) và thống kê giao thức (số gói TCP, UDP, ICMP đã gửi/nhận).
* **`ss`** (socket statistics) là công cụ hiện đại, được coi là "người kế thừa" của `netstat` (`netstat` thuộc gói `net-tools` đã cũ và không còn được cài mặc định trên nhiều hệ thống mới).

**Ưu điểm của `ss`:** nhanh hơn (truy vấn trực tiếp từ kernel), đầy đủ hơn (nhiều thông tin chi tiết về socket, trạng thái kết nối), có sẵn trên hầu hết Linux hiện đại.

Xem các cổng đang lắng nghe: `ss -tuln`

|Cờ|Ý nghĩa|
|-|-|
|`t`|TCP|
|`u`|UDP|
|`l`|listening (đang lắng nghe)|
|`n`|hiển thị số port thay vì tên dịch vụ|

### `dirb`

Dùng để quét và phát hiện các thư mục hoặc file ẩn trên web server. Hoạt động bằng cách gửi các yêu cầu HTTP dựa trên wordlist (`/usr/share/dirb/wordlists/`) và phân tích phản hồi để tìm ra tài nguyên có thể truy cập.

Cách cài: `sudo apt install dirb`

> Chỉ quét các hệ thống bạn sở hữu hoặc được phép kiểm thử (lab của mình, môi trường học tập).

### Các lệnh mạng cơ bản khác **\[+]**

|Lệnh|Ý nghĩa|
|-|-|
|`ping -c 4 <host>`|Kiểm tra kết nối tới host (gửi 4 gói rồi dừng).|
|`ip a`|Xem địa chỉ IP của các card mạng.|
|`ip route`|Xem bảng định tuyến.|
|`curl -I <url>`|Xem phần header HTTP của một trang.|
|`curl <url>`|Tải nội dung trang ra màn hình.|
|`wget <url>`|Tải file về máy.|

\---

## 11\. Tự động hóa: file script và cron

### Viết và chạy một file Bash

1. Tạo file: `nano filename.sh`. Dòng đầu tiên là `#!/bin/bash` để báo cho hệ thống biết đây là file bash.
2. Cấp quyền thực thi: `chmod +x filename.sh`
3. Chạy: `./filename.sh`

`ls -ld filename.sh` để kiểm tra quyền của file.

### Cron job

Cron job là 1 chương trình giúp bạn làm những công việc lặp đi lặp lại vào một khoảng thời gian cố định. Ví dụ bình thường cứ sau mỗi 3 ngày bạn phải xóa các file tmp để giải phóng bộ nhớ, thay vì tự làm thì bạn setup cho cron cứ sau 3 ngày thì tự động xóa file tmp.

File cron là file text đơn giản chứa các lệnh được chạy trong thời gian cụ thể. File crontab mặc định trong hệ thống là `/etc/crontab` và nằm trong thư mục crontab `/etc/cron.\*/`. Chỉ quản trị viên hệ thống mới có thể chỉnh sửa file crontab trên hệ thống.

**Cấu tạo:**

```
\* \* \* \* \* command
```

|Vị trí|Ý nghĩa|Giá trị|
|-|-|-|
|1|phút|0–59|
|2|giờ|0–23|
|3|ngày|1–31|
|4|tháng|1–12|
|5|thứ|0–7 (0 và 7 là CN, 1 là T2, ... 6 là T7)|

**Cú pháp chi tiết:**

|Ký tự|Ý nghĩa|
|-|-|
|`\*`|Tất cả tham số (VD dấu `\*` ở vị trí đầu nghĩa là chạy mỗi phút từ 0 đến 59)|
|`,`|Ngăn cách các giá trị cụ thể (VD vị trí thứ 2 là `1,10,20` thì chạy vào lúc 1h, 10h và 20h)|
|`-`|Một khoảng liên tục (VD vị trí thứ 3 là `2-5` nghĩa là chạy vào các ngày 2, 3, 4, 5)|
|`/`|Khoảng nghỉ (VD vị trí thứ 4 là `\*/2` là cứ mỗi 2 tháng chạy 1 lần)|
|`L`|Cuối cùng. Ví dụ `3L` nghĩa là thứ tư cuối cùng của tháng. (Chỉ một số bản cron hỗ trợ.)|

**Phím tắt nhanh (shortcuts):**

|Shortcut|Ý nghĩa|
|-|-|
|`@reboot`|Chạy một lần duy nhất ngay khi hệ thống vừa khởi động.|
|`@hourly`|Chạy vào phút đầu tiên của mỗi giờ (tương đương `0 \* \* \* \*`).|
|`@daily` hoặc `@midnight`|Chạy vào lúc 00:00 mỗi ngày (tương đương `0 0 \* \* \*`).|
|`@weekly`|Chạy vào lúc 00:00 ngày Chủ Nhật hàng tuần (tương đương `0 0 \* \* 0`).|
|`@monthly`|Chạy vào lúc 00:00 ngày đầu tiên của mỗi tháng (tương đương `0 0 1 \* \*`).|
|`@yearly`|Chạy vào lúc 00:00 ngày 1 tháng 1 hàng năm (tương đương `0 0 1 1 \*`).|

**Lệnh quản lý:**

|Lệnh|Ý nghĩa|
|-|-|
|`crontab -e`|Mở trình chỉnh sửa để thêm, sửa hoặc xóa các cron job.|
|`crontab -l`|Xem danh sách các cron job hiện có của bạn.|
|`crontab -r`|Xóa **toàn bộ** các cấu hình cron job hiện có.|

\---

## 12\. Bash scripting

### Biến

* Biến trong bash mặc định là kiểu chuỗi và không phân biệt được số thực hay số nguyên.
* Khai báo: `a="Hello"` (lưu ý **không được có khoảng trắng** quanh dấu `=`).
* Sử dụng biến bằng dấu `$`: `$a`.
* Trong dấu `""` bash hiểu `$a` là biến: `echo "$a Linux"` → `Hello Linux`.
* Trong dấu `''` bash không hiểu `$a` là biến: `echo '$a Linux'` → `$a Linux`.

**Biến đặc biệt**

|Biến|Ý nghĩa|
|-|-|
|`$0`|Tên/đường dẫn của chính chương trình hay shell đang thực thi (ví dụ `/bin/bash`).|
|`$1`, `$2`, `$3`...|Các tham số bạn truyền vào câu lệnh (Positional Parameters).|

### Tính toán

* Tính toán số nguyên cơ bản: dùng ngoặc tròn đôi `$(( x + y ))`.
* Trong `(( ))` không cần thêm `$` trước biến và không cần quan tâm khoảng trắng. Có thể dùng các toán tử so sánh `==`, `!=`, `<`, `>`, `>=`, `<=` mà không cần `-eq`, `-lt`, `-gt`.
* Có 2 cách lấy giá trị sau khi tính:

  1. `a=$((3+5))` thêm dấu `$` để lấy giá trị tính ra ngoài.
  2. `((a = 3+5))` cho biến nhận giá trị vào bên trong luôn.
* `$(...)` là **command substitution** (thay thế lệnh): chạy lệnh bên trong rồi thay bằng kết quả đầu ra, ví dụ `today=$(date)`.
* Dấu `( )` tạo ra 1 vùng không gian riêng biệt (**subshell**): mọi kết quả, tính toán trong đây sẽ không ảnh hưởng đến các biến bên ngoài. Để lấy giá trị ra ngoài có thể thêm `$` phía trước để lấy kết quả, hoặc đưa kết quả vào file `/tmp` rồi lấy ra.

### `bc` (tính toán số thập phân)

`bc` (An Arbitrary Precision Calculator Language) là công cụ tính toán toán học mạnh mẽ. Điểm khác so với cơ chế mặc định của Bash là nó hỗ trợ số thập phân (dấu phẩy động) và độ chính xác tùy biến.

|Tham số|Ý nghĩa \& cách dùng|
|-|-|
|`-l` (`--mathlib`)|Quan trọng nhất. Gọi thư viện toán học nâng cao: tự động đặt số chữ số thập phân (`scale`) thành 20, đồng thời cho phép dùng các hàm lượng giác, logarit, hàm mũ.|
|`-q` (`--quiet`)|Bỏ qua lời chào bản quyền khi khởi động bc ở chế độ tương tác.|
|`-s` (`--standard`)|Chỉ chạy các tính năng chuẩn POSIX bc, cảnh báo hoặc từ chối các tính năng mở rộng của GNU bc.|
|`-w` (`--warn`)|Cảnh báo nếu script dùng các tính năng mở rộng của GNU bc thay vì chuẩn POSIX.|
|`-h` (`--help`)|Hiển thị hướng dẫn sử dụng nhanh.|

Các hàm khi chạy `bc -l` (tên hàm chỉ gồm 1 ký tự):

* `s(x)`: Sine của x (x tính bằng radian).
* `c(x)`: Cosine của x (radian).
* `a(x)`: Arctangent (trả về radian).
* `l(x)`: Logarit tự nhiên (ln x).
* `e(x)`: Hàm mũ của e (e^x).
* `j(n, x)`: Hàm Bessel bậc nguyên n.
* `sqrt(x)`: Căn bậc hai (có sẵn, không cần `-l`, nhưng cần chỉnh `scale` để thấy số thập phân).

Ví dụ: `echo "scale=3; 10/3" | bc` hoặc `echo "sqrt(2)" | bc -l`

### `printf` (in có định dạng)

`printf` in văn bản ra màn hình có định dạng, hoạt động tương tự hàm `printf` trong ngôn ngữ C. `echo` đơn giản nhưng bị hạn chế khi cần căn chỉnh cột, định dạng số thập phân, hoặc in các ký tự đặc biệt.

|Ký hiệu|Ý nghĩa|
|-|-|
|`%s`|Chuỗi văn bản (String)|
|`%d` hoặc `%i`|Số nguyên có dấu (Integer)|
|`%f`|Số thập phân (Float)|
|`\\n`|Xuống dòng (New line)|
|`\\t`|Dấu Tab (thụt lề)|

Ví dụ: `printf "%s có %d tuổi\\n" "Nam" 20`

### Điều kiện: `\[ ]`, `\[\[ ]]`, `if`

**`\[ ]`** là câu lệnh so sánh điều kiện nguyên bản trong bash (thật chất là cách viết tắt của lệnh `test`).

* Trong `\[ ]` **bắt buộc phải có khoảng trắng** ở 2 đầu phía trong. Ví dụ: `\[ "$a" == "$b" ]` (đúng), `\[$a==$b]` (sai).
* Bắt buộc bọc biến trong dấu `""` để tránh lỗi nếu biến bị bỏ trống (Null) hoặc chứa khoảng trắng.

|Toán tử|Ý nghĩa|
|-|-|
|`-eq`|Bằng (`==`)|
|`-ne`|Khác (`!=`)|
|`-gt`|Lớn hơn (`>`)|
|`-ge`|Lớn hơn hoặc bằng (`>=`)|
|`-lt`|Nhỏ hơn (`<`)|
|`-le`|Nhỏ hơn hoặc bằng (`<=`)|
|`-a`|AND|
|`-o`|OR|

**`\[\[ ]]`** là tính năng nâng cao của bash (không thuộc POSIX chuẩn), sinh ra để khắc phục các nhược điểm của `\[ ]`:

* Không quan tâm đến khoảng trắng trong biến, không cần bọc biến trong `""`.
* Dùng được các toán tử so sánh thông thường như `\&\&` (AND), `||` (OR).
* Hỗ trợ tìm kiếm theo mẫu (wildcard): dùng `==` với ký tự đại diện `\*`.
* Hỗ trợ biểu thức chính quy (Regex): dùng toán tử `=\~` để kiểm tra định dạng chuỗi phức tạp.

**Các toán tử kiểm tra file và chuỗi thông dụng** (dùng được cho cả `\[ ]` và `\[\[ ]]`):

|Toán tử|Ý nghĩa|
|-|-|
|`-f $path`|Đường dẫn đó có phải là một File tồn tại hay không.|
|`-d $path`|Đường dẫn đó có phải là một Thư mục tồn tại hay không.|
|`-z $string`|Chuỗi đó có rỗng (độ dài bằng 0) hay không.|
|`-n $string`|Chuỗi đó có dữ liệu (độ dài lớn hơn 0) hay không.|

**Cấu trúc `if`:**

```bash
if \[ điều\_kiện ]; then
    lệnh
elif \[ điều\_kiện\_khác ]; then
    lệnh
else
    lệnh
fi
```

### Vòng lặp

```bash
# while
while \[ điều\_kiện ]
do
    câu\_lệnh
done

# for với dãy số: {bắt\_đầu..kết\_thúc..bước\_nhảy}
for i in {1..99..2}
do
    echo $i
done
```

* `1` là bắt đầu, `99` là kết thúc, `2` là bước nhảy.
* `seq \[OPTION] \[từ] \[đến]`: lệnh sinh ra dãy số liên tiếp. Tùy chọn `-w` ("width") giữ độ rộng cố định bằng cách thêm số 0 ở đầu (ví dụ `seq -w 1 10`).

### Đọc dữ liệu: `read`, `mapfile`, `IFS`

|Lệnh|Ý nghĩa|
|-|-|
|`read`|Đọc đúng 1 dòng từ bên ngoài (stdin) vào.|
|`read -a ten\_mang`|Đọc dữ liệu vào một mảng.|
|`mapfile`|Đọc tất cả các dòng từ stdin vào.|
|`mapfile -t`|Bỏ qua ký tự `\\n` ở cuối mỗi dòng.|
|`mapfile -n <số lượng>`|Đọc đúng số lượng dòng.|
|`mapfile -s <số lượng>`|Bỏ qua số lượng dòng rồi đọc tiếp.|
|`mapfile -O <chỉ số>`|Quy định chỉ số bắt đầu ghi vào array (mặc định bắt đầu ghi từ chỉ số 0).|
|`IFS`|Đổi ký tự phân chia giữa các phần tử.|

```bash
# Giả sử bạn có một chuỗi định dạng CSV
du\_lieu="Nam,26,Hà Nội,Kỹ sư"

# Đổi IFS thành dấu phẩy trước khi read
IFS=',' read -a thong\_tin <<< "$du\_lieu"
```

### Mảng (array)

Bash hỗ trợ hai loại mảng chính:

1. **Mảng chỉ số (Indexed Array):** các phần tử được truy cập bằng số nguyên, bắt đầu từ số 0.
2. **Mảng liên hợp (Associative Array):** các phần tử được truy cập bằng một "từ khóa" (key/string), giống Dictionary trong Python hay Map trong C++ (hỗ trợ từ Bash 4.0 trở lên).

```bash
# Cách 1: Khai báo trực tiếp một danh sách
he\_dieu\_hanh=("Ubuntu" "Debian" "Fedora" "CentOS")

# Cách 2: Khai báo tường minh bằng lệnh declare (tùy chọn)
declare -a trai\_cay
trai\_cay\[0]="Táo"
trai\_cay\[1]="Chuối"

# Cách 3: Thêm phần tử vào mảng đã có (Append)
he\_dieu\_hanh+=("ArchLinux")

# Lấy phần tử đầu tiên (vị trí 0)
echo "${he\_dieu\_hanh\[0]}"       # Kết quả: Ubuntu

# Lấy TẤT CẢ các phần tử trong mảng (Dùng dấu \* hoặc @)
echo "${he\_dieu\_hanh\[@]}"       # Kết quả: Ubuntu Debian Fedora CentOS ArchLinux

# Đếm SỐ LƯỢNG phần tử có trong mảng (Thêm dấu # phía trước)
echo "${#he\_dieu\_hanh\[@]}"      # Kết quả: 5

# Lấy danh sách các CHỈ SỐ (Index) của mảng (Thêm dấu ! phía trước)
echo "${!he\_dieu\_hanh\[@]}"      # Kết quả: 0 1 2 3 4
```

**Mảng liên hợp:**

```bash
# 1. Khai báo mảng liên hợp
declare -A thong\_tin

# 2. Gán giá trị theo Key
thong\_tin\[ten]="Nguyễn Văn A"
thong\_tin\[tuoi]=25
thong\_tin\[thanh\_pho]="Hà Nội"

# 3. Truy cập dữ liệu bằng Key
echo "${thong\_tin\[ten]} năm nay ${thong\_tin\[tuoi]} tuổi."
# Kết quả: Nguyễn Văn A năm nay 25 tuổi.

# 4. Lấy tất cả các Key hiện có
echo "${!thong\_tin\[@]}"
# Kết quả: tuoi ten thanh\_pho
```

**Duyệt qua mảng:**

```bash
server\_list=("192.168.1.1" "192.168.1.2" "192.168.1.3")

# Duyệt qua từng IP trong mảng
for ip in "${server\_list\[@]}"; do
    echo "Đang kiểm tra kết nối tới IP: $ip"
    # Bạn có thể bỏ lệnh ping vào đây, ví dụ: ping -c 1 "$ip"
done
```

Xóa một phần tử hoặc xóa cả mảng: `unset ten\_mang\[1]` hoặc `unset ten\_mang`.

### Ví dụ tổng hợp **\[+]**

Script kiểm tra danh sách máy còn sống (dùng mảng, vòng lặp, điều kiện):

```bash
#!/bin/bash
server\_list=("192.168.1.1" "192.168.1.2" "192.168.1.3")

for ip in "${server\_list\[@]}"; do
    if ping -c 1 -W 1 "$ip" > /dev/null 2>\&1; then
        echo "$ip: online"
    else
        echo "$ip: offline"
    fi
done
```

\---

## 13\. Thủ thuật: thoát shell bị hạn chế

### Thoát qua `more` và `vi`

Khi shell đăng nhập không phải `/bin/bash` mà là một chương trình chỉ hiển thị văn bản:

1. `more` sẽ tự thoát khi xuất văn bản xong, nên thu nhỏ terminal lại để `more` chỉ hiển thị 1 phần, từ đó không tự thoát.
2. Trong lúc đó bấm `v` để mở `vi` ngay trong `more`.
3. Trong `vi`, nhập `:set shell=/bin/bash` rồi Enter, sau đó `:shell` để mở một shell con.

### Thoát khỏi "uppercase shell" bằng `$0`

Nếu một chương trình chuyển mọi lệnh bạn gõ thành chữ hoa, các lệnh sẽ không chạy được. Nhưng ký tự đặc biệt và số không bị in hoa, nên gõ `$0` vẫn nguyên vẹn.

1. **Bản chất của `$0`:** là một biến đặc biệt luôn chứa tên của chính chương trình/shell đang thực thi (ví dụ `/bin/sh` hoặc `/bin/bash`).
2. **Vì sao hữu ích:** chương trình "bẫy" chỉ thấy bạn gõ 2 ký tự `$` và `0`, không biến chữ cái nào thành chữ hoa. Khi lệnh `$0` được thực thi, máy hiểu là "người dùng muốn gọi chương trình `/bin/sh`", nên bạn nhận được một shell bình thường.

