# Visibility report

- Thư mục nhãn: `dataset\labels\train`
- 20 ảnh, 29 skeleton, trung bình 15.93 khớp có v > 0 mỗi người
- Tổng: v=2 343 | v=1 119 | v=0 31

| # | Khớp | v=2 | v=1 | v=0 | %v=1 |
| ---: | --- | ---: | ---: | ---: | ---: |
| 0 | nose | 26 | 3 | 0 | 10% |
| 1 | left_eye | 23 | 6 | 0 | 21% |
| 2 | right_eye | 25 | 4 | 0 | 14% |
| 3 | left_ear | 11 | 18 | 0 | 62% |
| 4 | right_ear | 17 | 12 | 0 | 41% |
| 5 | left_shoulder | 25 | 4 | 0 | 14% |
| 6 | right_shoulder | 28 | 1 | 0 | 3% |
| 7 | left_elbow | 22 | 7 | 0 | 24% |
| 8 | right_elbow | 23 | 6 | 0 | 21% |
| 9 | left_wrist | 18 | 11 | 0 | 38% |
| 10 | right_wrist | 19 | 9 | 1 | 31% |
| 11 | left_hip | 23 | 5 | 1 | 17% |
| 12 | right_hip | 21 | 7 | 1 | 24% |
| 13 | left_knee | 17 | 7 | 5 | 24% |
| 14 | right_knee | 17 | 7 | 5 | 24% |
| 15 | left_ankle | 16 | 4 | 9 | 14% |
| 16 | right_ankle | 12 | 8 | 9 | 28% |

## Đọc bảng này thế nào

1. Khớp nào có **%v=1 cao**: khớp hay bị che. Cổ tay và hông thường là hai vị trí cần xem lại guideline trước khi kết luận.
2. Khớp nào có **v=0 cao bất thường**: mọi người đang dùng Outside ở chỗ đáng lẽ là Occluded. Đó là lỗi số 3 của slide 46, và nó xoá thẳng khớp đó khỏi bảng điểm OKS.
3. Khi so hai người: **lệch lớn = bất đồng về guideline**, không phải về bức ảnh. Sửa guideline trước, sửa nhãn sau.
