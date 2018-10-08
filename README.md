# Cài đặt hugo blog (Ubuntu 18.04)
---
## Bước 1: Cài đặt hugo package
Truy cập
```
https://github.com/gohugoio/hugo/releases
```
- Đối với ubuntu, down bản .deb, cài như bình thường.

### Bước 2: Chuẩn bị
Hugo page yêu cầu 2 repo:
- 1 (blog) repo: Chứa bài viết 
- 1 (user.github.io) repo: Chứa project build (hugo cần build lại từ raw content sang kiến trúc riêng của hugo).
Lựa chọn theme
- Down theme hugo chỉ định về (https://github.com/lacoski/spf13.com) bản chất theme hugo là 1 repo raw kiến trúc hugo. (zip hoặc git repo)

### Bước 3: Cài đặt
- Clone repo blog về máy (git clone <blog-url>)
- Copy dữ liệu từ theme chỉ định sang
```
cp <theme>/* blog/
```
- Kiểm tra cấu hình
```
vi blog/config.toml

---

baseurl = "https://lacoski.github.io/" # Sửa lại theo git user page io
title = "Blog của Nguyễn Bá Thành" # Title của trang
languageCode = "en-us"
disqusShortname = "thanhnb"
copyright = "Copyright (c) 2008 - 2018, Steve Francia; all rights reserved."
MetaDataFormat = "toml"
googleAnalytics = "UA-7131036-1"

[author]
    name = "Nguyễn Bá Thành"

[indexes]
    tag = "tags"
    topic = "topics"

[privacy]
  [privacy.googleAnalytics]
    anonymizeIP = true
    respectDoNotTrack = true
    useSessionStorage = true
  [privacy.twitter]
    enableDNT = true
  [privacy.youtube]
    privacyEnhanced = true
```

### Bước 4: Build thử tại máy
```
$ cd blog/
$ hugo server -D
...
Watching for changes in /home/thanhnb/Hugo/blog/{content,layouts,static}
Watching for config changes in /home/thanhnb/Hugo/blog/config.toml
Serving pages from memory
Running in Fast Render Mode. For full rebuilds on change: hugo server --disableFastRender
Web Server is available at http://localhost:1313/ (bind address 127.0.0.1)
Press Ctrl+C to stop

```
> Truy cập web theo URL: http://localhost:1313/

### Bước 5: Viết bài
Truy cập vào nơi lưu trữ các bài biết, viết bài theo kiến trúc từng theme

Ở đây, truy cập
```
blog/content/post
blog/content/presentation
blog/content/project
```

### Bước 6: Build, deploy
#### Đầu tiên xóa file `public` (nếu có)
```
cd blog/
rm -rf public
```
#### Add remote branch
> Code hugo sau khi build sẽ được đưa tới repo github io 
```
git submodule add -b master git@github.com:<USERNAME>/<USERNAME>.github.io.git public

VD:
git submodule add -b master git@github.com:lacoski/lacoski.github.io.git public
```
Lưu ý:
- Để add được git submodule phải sử dụng keypair ssh của github (Truy cập `https://github.com/settings/keys`, add key máy sử dụng)

Kết quả:
```
git submodule add -b master git@github.com:lacoski/lacoski.github.io.git public
Cloning into '/home/thanhnb/Hugo/blog/public'...
remote: Enumerating objects: 3624, done.
remote: Counting objects: 100% (3624/3624), done.
remote: Compressing objects: 100% (1269/1269), done.
remote: Total 3624 (delta 1972), reused 3542 (delta 1890), pack-reused 0
Receiving objects: 100% (3624/3624), 18.89 MiB | 89.00 KiB/s, done.
Resolving deltas: 100% (1972/1972), done.
```

#### Build web
```
cd blog/
vi deploy.sh

---

#!/bin/bash

echo -e "\033[0;32mDeploying updates to GitHub...\033[0m"

# Build the project.
hugo # if using a theme, replace with `hugo -t <YOURTHEME>`

# Go To Public folder
cd public
# Add changes to git.
git add .

# Commit changes.
msg="rebuilding site `date`"
if [ $# -eq 1 ]
  then msg="$1"
fi
git commit -m "$msg"

# Push source and build repos.
git push origin master

# Come Back up to the Project Root
cd ..
```
- Cấp quyền thực thi 
```
chmod +x deploy.sh
```
- Chạy
```
./deploy.sh "commit ..."

....

[master 33a19ff] rebuilding site Thứ hai, 08 Tháng 10 năm 2018 10:58:06 +07
 1 file changed, 6 insertions(+), 6 deletions(-)
Counting objects: 3, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 365 bytes | 365.00 KiB/s, done.
Total 3 (delta 2), reused 0 (delta 0)
remote: Resolving deltas: 100% (2/2), completed with 2 local objects.
To github.com:lacoski/lacoski.github.io.git
   0d13cb8..33a19ff  master -> master
thanhnb@thanhnb-pc:~/Hugo/blog$ 

```

### Bước 7: Truy cập web đã build
Truy cập
```
https://<user>.github.io
```