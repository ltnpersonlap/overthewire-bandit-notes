# OverTheWire Bandit – Walkthrough (Level 0 → 25)

## Mục lục

- [Level 0](#level-0)
- [Level 0 → 1](#level-0--1)
- [Level 1 → 2](#level-1--2)
- [Level 2 → 3](#level-2--3)
- [Level 3 → 4](#level-3--4)
- [Level 4 → 5](#level-4--5)
- [Level 5 → 6](#level-5--6)
- [Level 6 → 7](#level-6--7)
- [Level 7 → 8](#level-7--8)
- [Level 8 → 9](#level-8--9)
- [Level 9 → 10](#level-9--10)
- [Level 10 → 11](#level-10--11)
- [Level 11 → 12](#level-11--12)
- [Level 12 → 13](#level-12--13)
- [Level 13 → 14](#level-13--14)
- [Level 14 → 15](#level-14--15)
- [Level 15 → 16](#level-15--16)
- [Level 16 → 17](#level-16--17)
- [Level 17 → 18](#level-17--18)
- [Level 18 → 19](#level-18--19)
- [Level 19 → 20](#level-19--20)
- [Level 20 → 21](#level-20--21)
- [Level 21 → 22](#level-21--22)
- [Level 22 → 23](#level-22--23)
- [Level 23 → 24](#level-23--24)
- [Level 24 → 25](#level-24--25)

---

## Level 0

**Goal:** Kết nối với máy chủ overthewire qua ssh

```bash
ssh -p 2220 bandit0@bandit.labs.overthewire.org
```

Cấu trúc lệnh ssh: `ssh option user@host lệnh`

Các thành phần gồm:

- `ssh`: chương trình client SSH
- Các tùy chọn (option) hay dùng:
  - `-p`: chọn cổng của server
  - `-i`: chỉ định private key
  - `-l`: chỉ định user tương ứng với user@
  - `-J host`: nhảy qua máy trung gian
  - `-4` / `-6`: chỉ định ipv4 hoặc ipv6
  - `-A`: chuyển tiếp ssh-agent sang máy từ xa
  - `-a`: tắt chuyển tiếp ssh-agent
- `user@`: chỉ định user
- `host`: địa chỉ máy từ xa (IP hoặc tên miền)
- `lệnh` (tùy chọn): Lệnh chạy trên máy từ xa rồi thoát

> **Lưu ý:** Nếu có lệnh ở cuối thì nó sẽ chạy lệnh ở server rồi đúng phiên làm việc ngay.

---

## Level 0 → 1

**Goal:** tìm mật khẩu cho level 1 nằm trong file readme

**Step:**

1. Dùng lệnh `ls` để xem các file trong thư mục hiện hành, có thể thêm option `-a` để không bỏ sót các file ẩn
2. Đọc nội dung:

   ```bash
   cat readme
   ```

> **Lưu ý:** nên dùng lệnh `cat` hoặc `less` để đọc file hơn là dùng `nano` hoặc `vim` để tránh vô tình làm thay đổi metadata/nội dung file

---

## Level 1 → 2

**Goal:** đọc file chứa tên duy nhất là `-`.

**Problem:** lệnh `cat -` thất bại vì linux cli hiểu `-` là tham số

Để đọc file có tên là "-" ta dùng các cách sau:

| Cách | Mô tả | Ví dụ |
|------|-------|-------|
| C1 | thêm `./` trước tên "-" | `cat ./-` |
| C2 | thêm `--` trước tên "-" | `cat -- -` |
| C3 | dùng đường đẫn tuyệt đối | `cat /home/user/-` |
| C4 | Dùng chuyển hướng đầu vào | `cat < ./-` |

---

## Level 2 → 3

**Goal:** đọc file có tên `--spaces in this filename--`

Để đọc các file vừa chứa khoản trắng vừa chứa `--` ở đầu, ta sẽ kết hợp `./` đọc file chứ `-` ở level 1 và bọc tên file trong dấu `''` hoặc `""`

```bash
cat ./ "--spaces in this filename--"
```

---

## Level 3 → 4

**Goal:** tìm file chứa mật khẩu trong `inhere`

**Step:**

1. dùng lệnh: `file inhere` để xác định đây là file hay thư mục
2. dùng lệnh: `cd inhere` để vào folder inhere
3. dùng lệnh: `ls -a` để tìm các file ẩn
4. dùng `cat "...Hiding-From-You"` để đọc mật khẩu

> **Lưu ý:** các file bắt đầu bằng dấu "." thường là file ẩn nên dùng `ls` không đọc được. Cần thêm tham số `a` (all) để đọc tất cả file

---

## Level 4 → 5

**Goal:** tìm file có để đọc được trong `inhere`

**Problem:** trong thư mục inhere có rất nhiều file (`-file00` đến `-file09`) thay vì kiểm tra từng file, ta dùng `*` (wildcard) đại diện cho tất cả các file có trong thư mục hiện tại.

```bash
file ./*
```

sau khi tìm được `-file07: ASCII text` thì ta chỉ cần dùng: `cat ./-file07` để đọc file

> **Lưu ý:** vì tên file bắt đầu bằng dấu `-` nên ta phải thêm `./` để linux cli không hiểu nhầm thành tham số

**Nâng cao:** trong thực tế số file có thể lên đến hàng trăng file nên ta có thể dùng lệnh:

```bash
file ./* | grep 'ASCII text'
```

để lọc ra đúng các file có định dạng ASCII text

---

## Level 5 → 6

**Goal:** Tìm file với yêu cầu sau:

- dễ đọc đối với con người
- Kích thước 1033 byte
- không thể thực thi

```bash
find . -type f -size 1033c -exec file {} \; | grep "ASCII text"
```

`find` là lệnh dùng để tìm kiếm. VD `find readme.txt` (tìm file readme.txt trong thư mục hiện tại và thư mục con của nó)

| Thành phần | Ý nghĩa |
|------------|---------|
| `find .` | duyệt toàn bộ thư mục hiện tại (`.`) và tất cả các thư mục con bên trong nó. |
| `-type f` | thể loại cần tìm (`f`: file hoặc `d`: directory) |
| `-size 1033c` | kích thước của file đó (`c` → byte; `k` → kilobytes (1024 bytes); `M` → megabytes; `G` → gigabytes) |
| `-exec file {} \;` | chạy lệnh sau khi tìm dược kết quả đó |

Trong `-exec file {} \;`:

- `file` là lệnh file
- `{}` nghĩa là find sẽ thay thế đường dẫn tới file đó vào `{}`
- `\;` (hoặc `+`) là kết thúc lệnh trong `-exec`

ngoài ra ta còn có thể dùng lệnh

```bash
find . -type f -size 1033c ! -executable
```

`! -executable`: dấu `!` nghĩa là không (NOT) đoạn này nghĩa là file đó không có quyền thực thi (execute)

---

## Level 6 → 7

**Goal:** tìm file với yêu cầu:

- Được lưu trữ ở đâu đó trên máy chủ
- Thuộc sở hữu của người dùng bandit7
- thuộc sở hữu của nhóm bandit6
- Kích thước 33 byte

trong linux có 3 nhóm quyền: user, group và other

Khi kiểm tra với `ls -l`, cấu trúc phân quyền thể hiện rõ danh tính:

```
-rw-r----- 1 bandit7 bandit6 33 Sep 26 21:53 /var/lib/dpkg/info/bandit7.password
```

trong đó cột thứ 3 (`bandit7`) là user còn cột thứ 4 (`bandit6`) là group

Vậy để tìm được file có yêu cầu trên ta dùng lệnh:

```bash
find / -size 33c -exec ls -l {} \; | grep 'bandit7 bandit6'
```

- `find /` duyệt qua tất cả các thư mục trên máy
- `ls -l` là để xem quyền người dùng (user) và nhóm người dùng (group)

**Cách tối ưu hơn**

```bash
find / -type f -user bandit7 -group bandit6 -size 33c 2>/dev/null
```

Trong file đã có sẵn 2 thuộc tính user và group

Khi chạy lệnh `find /` nó sẽ duyệt qua toàn bộ hệ thống bao gồm cả thư mục bảo mật, thư mục root. Nó sẽ hiện lên màn hình các dòng báo lỗi như `Permission denied`

Để tránh rác màn hình ta sẽ chuyển hướng luồng lỗi (Standard Error - 2) về cống rác `/dev/null` bằng cú pháp `2>/dev/null`

Trong Linux, mỗi chương trình có ba "luồng" chính:

| Số | Tên | Ý nghĩa |
|----|-----|---------|
| 0 | stdin | đầu vào. |
| 1 | stdout | đầu ra chuẩn (thường là màn hình). |
| 2 | stderr | đầu ra lỗi (các thông báo lỗi). |

**Cơ chế Chuyển hướng (Redirection Operators):**

- `>` lưu đầu ra chuẩn (stdout) vào 1 file.
  - Nếu file chưa tạo thì tạo file mới
  - nếu file đã được tạo thì xóa dữ liệu cũ lưu dữ liệu mới vào.
- `>>` Thêm đầu ra chuẩn (stdout) vào 1 file (không xóa dữ liệu cũ có trong file)
- `<` nghĩa là lấy dữ liệu trong file làm đầu vào (stdin)
- `2>` : Chỉ định chuyển hướng luồng lỗi stderr.
- `2>&1`: nghĩa là "chuyển hướng luồng lỗi (2) sang cùng nơi với luồng chuẩn (1)".
- (Trong Bash hiện đại có thể viết tắt `&> file.txt` để lưu cả stdout và stderr).
- `/dev/null` là một "hố đen" mọi thứ ghi vào đó sẽ bị xóa bỏ.

---

## Level 7 → 8

**Goal:** tìm mật khẩu nằm cạnh từ "millionth" trong file data.txt

```bash
grep "millionth" data.txt
```

`grep` tìm kiếm chuỗi hoặc mẫu (pattern) trong văn bản, file, hoặc đầu ra của lệnh khác.

Cấu trúc: `grep [OPTIONS] PATTERN [FILE...]`

**[OPTIONS]**

| Option | Ý nghĩa |
|--------|---------|
| `-i` | Không phân biệt hoa thường. |
| `-n` | Hiển thị số dòng chứa kết quả. |
| `-r` | Tìm kiếm đệ quy trong thư mục. |
| `-w` | Chỉ khớp toàn bộ từ. |
| `-l` | Chỉ hiển thị tên file có kết quả. |
| `-v` | Hiển thị các dòng không khớp. |
| `-F` | so khớp theo chuỗi cố định (không regex). |
| `-x` | khớp toàn bộ dòng. |
| `-f file1.txt` | dùng nội dung file file1.txt để làm mẫu so sánh. |
| `--color=auto` | Tô màu kết quả để dễ nhìn. |
| `-F` | Fixed strings (coi pattern là chuỗi thuần túy, bỏ qua cú pháp Regex để tối ưu tốc độ). |
| `-E` | Extended Regex (cho phép dùng các toán tử mở rộng như `\|`, `+`, `?`). |
| `-C <N>` | Hiển thị ngữ cảnh (Context) gồm N dòng trước và sau dòng kết quả. |

**[FILE...]**

`grep "main" *.c` tìm tất cả các file có đuôi .c

---

## Level 8 → 9

**Goal:** tìm dòng văn bản duy nhất chỉ xuất hiện một lần trong file data.txt

```bash
sort data.txt | uniq -u
```

### `sort`

`sort` trong Linux là lệnh dùng để sắp xếp các dòng văn bản từ file hoặc đầu vào chuẩn (stdin). Mặc định, nó sắp xếp theo thứ tự chữ cái tăng dần, nhưng có thể tùy chỉnh để sắp xếp theo số, đảo ngược, theo cột cụ thể, hoặc loại bỏ trùng lặp.

Một vài loạt sort phổ biến:

| Lệnh | Ý nghĩa |
|------|---------|
| `sort file.txt` | Sắp xếp theo thứ tự chữ cái. |
| `sort -r file.txt` | Sắp xếp ngược (descending). |
| `sort -n file.txt` | Sắp xếp theo số (numeric). |
| `sort -nr file.txt` | Sắp xếp số theo thứ tự giảm dần. |
| `sort -k2 file.txt` | Sắp xếp theo cột thứ 2. |
| `sort -t: -k3 -n /etc/passwd` | Sắp xếp theo UID (cột 3, phân cách bằng dấu `:`). |
| `sort -u file.txt` | Loại bỏ dòng trùng lặp sau khi sắp xếp. |
| `sort -h sizes.txt` | Sắp xếp theo kích thước dạng người đọc (K, M, G). |
| `sort -V versions.txt` | Sắp xếp theo phiên bản (ví dụ: 1.9 < 1.10). |

### `uniq`

\-uniq trong Linux là lệnh dùng để loại bỏ các dòng trùng lặp liên tiếp trong văn bản. Nó thường được dùng kết hợp với sort, vì uniq chỉ phát hiện được các dòng trùng nhau khi chúng nằm liền kề.

Cấu trúc: `uniq [OPTIONS] [INPUT [OUTPUT]]`

| Lệnh | Ý nghĩa |
|------|---------|
| `uniq file.txt` | Loại bỏ dòng trùng lặp liên tiếp. |
| `uniq -c file.txt` | Hiển thị số lần xuất hiện của mỗi dòng. |
| `uniq -d file.txt` | Chỉ hiển thị các dòng bị trùng lặp. |
| `uniq -u file.txt` | Chỉ hiển thị các dòng duy nhất (không trùng). |
| `uniq -i file.txt` | Bỏ qua phân biệt hoa thường khi so sánh. |

---

## Level 9 → 10

**Goal:** tìm chuỗi ký tự dễ đọc, đứng sau một vài ký tự `=` trong file data.txt

```bash
strings data.txt | grep -E '==+'
```

- `strings` kiểm tra các ký tự có thể đọc được trong file
- `-E '==+'` là toán tử mở rộng ý nghĩa là có từ 2 dấu bằng trở lên (tương đương với `={2,}`)

---

## Level 10 → 11

**Goal:** giải mã file data.txt đã được mã hóa bằng base64

```bash
base64 --decode data.txt
```

`base64` là phương thức convert dạng mã hóa 2 chiều từ binary sang string để có thể gửi đi được trong network một cách dễ dàng. Các binary lúc này sẽ được thể hiện bằng các ký tự mã ASCII .

**Cơ chế:** chuyển các ký tự ra bit nhị phân sau đó chia các bit nhị phân thành nhóm 6 bit. Nếu không đủ thì thêm các bit 0 sao cho đủ 6 bit. Sau đó dùng bản mã của base64 chuyển thành chữ cái tương ứng. Đoạn mã nào có thêm bit bổ xung ta sẽ bổ xung bằng đấu `=` để nhận biết.

Bản chất là chứ 24 bit là tạo ra được 4 ký tự. Nên trường hợp nếu đến cuối còn dư 1 byte (8bit) thì sẽ mượn thêm 4bit cho đủ 12bit để tạo ra 2 ký tự đồng thời mượn thêm 2 dấu bằng cho đúng quy luật 24bit là được 4 ký tự. Dư 2 byte (16 bit) mượn thêm 2bit 0 để tạo ra 3 ký tự thật thêm 1 dấu `=` cho đủ 4 ký tự

Cú pháp: `base64 [OPPTION] [filename]`

| OPPTION | Ý nghĩa |
|---------|---------|
| `-d` / `--decode` | Giải mã Base64 |
| `-i` / `--ignore-garbage` | Bỏ qua ký tự không hợp lệ khi giải mã |
| `-w 0` | Ngăn ngắt dòng khi mã hóa (giải mã không cần) |

---

## Level 11 → 12

**Goal:** Các ký tự trong file data.txt đã bị dịch chuyển đi 13 lần so với bản ký tự ban đầu, cần giải mã để lấy mật khẩu cho level tiếp theo.

```bash
cat data.txt | tr 'N-ZA-Mn-za-m' 'A-Za-z'
```

`tr` giúp xoay, nén, xóa các ký tự theo mong muốn

Cú pháp: `tr [tùy chọn] SET1 [SET2]`

- `SET1`: tập ký tự nguồn.
- `SET2`: tập ký tự đích (nếu dịch).

Tùy chọn:

| Option | Ý nghĩa |
|--------|---------|
| `-c` | đảo ngược tập ký tự (thay các ký tự đã xuất hiện trong chuỗi a bằng các ký tự không có trong chuỗi a). |
| `-d` | xóa ký tự. |
| `-s` | nén chuỗi ký tự lặp (VD aaa → a). |
| `-t` | cắt SET1 cho bằng SET2. |

---

## Level 12 → 13

**Goal:** tệp data.txt là bản sao hex của một tệp đã được nén nhiều lần hãy giải hết file nén để tìm ra mật khẩu.

### Thuật ngữ

- **hex dump:** là cách hiển thị nội dung của một file hoặc vùng nhớ dưới dạng mã hex (hệ 16), kèm theo phần biểu diễn ASCII để bạn dễ quan sát dữ liệu nhị phân. Nó thường dùng để kiểm tra dữ liệu thô, debug, hoặc phân tích file.
- **Magic number:** là một chuỗi byte đặc biệt nằm ở đầu file (header) dùng để nhận diện định dạng của file.
- **gzip (.gz)** là một công cụ nén dữ liệu. Nó dùng thuật toán DEFLATE để giảm kích thước file, giúp tiết kiệm dung lượng và tăng tốc độ truyền tải. Magic number của Gzip: bắt đầu bằng `1F 8B`
- **bzip2 (.bz2)** là 1 công cụ nén dữ liệu tương tự gzip nhưng dùng dùng thuật toán Burrows–Wheeler transform (BWT) kết hợp với Huffman coding. có tỷ lệ nén tốt hơn gzip, nhưng tốc độ nén/giải nén có thể chậm hơn. Magic number của Bzip2: bắt đầu bằng `42 5A` (chữ "BZ")
- **tar** là một công cụ để gom nhiều file/thư mục lại thành một gói duy nhất (archive)

**OPTION của tar:**

| Option | Ý nghĩa |
|--------|---------|
| `-c` (create) | tạo gói mới. |
| `-x` (extract) | giải nén gói. |
| `-f` (file) | chỉ định tên file archive. |
| `-v` (verbose) | hiển thị chi tiết quá trình. |
| `-z` (gzip) | nén/giải nén bằng gzip. |
| `-j` (bzip2) | nén/giải nén bằng bzip2. |
| `-J` (xz) | nén/giải nén bằng xz. |

trong đó luôn phải có `-f` để chỉ định tên file

### Step

1. Dùng lệnh: `mktemp -d` Để tạo ra 1 thư mục tmp có tên ngẫu nhiên (`/tmp/tmp.VCwCEhs0GF`)
2. Dùng lệnh: `cp data.txt /tmp/tmp.VCwCEhs0GF` để copy file data.txt này vào thư mục tmp vừa tạo. Làm như vậy để giữ an toàn cho file gốc tránh trường hợp lỗi dẫn đến mất file.
3. Dùng lệnh: `cd /tmp/tmp.VCwCEhs0GF` để đến thư mục tmp
4. Dùng lệnh `cat data.txt` để đọc data.txt. ta sẽ thấy `00000000: 1f8b ...` Đây là 1 Magic number và ta xác định được đây là file nén .gz.
5. Vì tệp data.txt là 1 hex dump nên ta sẽ dùng `xxd` để đọc được file này.

   ```bash
   xxd -r data.txt > data.gz
   ```

   - `xxd -r` đọc và dịch file data.txt
   - `> data.gz` rồi ghi vào data.gz (vì ta đã xác định được đây là file .gz ở trên)
6. Dùng lệnh: `gzip -d data.gz` để giải nén file data.gz. Lúc này da sẽ đc file data
7. Dùng lệnh: `file data` để kiểm tra xem đây là file gì ta sẽ được `data: bzip2 compressed data, block size = 900k`. Đây là file bzip2.
8. Dùng lệnh `bzip2 -d data` để giải nén ta sẽ được file data.out
9. Dùng lệnh: `file data.out` để kiểm tra xem đây là file gì ta sẽ đượcdata.out: `gzip compressed data, was "data4.bin",....` ta kết luận đây là file .gz
10. Dùng lệnh: `mv data.out data.gz` để đổi tên file
11. Dùng lệnh: `gzip -d data.gz` để giải nén ta sẽ nhận được file data.
12. Dùng lệnh: `file data` ta sẽ biết được `data: POSIX tar archive (GNU)` đây là file tar
13. Dùng lệnh: `tar -xf data` để giải nén ta nhận được file data5.bin
14. dùng lệnh: `file data5.bin` ta sẽ biết được data5.bin là file tar
15. Dùng lệnh: `tar -xf data5.bin` để giải nén ta nhận được file data6.bin
16. dùng lệnh: `file data6.bin` ta sẽ biết được data5.bin là file bzip2
17. Dùng lệnh: `bzip2 -d data6.bin` ta sẽ nhận được file data6.bin.out
18. Dùng lệnh: `file data6.bin.out` ta sẽ biết được đây là file tar
19. Dùng lệnh: `tar -xf data6.bin.out` ta sẽ được file data8.bin
20. Dùng lệnh: `file data8.bin` ta biết đây là file .gz
21. Dùng lệnh: `mv data8.bin data8.gz` để đổi về file .gz
22. Dùng lệnh: `gzip -d data8.gz` để giải nén ta sẽ nhận được file data8.
23. Dùng lệnh: `file data8` ta sẽ thấy đây là file ASCII text là file chứa pass
24. Dùng lệnh: `cat data8` để đọc pass

---

## Level 13 → 14

**Goal:** Copy file sshkey.private từ bandit13 về máy để vào được bandit14

### Cơ chế phân quyền trong linux

Như đã nối ở trên tròn linux có 3 nhóm quyền: user, gruop, others

mỗi nhóm sẽ có 3 quyền: `r` - read (4); `w` - wire (2); `x` - execute (1)

khi bạn dùng `ls -l` bạn sẽ thấy phần phân quyền này: VD

```
-rw-r-----   1 bandit14 bandit13 2602 Sep 26 21:52 sshkey.private
```

- Đầu điên `-` đại diện đây là file thường.
- `rw-` là nhóm quyền của user. Thể hiện user có quền đọc và ghi vào file không có quyền chạy file
- `r--` là nhóm quyền của group. Thể hiện group chỉ có quyền đọc chứ không được ghi và chạy file
- `---` là nhóm quền của other. Thể hiện other ko có quyền gì cả

Ngoài ra thay vì biểu diễn dạng rwx thì còn có cách biễu diễn dưới dạng số:

| Nhóm | Phép tính |
|------|-----------|
| `rw-` | 4+2+0=6 |
| `r--` | 4+0+0=4 |
| `---` | 0+0+0= |

Nên có thể biểu diễn là `640`

### Step

1. bash:

   ```bash
   scp -P 2220 bandit13@bandit.labs.overthewire.org:/home/bandit13/sshkey.private ./sshkey.private
   ```

   - `scp` là để dùng để lấy file từ server ssh về máy
   - `-P 2220` chỉ định port
   - `bandit13@bandit.labs.overthewire.org` là user@host
   - `:/home/bandit13/sshkey.private ./sshkey.private` đường dẫn tới file cần lấy trên server và đường dẫn đến nơi lưu file trên máy local
2. Khi lấy được file sshkey.private về máy local, kiểm tra phân quyền của file sshkey.private sẽ thấy: `-rw-r----- 1 user user 2602 Oct  5 09:59 sshkey.private`. Nhưng ssh private key chỉ chấp nhận quyền truy cập dành riêng cho user còn group và others bắt buộc phải không có quyền. Nhưng ở đây ta thấy group có quyền đọc. Nên cần phân quyền lại.
3. Dùng lệnh: `chmod 600 sshkey.private` để file sshkey.private chỉ có quyền đọc và ghi ở user còn group và others sẽ không có quyền.
4. Dùng lệnh: `ssh -p 2220 -i sshkey.private bandit14@bandit.labs.overthewire.org` để vào level tiếp.

---

## Level 14 → 15

**Goal:** gửi mật khẩu của cấp độ hiện tại đến cổng 30000 trên localhost

**Step:**

1. Dùng lệnh: `cat /etc/bandit_pass/bandit14` để lấy pass của bandit14
2. Dùng lệnh `echo "{pass_bandit14}" | nc localhost 30000` để gửi pass lên host localhost và port 30000

### `nc` (netcat)

`nc` (hay netcat) là một công cụ dòng lệnh trong Linux/Unix.

Nó thường được gọi là "dao Swiss Army của mạng" vì có rất nhiều chức năng liên quan đến kết nối TCP/UDP.

Chủ yếu dùng để:

- Tạo kết nối TCP/UDP tới một host/port.
- Lắng nghe (listen) trên một port để nhận dữ liệu.
- Truyền file qua mạng.
- Kiểm tra dịch vụ mạng (giống như telnet nhưng mạnh hơn).

| Tùy chọn | Ý nghĩa |
|----------|---------|
| `-l` | Listen (lắng nghe) |
| `-p` | Chỉ định port |
| `-v` | Verbose (hiển thị chi tiết) |
| `-z` | Chế độ scan, không gửi dữ liệu |
| `-u` | Dùng UDP thay vì TCP |

---

## Level 15 → 16

**Goal:** Gửi mật khẩu của cấp độ hiện tại đến cổng 30001 trên localhost bằng mã hóa SSL/TLS.

```bash
echo 'pass_bandit15' | ncat --ssl localhost 30001
```

### `ncat`

`ncat` là một phiên bản nâng cấp của netcat (nc), được phát triển bởi dự án Nmap. Nó giữ nguyên sự linh hoạt của nc nhưng bổ sung thêm nhiều tính năng hiện đại và bảo mật hơn. Nếu nc được gọi là "dao đa năng của mạng", thì ncat chính là phiên bản cải tiến, mạnh mẽ và an toàn hơn.

**Điểm khác biệt chính giữa nc và ncat**

- **Bảo mật:** ncat hỗ trợ SSL/TLS để mã hóa dữ liệu, trong khi nc chỉ truyền dữ liệu thô.
- **Tính năng nâng cao:** có thể hoạt động như proxy, relay, hoặc hỗ trợ nhiều kết nối cùng lúc.
- **Tích hợp với Nmap:** đi kèm trong bộ công cụ Nmap, thường dùng để kiểm thử bảo mật.
- **Tương thích:** vẫn hỗ trợ cú pháp cơ bản giống nc, nên dễ chuyển đổi.

Cấu trúc: `ncat [tùy chọn] <host> <port>`

**Vài điểm nổi bật**

Lắng nghe với SSL

```bash
ncat --ssl -l -p 1234
```

Proxy đơn giản

```bash
ncat --proxy-type http --proxy 127.0.0.1:8080 example.com 80
```

→ Dùng ncat như một proxy để kết nối qua máy trung gian.

---

## Level 16 → 17

**Goal:** Gửi mật khẩu của cấp độ hiện tại đến một cổng trên localhost trong phạm vi từ 31000 đến 32000. Trước tiên, hãy tìm xem cổng nào trong số này có máy chủ đang lắng nghe. Sau đó, tìm xem máy chủ nào hỗ trợ SSL/TLS và máy chủ nào không.

**Step:**

1. Dùng lệnh:

   ```bash
   nmap -sV -p 31000-32000 --script=ssl-cert localhost
   ```

2. tìm dòng `31790/tcp open  ssl/unknown` Đây là dòng thông báo có mở dịch vụ ssl nhưng nmap không nhận diện được chính xác loại (nmap không có fingerprint trong cơ sở dữ liệu). Thật chất đây là pass checker của bandit.
3. Dùng lệnh: `echo 'pass_bandit16' | ncat --ssl localhost 31790`
4. Nó sẽ trả về 1 đoạn: `-----BEGIN OPENSSH PRIVATE KEY----- .... -----END OPENSSH PRIVATE KEY-----` Đây là ssh private key. copy đoạn này về máy tạo 1 file để lưu nó lại (VD `sshkey17.private`).
5. Dùng lệnh: `chmod 600 sshkey17.private` cấp lại quyền cho file
6. dùng file đó đăng nhập vào cấp tiếp theo

### `nmap`

`nmap` là một công cụ cực kỳ mạnh để quét mạng và port. Nó thường được dùng để kiểm tra các dịch vụ đang chạy trên một máy chủ, phát hiện hệ điều hành, và thậm chí tìm lỗ hổng bảo mật.

Cú pháp: `nmap [options] target`

| Tùy chọn | Ý nghĩa |
|----------|---------|
| `-p` | chỉ port hoặc dãy port |
| `-p-` | quét tất cả port |
| `-sV` | phát hiện phiên bản dịch vụ |
| `-O` | phát hiện hệ điều hành |
| `-A` | quét nâng cao (dịch vụ + OS + script) |
| `-T4` | tăng tốc độ quét |
| `-Pn` | Bỏ qua ping, quét trực tiếp port |
| `-sC` | hy script mặc định (tương đương `--script=default`). chỉ muốn có thông tin cơ bản |
| `--script=ssl-cert` | Chạy script cụ thể để xem chứng chỉ SSL |

---

## Level 17 → 18

**Goal:** So sánh 2 file passwords.new và passwords.old để tìm ra dòng duy nhất khác giữa 2 file. Dòng khác ở file passwords.new là mật khẩu.

```bash
diff passwords.new passwords.old
```

| Option | Ý nghĩa |
|--------|---------|
| `-q` hoặc `--brief` | chỉ báo có khác nhau hay không, không in chi tiết. |
| `-s` | báo cả khi hai file giống hệt nhau. |
| `-u` | hiển thị theo dạng "unified diff" (thường dùng trong patch, Git). |
| `-y` | hiển thị song song hai file để dễ so sánh. |
| `-r` | so sánh đệ quy cả thư mục. |
| `-i` | bỏ qua khác biệt về chữ hoa/thường. |
| `-w` | bỏ qua khác biệt về khoảng trắng. |

---

## Level 18 → 19

**Goal:** Khi đăng nhập vào level 18 ai đó đã sửa đổi tệp `.bashrc` để khiến bạn tự động đăng xuất khi đăng nhập bằng SSH. Tìm cách đọc file readme để lấy pass

```bash
ssh -p 2220 bandit18@bandit.labs.overthewire.org cat /home/bandit18/readme
```

Tùy chọn lệnh ở cuối sẽ đọc file readme rồi thoát luôn chứ không cần vào bandit18.

---

## Level 19 → 20

**Goal:** Mượn tạm quyền thực thi của người tạo ra file để xem pass

**Step:**

1. Dùng lệnh: `file bandit20-do` bạn sẽ thấy: `bandit20-do: setuid ELF 32-bit LSB executable,....`

   `setuid` là đặt biệt cho phép bạn tạm thời dùng quyền của chủ sở hữu file bandit20-do để chạy 1 lệnh nào đó thay vì quyền user của bạn.
2. Dùng lệnh: `./bandit20-do cat /etc/bandit_pass/bandit20` để đọc pass.

---

## Level 20 → 21

**Goal:** Mở 1 port để lắng nghe và để pass của level trước vào sau đó dùng file suconnect để kết nối tới port đó. Nếu mật khẩu level trước trùng khớp thì sẽ trả về mật khẩu tiếp theo.

**Step:**

1. Dùng lệnh: `echo "pass_bandit20" | ncat -l -p 2026 &`. Mở port 2026 để lắng nghe và đưa pass_bandit20 đó.
2. Dùng lệnh: `/suconnect 2026`. Kết nối đến port 2026 để suconnect kiểm tra pass_bandit20. Nếu nó khớp thì sẽ trả về pass tiếp theo.

Dấu `&` thể hiện là bạn đưa tiến trình này xuống chạy ngầm, xong nó sẽ hiện (VD `[1] 217`). Đây thể hiện tiến trình đang chạy ngầm. Nếu không làm như vậy nó sẽ chiếm dụng terminal, buộc bạn phải làm xong việc rồi tắt nó đi mới dùng lệnh tiếp theo được.

**Các lệnh hay dùng để xử lý tiếng trình:**

| Lệnh | Ý nghĩa |
|------|---------|
| `jobs` | Liệt kê các tiến trình đang chạy ngầm trong session hiện tại. |
| `fg %1` | Đưa tiến trình vị trí số 1 từ background trở lại foreground. |
| `bg %1` | Tiếp tục chạy tiến trình số 1 ở background nếu nó đang bị tạm dừng (Paused). |
| `kill %1` hoặc `kill PID` | Dừng/xóa tiến trình ngầm (rất hữu ích nếu mở nhầm port hoặc port bị chiếm). |

Ngoài ra nếu dùng ncat còn có 1 tham số khác là `-k` (Keep-alive): Giữ cho ncat tiếp tục lắng nghe không bị ngắt giữa chừng

---

## Level 21 → 22

**Goal:** kiểm tra trong thư mục `etc/cron.d/` để tìm pass cho level tiếp theo.

### Cron job

Cron job là 1 trương trình giúp bạn làm những công việc lặp đi lặp lại vào 1 khoản thời gian cố định, ví dụ bình thường cứ sau mỗi 3 ngày bạn phải xóa các file tmp để giả phóng bộ nhớ thì thay vì phải tự làm việc đó thì bạn setup cho cron cứ sau 3 ngày thì tự động xóa file tmp.

File cron là file text đơn giản chứa các lệnh được chạy trong thời gian cụ thể. File crontab mặc định trong hệ thống là `/etc/crontab` và nằm trong thư mục crontab `/etc/cron.*/`. Chỉ quản trị viên hệ thống mới có thể chỉnh sửa file crontab trên hệ thống.

**Cấu tạo:**

```
* * * * * command
```

- dấu `*` thứ nhất chỉ phút (tham số từ 0 đến 59)
- dấu `*` thứ hai chỉ giờ (tham số từ 0 đến 23)
- dấu `*` thứ ba chỉ ngày (tham số từ 1 đến 31)
- dấu `*` thứ tư chỉ tháng (tham số từ 1 đến 12)
- dấu `*` thứ năm chỉ thứ (tham số từ 0 đến 7) (Lưu ý số 0 là CN, số 1 là T2,... số 6 là T7, số 7 là CN)

Ngoài ra còn các cú pháp chi tiết hơn như sau:

| Ký tự | Ý nghĩa |
|-------|---------|
| `*` (dấu hoa thị) | để xác định tất cả tham số được lên lịch (VD dấu `*` ở vị trí đầu nghĩa là chạy mỗi phút từ 0 đến 59) |
| `,` (dấu phẩy) | Để ngăn cách các giá trị cụ thể (VD vị trí thứ 2 là `1,10,20` thì nghĩa là chạy vào lúc 1h, 10h và 20h) |
| `-` (dấu gạch nối) | Để xác định 1 khoản tham số liên tục (VD ở vị trí thứ 3 là `2-5` nghĩa là chạy vào các ngày 2,3,4,5) |
| `/` (dấu gạch chéo) | Để xác định khoản nghỉ (VD vị trí thứ 4 là `*/2` là cứ mỗi 2 tháng chạy 1 lần) |
| `L` | Cuối cùng (L) – cho mục đích cụ thể là chỉ định ngày cuối cùng của tuần trong tháng. Ví dụ, `3L` nghĩa là thứ tư cuối cùng. |

**Các phím tắt nhanh (Shortcuts)**

Thay vì viết 5 trường thời gian bằng số, bạn có thể dùng các chuỗi ký tự đặc biệt có sẵn:

| Shortcut | Ý nghĩa |
|----------|---------|
| `@reboot` | Chạy một lần duy nhất ngay khi hệ thống vừa khởi động. |
| `@hourly` | Chạy vào phút đầu tiên của mỗi giờ (tương đương `0 * * * *`). |
| `@daily` hoặc `@midnight` | Chạy vào lúc 00:00 mỗi ngày (tương đương `0 0 * * *`). |
| `@weekly` | Chạy vào lúc 00:00 ngày Chủ Nhật hàng tuần (tương đương `0 0 * * 0`). |
| `@monthly` | Chạy vào lúc 00:00 ngày đầu tiên của mỗi tháng (tương đương `0 0 1 * *`). |
| `@yearly` | Chạy vào lúc 00:00 ngày 1 tháng 1 hàng năm (tương đương `0 0 1 1 *`) |

**Lệnh quản lý Cron Job**

Để cấu hình cron, bạn sử dụng công cụ điều khiển crontab với các tùy chọn sau:

| Lệnh | Ý nghĩa |
|------|---------|
| `crontab -e` | Mở trình chỉnh sửa để thêm, sửa hoặc xóa các cron job. |
| `crontab -l` | Xem danh sách các cron job hiện có của bạn. |
| `crontab -r` | Xóa toàn bộ các cấu hình cron job hiện có. |

### Các bước tìm mật khẩu

1. Dùng lệnh: `cd etc/cron.d/` để vào trong thư mục chứa cron cần tìm:
2. Dùng lệnh: `ls -l` để tìm đúng cron job: `-rw-r--r-- 1 root root 120 Sep 26 21:53 cronjob_bandit22`. Vì mình cần tìm pass lên level 22 nên cronjob_bandit22 là file con job mình cần
3. Dùng lệnh `cat cronjob_bandit22` ta sẽ thấy:

   ```
   @reboot bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null
   * * * * * bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null
   ```

   ta sẽ thấy corn này chạy lệnh file script `/usr/bin/cronjob_bandit22.sh` khi hệ thống khởi động và mỗi phút 1 lần liên tục và ném stdout và stderr và "hố đen" để không hiện lên màn hình terminal.
4. Dùng lệnh: `cat /usr/bin/cronjob_bandit22.sh` để đọc đoạn script và cron job chạy:

   ```bash
   #!/bin/bash
   chmod 644 /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
   cat /etc/bandit_pass/bandit22 > /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
   ```

   Đoạn script thực hiện cấu quền 644 cho file `/tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv` và xuất pass của bandit22 vào file tmp này.
5. Dùng lệnh: `cat /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv` để đọc mật khẩu

---

## Level 22 → 23

**Goal:** kiểm tra trong thư mục `etc/cron.d/` để tìm pass cho level tiếp theo.

### `cut`

lệnh `cut` trong linux dùng để cắt hoặc trích xuất 1 dữ liệu từ mỗi trong file hoặc từ input dựa trên ký tự, bytes, hoặc trường được phân cách bởi dấu phân cách.

Cấu trúc: `cut OPTION FILE`

| OPTION | Ý nghĩa |
|--------|---------|
| `-c` (Characters) | chọn số lượng ký tự (An toàn với Tiếng Việt) |
| `-b` (bytes) | chọn theo số lượng bytes (Tiếng việt có dấu hoặc emoji có thể chiếm từ 2 đến 4 bytes nên khi cắt theo bytes rất dễ bị lỗi hiển thị) |
| `-f` (Fields) | Cắt theo cột/trường dựa trên một ký tự phân tách (mặc định là ký tự Tab). |
| `-d` (Delimiter) | Thay đổi ký tự phân tách cột (mặc định là Tab). |

### `md5sum`

`md5sum` là một công cụ dòng lệnh trên Linux được sử dụng để tính toán và kiểm tra mã băm (checksum) MD5 của một tệp tin

### Step

1. Dùng lệnh: `cat /etc/cron.d/cronjob_bandit23` để đọc corn:

   ```
   @reboot bandit23 /usr/bin/cronjob_bandit23.sh  &> /dev/null
   * * * * * bandit23 /usr/bin/cronjob_bandit23.sh  &> /dev/null
   ```

   Ta sẽ thấy cron này chạy script `/usr/bin/cronjob_bandit23.sh`
2. `cat /usr/bin/cronjob_bandit23.sh` để đọc script:

   ```bash
   #!/bin/bash
   myname=$(whoami)
   mytarget=$(echo I am user $myname | md5sum | cut -d ' ' -f 1)
   echo "Copying passwordfile /etc/bandit_pass/$myname to /tmp/$mytarget"
   cat /etc/bandit_pass/$myname > /tmp/$mytarget
   ```

   `whoami` là câu lệnh trả về tên người trực tiếp chạy script đó (ở đây là bandit23)

   `echo I am user $myname | md5sum | cut -d ' ' -f 1`. Nghĩa là đưa dòng chữ "I am user bandit23" vào hàm băm md5 rồi lấy trường thứ nhất (`-f 1`) được phân cách bởi đấu space (`-d' '`). Gán mã băm đó vào biến mytarget. rồi xuất pass của level 22 vào file `/tmp/$mytarget`
3. Dùng lệnh: `md5sum <<<"I am user bandit23" | cut -d' ' -f 1` để tìm ra tên của file tmp chứa pass: ta được `8ca319486bfbbc3663ea0fbe81326349`
4. Dùng lệnh: `cat /tmp/8ca319486bfbbc3663ea0fbe81326349` để tìm ra pass

---

## Level 23 → 24

**Goal:** kiểm tra trong thư mục `etc/cron.d/` để tìm pass cho level tiếp theo.

```bash
!/bin/bash
shopt -s nullglob
myname=$(whoami)
cd /var/spool/"$myname"/foo || exit
echo "Executing and deleting all scripts in /var/spool/$myname/foo:"
for i in * .*;
do
    if [ "$i" != "." ] && [ "$i" != ".." ];
    then
        echo "Handling $i"
        owner="$(stat --format "%U" "./$i")"
        if [ "${owner}" = "bandit23" ] && [ -f "$i" ]; then
            timeout -s 9 60 "./$i"
        fi
        rm -rf "./$i"
    fi
done
```

Đoạn script trên sẽ vào thư mục `/var/spool/bandit24/foo` và kiểm tra tất cả các file thông thường có owner là banidit23 nếu đúng thì sẽ chạy file đó trong đúng 60s. Nếu sau 60s mà script chưa chạy xong thì lặp tức cưỡng chế ngừng (kill) ngay lặp tức. Sau đó xóa vĩnh viễn file đó mà không cần thông báo.

### Step

2. Dùng lệnh: `ls -l /var/spool/bandit24` ta sẽ thấy folder foo sẽ có quyền:

   ```
   drwxrwx-wx 5 root bandit24 4096 Oct  8 12:15 foo
   ```

   Trong đó others sẽ có quyền `-wx`, nghĩa là ta chỉ có quyền thực thi và viết vào folder đó chứ không được đọc (`ls`) folder đó.
3. Dùng lệnh: `mktemp -d` Để tạo ra 1 thư mục temp `/tmp/tmp.K01gtbZysr`
4. Dùng lệnh: `chmod 777 /tmp/tmp.K01gtbZysr` để cấp quyền cho others và group có quyền đọc, viết, thực thi vào thư mục đó. Chủ yếu là quyền read của others vì khi đưa file vào thư mục `/var/spool/bandit24/foo`, thư mục sẽ chạy dưới quyền của bandit24, nên ta cần cho bandit24 được phép viết vào thư mục tmp.
5. Dùng lệnh: `nano /tmp/tmp.K01gtbZysr/hack.sh` và viết vào trong đó đoạn script:

   ```bash
   #!/bin/bash
   cat /etc/bandit_pass/bandit24 > /tmp/tmp.K01gtbZysr/pass.txt
   ```

   Do file này sẽ chạy dưới quyền của bandit24 nên ta có thể đọc được password của bandit24
6. Dùng lệnh: `chmod 755 /tmp/tmp.K01gtbZysr/hack.sh` để cấp quyền thực thi để chạy file
7. Dùng lệnh: `cp /tmp/tmp.K01gtbZysr/hack.sh /var/spool/bandit24/foo` để đưa file hack.sh vào thư mục `/var/spool/bandit24/foo`.
8. Dùng lệnh: `cat /tmp/tmp.K01gtbZysr/pass.txt` Sau 1 phút đọc file `/tmp/tmp.K01gtbZysr/pass.txt` để lấy pass

---

## Level 24 → 25

**Goal:** Gửi mật khẩu level hiện tại cộng với 1 mã pin 4 số cách nhau bởi dấu cách (space) đến port 30002. Và ta không biết mã pin là bao nhiêu chỉ biết mã pin nằm trong khoản 0000 đến 9999.

**Step:**

1. Dùng lệnh: `echo "pass_bandit24 0000" | ncat localhost 30002` để kiểm tra thử xem nó sẽ trả lời thế nào:

   ```
   I am the pincode checker for user bandit25. Please enter the password for user bandit24 and the secret pincode on a single line, separated by a space.
   Wrong! Please enter the correct current password and pincode. Try again.
   ```

2. Dùng lệnh: `nano /tmp/bruteforce.sh` Để tạo ra 1 file script:

   ```bash
   #!/bin/bash
   pass24=pass_bandit24
   for i in {0000..9999}
   do
       echo "$pass24" "$i" | ncat localhost 30002 > /tmp/pass.txt
       if ! grep -q "Try again" /tmp/pass.txt;then
           cat /tmp/pass.txt
           break
       fi
   done
   ```

   Đoạn script này sẽ tạo ra 1 vòng lặp chạy từ 0000 đến 9999 để thử mã pin, sau đó đưa pass pin lên port 30002 rồi đưa kết quả nó trả về vào file `/tmp/pass.txt`. Vì đã thử ở trên, nếu nó sai thì sẽ hiện "Try again" nên nếu đến mã pin nào không có "Try again" thì đó là mã pin có pass và nó sẽ in kết quả chứa pass ra:

   ```
   I am the pincode checker for user bandit25. Please enter the password for user bandit24 and the secret pincode on a single line, separated by a space.
   Correct!
   The password of user bandit25 is pass_bandit25
   ```

> **Lưu ý:** trước khi chạy `/tmp/bruteforce.sh` thì cần cấp quyền thực thi (x) cho nó trước:
>
> ```bash
> chmod +x /tmp/bruteforce.sh
> ```
