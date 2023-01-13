---
draft: true
---

# Hướng dẫn viết Event cho Admin
Mọi người có thể xem ví dụ ở [đây](./2022-09-05-thaidn-talk/index.md) nha (ở chế độ raw)

## Tạo thư mục và file
Tạo thư mục `/content/event/<tên-sự-kiện>` và thêm một file index.md với cú pháp gồm các field

1. title
    - Tên của sự kiện
```
title: "Grokking Techtalk 46: Những bài học về xâm nhập và bảo vệ hệ thống mạng Việt Nam"
```

2. event & event_url
    - Tên link và URL link đến trang sự kiện
```
event:  Grokking - BKISC Security Techtalk
event_url: https://www.facebook.com/events/612981910205304
```

3. location & address
    - Địa điểm tổ chức
```
location: A4-Hall, Ho Chi Minh University of Technology
address:
  street: 268 Ly Thuong Kiet
  city: Ho Chi Minh
  country: Vietnam
```

5. abstract
    - Giới thiệu nội dung buổi talk chi tiết
```
abstract: " Trong những năm gần đây, Việt Nam luôn là một trong những quốc gia có tỉ lệ nhiễm mã độc và hứng chịu các cuộc tấn công mạng thuộc nhóm cao trên thế giới. Bên cạnh đó, mức độ sử dụng máy tính và các thiết bị thông minh tại Việt Nam tăng đột biến do ảnh hưởng của COVID-19, và đây cũng chính là môi trường lý tưởng để virus bùng phát, lây lan mạnh. Điều nay làm dấy lên mối lo ngại về an ninh tr..."
```

6. summary
    - Giới thiệu buổi talk vắn tắt hơn, sẽ được đăng ở trang list các event /event
```
summary: Anh Dương Ngọc Thái, hiện đang làm việc tại Google và thường được biết đến thông qua blog cá nhân vnhacker@blogspot, sẽ kể về những lần tham gia kiểm thử và tấn công hệ thống của một số bệnh viện và ngân hàng ở Việt Nam. Qua đó, anh nói lên tầm quan trọng của kiểm thử bảo mật phần mềm và chính sách breach notification nhằm nâng cao nhận thức về bảo vệ thông tin người dùng của doanh nghiệp.
```

7. date & date_end & all_day
    - Ngày tổ chức và kết thúc
    - Format: `<YYYY>-<MM>-<DD>T<HH>:<MM>:<SS>Z`
```
date: '2022-09-05T18:00:00Z'
date_end: '2022-09-05T21:00:00Z'
all_day: false

```

8. publishDate
    - Ngày thông báo sự kiện diễn ra
    - Format: `<YYYY>-<MM>-<DD>T<HH>:<MM>:<SS>Z`
```
publishDate: '2017-01-01T00:00:00Z'
```

9. authors & tags
    - Sử dụng authors và tags như đã dùng đối với việc viết blog. Đối với author nên để khớp với author đã đặt bên /authors nếu người đó có xuất hiện trong member list /member
```
authors: ['onirique', 'thaidn']
tags: ['ctf','seminar','talk']
```

10. featured
    - Nếu có sẽ được để trên trang home
```
# Is this a featured talk? (true/false)
featured: true
```

11. url_[format]
    - Các định dạng khác liên quan đến buổi talk: slide, pdf, video, code. Đưa những url liên quan với những tài nguyên có định dạng khác nhau vào đây
```
url_code: ''
url_pdf: ''
url_slides: ''
url_video: ''
``` 

12. slides
    - Nếu đăng event dưới dạng markdown slide
```
# Markdown Slides (optional).
#   Associate this talk with Markdown slides.
#   Simply enter your slide deck's filename without extension.
#   E.g. `slides = "example-slides"` references `content/slides/example-slides.md`.
#   Otherwise, set `slides = ""`.

slides:
```

13. projects
    - Link sự kiện này đến 1 project
```
# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
```
14. image
    - Config cho hình featured.jpg/png cùng thư mục
```
image:
  caption: 'Image credit: [**Unsplash**](https://unsplash.com/photos/bzdhc5b3Bxs)'
  focal_point: Right
```