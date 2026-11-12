# Báo cáo thực hành Day 3 — điền trực tiếp trong fork của bạn

**Cách dùng:** Thay mọi dấu `…` bằng bài làm thật của bạn trước khi nộp link fork trên [LMS](https://aithucchien.vinuni.edu.vn). Giữ nguyên bốn mục và bảng để coach đọc nhanh. Viết ngắn, cụ thể theo ảnh/vùng; không cần thuật ngữ chuyên sâu. Ví dụ trong [hướng dẫn mẫu](../../reports/REPORT_TEMPLATE.md) chỉ giúp hiểu cách điền, không phải câu trả lời để chép lại.

- Mã học viên theo lớp: 2A202603044
- Ngày / CVAT local: 12/11/2026 / CVAT local (localhost:8080)
- Công cụ đã dùng: CVAT (Brush mask, Polygon trong thanh vẽ mask, Eraser, Remove underlying pixels); script `inspect_submissions.py` và GitHub Actions self-check

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
| cp5_occlusion | chưa có | 0 / 1 | 3 |
| cp3_thin | cp3_thin.zip | 1 / 1 | 3 |
| cp4_curb | cp4_curb.zip | 1 / 1 | 3 |
| cp6_coverage | chưa có | 0 / 1 | 3 |
| **Tổng tối đa** | | | **100** |

Nếu export lỗi, ghi task, dữ liệu đã Save đến đâu và lỗi đã báo coach.

## 2. Một quyết định trước khi dùng gợi ý

- Ảnh, vị trí và object Medium đầu tiên tự vẽ: 000000181542.jpg, [người phụ nữ áo dài trắng ở giữa ảnh / xe máy bên trái / ... ], vẽ bằng [Brush / Polygon] trước khi thử gợi ý nào.
- Class và quy tắc tôi dùng để chọn biên: class `[person / motorcycle / car]`. Quy tắc: chỉ vẽ phần nhìn thấy; [ví dụ: dừng mask ở viền vạt áo và không lấn sang xe máy che phía trước / tách người lái và xe máy thành hai object riêng / không tô bóng đổ dưới bánh xe].
- Nếu dùng gợi ý sau đó: không dùng gợi ý tự động.
- Nếu không dùng gợi ý: không dùng. Một quyết định gán nhãn của tôi: [ví dụ: người ngồi trên xe máy gán `person`, xe gán `motorcycle`, hai object riêng dù chồng lên nhau].

## 3. Một lỗi tôi tìm thấy và sửa

Chọn một lỗi **có thật** trong bài. Nếu công cụ lỗi khiến bạn chưa sửa được, ghi rõ đã thử gì và cần coach hỗ trợ gì; không ghi “đã sửa” khi chưa sửa.

- Task/ảnh/vùng: hard_panoptic, lớp `road` (stuff)
- Lỗi thuộc loại: phủ vùng (thiếu lớp road)
- Bằng chứng tôi nhìn thấy: sau khi export, script `inspect_submissions.py` báo "chưa thấy stuff class: road" cho hard_panoptic.zip, dù ảnh đường phố có mặt đường.
- Quy tắc và hành động sửa: road là stuff nên mọi vùng đường nhìn thấy phải có mask `road`. Tôi mở lại task trong CVAT, kiểm tra và sửa mask/label `road`, Save, export lại COCO 1.0.
- Sau sửa đã Save và export lại chưa? Đã Save, export lại, xóa hard_panoptic.zip cũ và upload bản mới lên `submissions/`.

## 4. Ba ca chưa chắc hoặc đã cân nhắc

Mỗi ca là một **vùng cụ thể** khiến bạn phải cân nhắc hai cách hiểu. Ghi dấu hiệu nhìn thấy hoặc quy tắc đã dùng, rồi nêu quyết định hoặc câu hỏi cho coach. Không cần ba lỗi; ca đã quyết định được cũng hợp lệ.

| Ảnh/vị trí | Hai cách hiểu có thể | Quy tắc/chứng cứ | Quyết định hoặc câu hỏi cho coach |
| --- | --- | --- | --- |
| easy_semantic, ảnh 7ee6d192-89e2408b (cao tốc), sườn đồi cỏ khô màu nâu | Là `vegetation` (cỏ là thực vật), hoặc để trống | Cây bụi xanh sẫm rõ ràng là vegetation; cỏ khô nâu không giống cây xanh | Chỉ tô mảng xanh sẫm, để trống cỏ khô. Câu hỏi cho coach: cỏ khô có tính vegetation không? |
| easy_semantic, ảnh 7ee6d192-89e2408b, dải rào bê tông bên trái và xe trên đường | Là `sidewalk`/`building`, hoặc để trống | Rào bê tông không phải vỉa hè và không thuộc 5 lớp; xe cũng không thuộc 5 lớp | Để trống rào bê tông và xe, không tô thành `road` |
| easy_semantic, ảnh 7ee6d192-89e2408b, lề đường phía trên vạch vàng bên trái | Là `road` (nhựa liền với mặt đường), hoặc để trống vì sát chân rào | Phân loại theo chức năng, không theo màu | Câu hỏi cho coach: lề đường sát chân rào có tính `road` không? |
