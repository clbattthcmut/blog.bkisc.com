---
draft: true
---

# Hướng dẫn đóng góp Blog
Các thành viên đăng các bài Writeup từ CTF, Wargames, Flare-on, NSUCryptos, những bài article chủ đề security, bug-bounty report,....  
Mọi người có thể xem ví dụ ở [đây](./duti/sekai-ctf-2022-bottle-poem/index.md) nha (ở chế độ raw)

## Tạo thư mục và file
Thành viên có nickname là `Bkisc` muốn viết bài `Sekai CTF 2022: Bottle Poem` sẽ tạo thư mục `/content/blog/bkisc/sekai-ctf-2022-bottle-poem`, thêm vào đó một file `index.md` và nhiều file ảnh khác nhau. Trong đó đặt 1 file ảnh chính `featured.jpg` để hiện ảnh này trên trang list các blog /blog. 
Sau đây là lưu ý cho file index.md
## Các field cần lưu ý trên Front Matter (YAML)
Mọi người có thể tham khảo thêm hướng dẫn đầy đủ tại [đây](https://wowchemy.com/docs/content/publications/)
1. title
    - Tên bài blog
2. author
    - Tác giả bài blog, một bài blog có thể có nhiều tác giả
    - Tên tác giả cần phải khớp với tên đã ghi cho field `authors` trong hướng dẫn thêm member
```
authors:
  - bkisc
  - hdthinh1012
```
3. date
    - Ngày đăng bài
    - Format: `<YYYY>-<MM>-<DD>T<HH>:<MM>:<SS>Z`
```
date: '2023-01-12T00:00:00Z'
```
4. doi
    - Field này mình chưa rõ
5. publishDate
    - **Note:** Do đến đúng publish date bài đăng mới xuất hiện trên blogs và hiện đang tính theo giờ GMT nên mong mọi người dời về trước 7 giờ hoặc trước hẳn 1 ngày để blog xuất hiện ngay lập tức sau khi pull request 
    - Ngày đăng bài, để trùng với 3. date
```
publishDate: '2023-01-12T00:00:00Z'
```
6. publication_types
    - Kiểu phát hành: do template mình lấy vốn để đăng bài báo khoa học nên kiểu ở đây sẽ liên quan đến các kiểu tạp chí khoa học nhiều hơn
    - Đối với bài CTF Writeup, mọi người chọn ['9'] là writeups nha. Mọi người tham khảo kiểu publication và số index tương ứng trong file `data/publication_types.toml`
```
# Publication type.
# Legend: 0 = Uncategorized; 1 = Conference paper; 2 = Journal article;
# 3 = Preprint / Working Paper; 4 = Report; 5 = Book; 6 = Book section;
# 7 = Thesis; 8 = Patent; 9 = Writeups
publication_types: ['9']
```

7. publication, publication_short
    - Hai mục này liên quan bài báo khoa học hơn, mọi người có thể bỏ qua
```
publication: ''
publication_short: ''
```

8. abstract
    - Mục này liên quan bài báo khoa học hơn, mọi người có thể viết tóm gọn nội dung bài blog
```
abstract: Write up for a challenge from Sekai CTF 2022
```

9. summary
    - Tóm tắt nội dung bài blog, sẽ được hiện thị trên trang list các blog /blog
```
# Summary. An optional shortened abstract.
summary: Deserialization attack with Python Bottle.
```

10. (Nên có) tags
    - Các tags để nhấn mạnh chủ đề bài viết của mọi người, các tags sẽ hiện ở trang /blog để người đọc có thể browse nhóm bài blog mong muốn nên mọi người nên để nha.
    - Mọi người nên xem kỹ các tag đã có khi mở url /blog trên web, để không tạo các tags trùng nghĩa nhưng khác spelling, tránh gây loãng.
    - Một số tags gợi ý như
        - ctf
        - root-me
        - wsa
        - burp-suite
        - ida
        - writeup
        - web
        - crypto
        - re
        - pwn
        - forensic
        - steganography
        - nsucrypto-<số năm>
        - isitdtu-<số năm>
        - ascis-<số năm>

```
tags:
  - ctf
  - writeup
  - web exploitation
```

11. links 
    - Link liên quan đến bài viết, mọi người có thể đăng link bài blog trên trang cá nhân của mình.
```
# Mọi người có thể đăng link từ original page của mình
links:
  - name: Original Link
    url: http://example.org
```

12. url_[upload-format]
    - Đối với các bài blog hay bài báo khoa học mọi người đăng trên đây còn tồn tại dưới các định dạng khác như PDF, Code (source code liên quan), Dataset (dữ liệu liên qua), Poster (???), Project (???), slides (trên slideshare hay google presentation), Source (???), video. Mọi người có thể đính kèm online url link cho các resource trên với format tương ứng
```
# Link đính kèm với format tương ứng
url_pdf: ''
url_code: ''
url_dataset: ''
url_poster: ''
url_project: ''
url_slides: ''
url_source: ''
url_video: ''
```

13. image
    - Config cho featured.png image mọi người đã thêm ở trên
```
image:
  caption: 'Image credit: [**Sekai CTF**](https://unsplash.com/photos/s9CC2SKySJM)'
  focal_point: ''
  preview_only: false
```

14. (Cho chuỗi bài viết series) projects
    - Field này dùng nếu như mọi người muốn liên hệ một vài bài viết chung series
    - Ví dụ mọi người muốn viết bài blog `Linux Kernel Exploitation part 1` cho projects/series `Linux-kernel-exploitation` mọi người có thể điền field này là `project: linux-kernel-exploitation` rồi tạo một thư mục content/project/linux-kernel-exploitation/index.md để hiển thị tên project/series đó sử dụng command: `hugo new  --kind project project/linux-kernel-exploitation`. 

```
# Associated Projects (optional).
#   Associate this publication with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `internal-project` references `content/project/internal-project/index.md`.
#   Otherwise, set `projects: []`.
projects: linux-kernel-exploitation
```

15. slides
    - field này dành cho ai đăng blog dưới dạng markdown slides
```
# Slides (optional).
#   Associate this publication with Markdown slides.
#   Simply enter your slide deck's filename without extension.
#   E.g. `slides: "example"` references `content/slides/example/index.md`.
#   Otherwise, set `slides: ""`.
slides: ""
```

16. featured
    - Nếu true sẽ được để trên trang home
```
featured: true
```

## Phần nội dung (Markdown)
Mọi người viết blog theo cú pháp Markdown với một số shortcodes hỗ trợ bới Hugo mặc định và shortcodes mở rộng hỗ trợ bởi theme Wowchemy

- Shortcode mặc định bởi Hugo tại [đây](https://gohugo.io/content-management/shortcodes/) (không nhiều)
- Shortcode mở rộng bởi Wowchemy theme tại [đây](https://wowchemy.com/docs/content/writing-markdown-latex/#math) (có cả hỗ trợ Latex với [math](https://wowchemy.com/docs/content/writing-markdown-latex/#math))