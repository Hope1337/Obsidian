Chán dùng chuột khi code?
Hãy dùng VIM

**Command Count Motion**
### Advance
```vim
# find, at the charector
f 

# find backward
Shift + f

# find, before the charactor
t 

find backward
Shift t

# find forward
; 

# find backward
, 

# copy mọi thứ trong ngoặc
ya{

# 
va{
va(
vi{

# new line
Ctrl + enter

# đóng mở ngoặc nhanh (visual mode, obsidian ko dùng dc)
Shift + s + {

# open new editor window
Ctrl + \
```

```vim
# open new terminal
Ctrl + `

# move to terminal 
Ctrl + w + j

# move back to editor
Ctrl + 1

# quit
:q

# quit and discard changes
:q!

# write and quit
:wq

# relative number
:set relativenumber
```
### 1. Normal mode

```vim
# insert mode (before current letter)
i

# insert mode at the beging of line
Shift + i

# after current letter
a

# insert mode at the end of line
Shift + a

# new line and insert mode (below)
o

# new line (up)
Shift + o

# undo
u

# redo
Ctrl + r

# erase current word + insert mode
s

# find 
/something

# press n to find the next thing
n

# or to find back
Shift + n

# find current word (backward)
#
# find current word (forward)
*
# mark a point and move to it
ma
'a

# center the screen
zz

# xem buffer
:reg

# paste the thing I just deleted
"0p

# delete still in norml mode
Shift + d
```

### 2. Visual mode

```vim
# visual mode
v

# visual block mode
Shift + v

# visual ? mode (quên rồi=))))
Ctrl + v

# tái chọn vùng đã chọn
gv

# copy
y

# copy line
yy

# paste after
p

# paste before
P

# cut
x

# delete
d

# delete line
dd

# delete from cursor on
D

# delete selected + insert mode
c

# delete line + insert mode
cc

# delete + change from cursor on 
D

# replace one char
r

# keep replace
R
```

### 3. Navigate

```vim
# move forward
w

# move backward
b

# delete word (even at middle of word)
diw

# move to begin of line
0             # remain normal mode 
Shift + i # insert mode

# move to end of line
$               # remain normal mode
Shift + a # insert mode

# woaaaaaaaaaaaa
ci"
ci(
ci{

# move to the next paragraph
}
{ # backward

# jump to next bracket
%

# jump to the next ,
t,

# delete to the next ,
ct,

# jump to begining of file
gg

#jump to the end of file
G

# jump to specific line
1337gg

# nhảy đến đầu màn hình
Shift + h 

# nhảy đến cuối màn hình
Shift + L 

# giữa màn hình
Shift + m

# Lên nửa trang
Ctrl + u

# Xuống nửa trang
Ctrl + d

# Lên một trang
Ctrl + b

# Xuống một trang
Ctrl + f

# Đưa dòng hiện tại lên đầu màn hình
z + t

# Đưa dòng hiện tại về cuối màn hình
z + b

```

### 4. Visual mode
```vim
# visual line mode
Shift + v

# indent 
>
<

# jump while in visual mode
o

# incremental 
g + Ctrl + a
```

### 5. Find and replace

```vim
# search
/

# backward
?

# search selected
/ + Ctrl + R + 0

# search current word
*

# search and replace
:%s/cũ/mới/[cờ]
```
Các cờ tùy chọn:
- `g`: thay thế toàn bộ trong mỗi dòng, nếu ko để thì chỉ thay thế phần tử khớp đầu tiên
- `i`: không phân biệt hoa thường, nếu ko để thì mặc định là I
- `c`: xác nhận trước khi thay thế
- `I`: phân biệt hoa thường

Giới hạn phạm vi thay thế:
```vim
# Chỉ dòng hiện tại
:s

# phạm vi cụ thể từ dòng 1 đến dòng 5
:1,5s

# từ dòng hiện tại tới cuối file
:.,$s

# trong vùng được chọn (bật visual mode là nó tự động thêm vào)
:'<,>'s
```

### 5. Macro 

```vim
# start macro key a
qa

# stop macro
q

# use macro 
@a
```


### 6. Navigate on working dir

```vim
# back and forth
Ctrl + Shift + E
```











![[Vim.pdf]]






