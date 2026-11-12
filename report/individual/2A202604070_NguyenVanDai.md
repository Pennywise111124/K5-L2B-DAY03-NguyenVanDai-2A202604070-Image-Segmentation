# Báo cáo thực hành Day 3 — điền trực tiếp trong fork của bạn

**Cách dùng:** Thay mọi dấu `…` bằng bài làm thật của bạn trước khi nộp link fork trên [LMS](https://aithucchien.vinuni.edu.vn). Giữ nguyên bốn mục và bảng để coach đọc nhanh. Viết ngắn, cụ thể theo ảnh/vùng; không cần thuật ngữ chuyên sâu. Ví dụ trong [hướng dẫn mẫu](../../reports/REPORT_TEMPLATE.md) chỉ giúp hiểu cách điền, không phải câu trả lời để chép lại.

- Mã học viên theo lớp: 2A202604070
- Ngày / CVAT local: 11–12/11/2026 / dùng CVAT online (app.cvat.ai)
- Công cụ đã dùng: CVAT online với Polygon, Draw a mask (Brush/Eraser) và AI assistance (Segment Anything 3), dùng SAM 3 ở easy_semantic và medium_instance; script scripts/inspect_submissions.py để kiểm ZIP; scoring/score.py và scoring/scorecard.py để tự chấm sau khi coach phát reference

Mã học viên là mã lớp cấp; không cần ghi họ tên trong report nếu kênh [LMS](https://aithucchien.vinuni.edu.vn) đã nhận diện bạn. Chỉ ghi công cụ thật sự đã dùng; không có SAM vẫn làm bài bình thường.

## 1. Bài đã nộp

Ghi tên ZIP đúng như file trong `submissions/` và số ảnh đã vẽ, Save. Chưa làm hoặc export lỗi thì ghi `chưa có`, không tạo ZIP rỗng. Cột điểm là điểm tối đa của task, **không phải điểm tự chấm**.

| Task | File ZIP đúng tên | Hoàn thành mấy ảnh | Điểm tối đa (coach chấm sau) |
| --- | --- | ---: | ---: |
| easy_semantic | easy_semantic.zip | 3 / 3 | 20 |
| medium_instance | medium_instance.zip | 3 / 3 | 32 |
| hard_panoptic | hard_panoptic.zip | 2 / 2 | 30 |
| cp1_holes | cp1_holes.zip | 1 / 1 | 3 |
| cp2_slice | cp2_slice.zip | 1 / 1 | 3 |
| cp5_occlusion | cp5_occlusion.zip | 1 / 1 | 3 |
| cp3_thin | cp3_thin.zip | 1 / 1 | 3 |
| cp4_curb | cp4_curb.zip | 1 / 1 | 3 |
| cp6_coverage | cp6_coverage | 1 / 1 | 3 |
| **Tổng tối đa** | | | **100** |

Không có export lỗi. Cả 9 ZIP đều đã qua `scripts/inspect_submissions.py` với kết quả `OK`.

## 2. Một quyết định trước khi dùng gợi ý

Chọn object đầu tiên bạn tự vẽ ở `medium_instance`, trước khi xem bất kỳ đề xuất tự động nào cho object đó. Ghi ảnh/vị trí đủ để tìm lại; “quy tắc biên” là lý do bạn chọn hoặc dừng mask ở ranh đó.

- Ảnh, vị trí và object Medium đầu tiên tự vẽ: `000000181542.jpg`, xe máy bên trái (xe có người ngồi), nằm phía sau người phụ nữ mặc áo dài.
- Class và quy tắc tôi dùng để chọn biên: `person`. Tôi dừng mask sát người phụ nữ vì chỉ gán nhãn phần nhìn thấy, không gán nhãn cho phần phải đoán của người bị che.
- Nếu dùng gợi ý sau đó: vùng gợi ý sai/đúng, hành động sửa/giữ và lý do: ở ảnh 2 tôi dùng SAM 3 để tìm `person`. Tôi kiểm lại từng đề xuất, những mask mờ hoặc không nhìn rõ có phải người hay không thì xóa, giữ những mask rõ ràng. Ở ảnh 3 tôi làm tương tự, chỉ xóa bớt các label tôi không nhìn rõ hoặc không chắc chắn. Lý do: chỉ giữ nhãn cho thứ tôi nhìn thấy được, không đoán.
- Nếu không dùng gợi ý: ghi “không dùng”; vẫn giải thích một quyết định gán nhãn của mình.

## 3. Một lỗi tôi tìm thấy và sửa

Chọn một lỗi **có thật** trong bài. Nếu công cụ lỗi khiến bạn chưa sửa được, ghi rõ đã thử gì và cần coach hỗ trợ gì; không ghi “đã sửa” khi chưa sửa.

- Task/ảnh/vùng: `easy_semantic`, ảnh `81ae7cbb-6bc63a4a.jpg` (phố có cây trụi lá), các ô tô tối màu đỗ trên đường bên tay phải.
- Lỗi thuộc loại: sai lớp
- Bằng chứng tôi nhìn thấy: sau khi dùng SAM 3 cho class `vegetation`, mask `vegetation` phủ cả lên những chiếc ô tô tối màu, dù đó là xe chứ không phải cây.
- Quy tắc và hành động sửa: Easy chỉ có 5 class (road, sidewalk, building, vegetation, sky), xe không thuộc class nào nên phải để trống. Tôi xóa phần xe khỏi mask `vegetation` và không gán sang class khác.
- Sau sửa đã Save và export lại chưa? Đã Save ngay sau khi sửa. `easy_semantic.zip` được export sau lần sửa này nên đã chứa bản đã sửa.

Tôi đã tự chạy script sau khi coach phát reference: `easy_semantic` mIoU 0.836 (19.4/20), trong đó `vegetation` IoU 0.678 là class thấp nhất cùng `sidewalk` 0.719. Scorecard ba tier là 56.9/82; đây là điểm tự đánh giá. Lỗi ở trên là do tôi tự phát hiện trước khi xem reference, và sau khi xem điểm tôi không sửa thêm bài nào vì không check ra được lỗi. Không tự ghi PASS/top 3/bonus; người phụ trách xác nhận theo tiêu chí lớp. Không đưa file ground truth vào fork.

## 4. Ba ca chưa chắc hoặc đã cân nhắc

Mỗi ca là một **vùng cụ thể** khiến bạn phải cân nhắc hai cách hiểu. Ghi dấu hiệu nhìn thấy hoặc quy tắc đã dùng, rồi nêu quyết định hoặc câu hỏi cho coach. Không cần ba lỗi; ca đã quyết định được cũng hợp lệ.

| Ảnh/vị trí | Hai cách hiểu có thể | Quy tắc/chứng cứ | Quyết định hoặc câu hỏi cho coach |
| --- | --- | --- | --- |
| 1. `cp6_coverage`, ảnh `7daa6479-67988f3f.jpg`: xe buýt và xe tải lớn | Tô `car` cho cả hai vì chúng là xe, hoặc để trống vì không thuộc schema | `classes.json` của cp6 chỉ có road, sidewalk, building, vegetation, sky, car, person; không có bus hay truck | Để trống, không tô `car` vì là xe lớn không thuộc schema. Câu hỏi cho coach: cửa kính của tòa nhà có tính là `building` không? Tôi chưa chắc |
| 2. `cp4_curb`, ảnh `7d83710e-4697c3b2.jpg`: dải cỏ nằm trên vùng vỉa hè | Bỏ trống vì cỏ không thuộc `road` hay `sidewalk`, hoặc tô `sidewalk` vì nằm trong vùng vỉa hè | `classes.json` của cp4 chỉ có road và sidewalk; ranh road–sidewalk xác định theo bó vỉa và chức năng | Tôi tô `sidewalk` vì vùng cỏ vẫn nằm trên sidewalk. Câu hỏi cho coach: vì cp4 chỉ có hai class, dải cỏ này có nên để trống không? |
| 3. `cp3_thin`, ảnh `839f7736-abe28069.jpg`: thanh ngang dài và các biển báo nhỏ ở xa | Thanh ngang không tính là `pole`, biển quá nhỏ thì bỏ; hoặc thanh ngang thuộc cột và biển nhận ra được thì tô | Trạm này yêu cầu vẽ vật mảnh ở zoom lớn; thanh ngang nằm trong cùng một cột; biển nhỏ nhưng đoán được là biển báo | Tôi tô thanh ngang là `pole` vì nó nằm trong một cột, và vẫn tô các biển báo ở xa dù rất nhỏ vì nhận ra được |
