---
draft: true
---
# Hướng dẫn thêm bản thân vào danh sách thành viên
Mọi người có thể xem ví dụ ở [đây](./吳恩達/_index.md) nha (ở chế độ raw)
## Tạo thư mục và file
Thành viên có nickname là `Bkisc` sẽ tạo thư mục /content/authors/bkisc và thêm 2 file _index.md và avatar.jpg (hình avatar bản thân)  
Dưới đây là lưu ý cho _index.md

## Các field cần lưu ý trên Front Matter (YAML)
0. draft
    - Cái này để set true/false nếu file này chỉ là nháp, không xuất hiện trên blog khi publish, field này mọi người để là false nha
```
draft: false
```

1. title
   - Tên của thành viên hiển thị trên danh sách thành viên (url: /member)
```
title: 吳恩達
```
2. authors
   - Tên được đặt dưới mỗi bài đăng mà người đó là tác giả, nên để khớp với tên thư mục con trong thư mục /authors và thư mục /blog
```
# Username (this should match the folder name)
authors:
   - 吳恩達
```
3. superuser
   - Mục này mọi người cứ để false nha
4. role
   - Để chuyên ngành, chức vụ của mọi người ví dụ như: Professor of Blockchain & Cryptography & Applied Machine Learning, Web Pentester, Pwner, Cryptographer, Reversing Engineer, Hacker lỏd và vân vân... Field này mọi người cứ đặt thoải mái nha.
```
role: web-exploitation
```
5. organization:
   - Danh sách tổ thức mà người đó liên quan/làm việc cho bên cạnh BKISC:
```
organizations:
   - name: Stanford University
    url: ''
   - name: VNG Corporation
    url: ''
```
6. bio
   - Trình bày ngắn gọn về bản thân (sẽ để kèm với tên bản thân ở cuối mỗi bài đăng)
```
bio: My research interests include distributed robotics, mobile computing and programmable matter.
```
7. interest
   - Sở thích, lĩnh vực quan tâm
```
interests:
   - Artificial Intelligence
   - Computational Linguistics
   - Information Retrieval
```
8. education
   - Giới thiệu về học vụ, khóa đào tạo đã trải qua
```
education:
  courses:
    - course: PhD in Artificial Intelligence
    institution: Stanford University
    year: 2012-2016
    - course: MEng in Artificial Intelligence
    institution: Massachusetts Institute of Technology
    year: 2009-2012
    - course: BSc in Artificial Intelligence
    institution: Massachusetts Institute of Technology
    year: 2008-2009
```

9. social
    - Thông tin mạng xã hội
```
social:
  - icon: envelope
    icon_pack: fas
    link: 'mailto:clb.attt@hcmut.edu.vn'
  - icon: twitter
    icon_pack: fab
    link: ''
  - icon: youtube
    icon_pack: fab
    link: ''
  - icon: facebook
    icon_pack: fab
    link: https://www.facebook.com/clbattthcmut
  - icon: google-scholar
    icon_pack: ai
    link: https://scholar.google.co.uk/citations?user=sIwtMXoAAAAJ
  - icon: github
    icon_pack: fab
    link: https://github.com/clbattthcmut
  - icon: globe
    icon_pack: fa
```

9-bis. social (Không bắt buộc) Link Resume/CV
   - Đính kèm CV, resume
   - Copy resume/CV của mọi người vào thư mục `static/files/<tên>.pdf` và thêm đoạn này vào cùng với social ở mục 9. trên (không cần nếu không muốn đính kèm CV/Resume)
```
# To enable, copy your resume/CV to `static/files/<tên>.pdf`
#social:
    - icon: cv
    icon_pack: ai
    link: files/<tên>.pdf
```

10. user_groups
   - User groups sẽ gom mọi người lại từng nhóm để thể hiện trên trang member
   - Trang mình gồm có 3 user group chính
      - Advisors: Dành cho Thầy Khương và các anh tham gia mentor hướng dẫn câu lạc bộ
      - Seniors: Các thành viên đã tốt nghiệp và thành viên từ câu lạc bộ đàn anh Efiens
      - Members: Thành viên hiện tại của câu lạc bộ
   - Một thành viên có thể thuộc 2 đến 3 nhóm khác nhau
```
user_groups:
  - Seniors
  - Advisors
```

## Phần nội dung (Markdown)
Phần này đăng khi bấm vào hình thành viên trên trang /member