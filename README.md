# DCT Geo Attack

Bài thực hành Labtainers khảo sát tấn công hình học vào kỹ thuật giấu tin trong ảnh bằng DCT (Discrete Cosine Transform).

Sinh viên sẽ trực tiếp quan sát ảnh hưởng của translation, crop, rotation, scaling và composite geometric attack tới BER và khả năng khôi phục thông điệp.

---

# Mục tiêu

Sau khi hoàn thành bài lab, sinh viên có thể:

- Hiểu cơ chế giấu tin bằng DCT block 8x8.
- Hiểu vai trò của hệ số mid-frequency.
- Quan sát ảnh hưởng của geometric attack tới BER.
- Phân tích sự mất đồng bộ block DCT.
- Thử nghiệm defense bằng geometric resynchronization.
- Đánh giá trade-off giữa robustness, PSNR và capacity.

---

# Kiến thức chính

## DCT

DCT biến đổi ảnh từ miền không gian sang miền tần số.

Trong bài lab, thông điệp được nhúng vào cặp hệ số mid-frequency:

```text
(3,4) và (4,3)
```

Ý tưởng nhúng:

```text
bit = 1 nếu coeff(3,4) > coeff(4,3)
bit = 0 nếu coeff(3,4) < coeff(4,3)
```

---

## Geometric Attack

Các tấn công hình học làm mất đồng bộ block DCT:

- Translation
- Crop
- Rotation
- Scaling
- Composite attack

Khi block extraction không còn trùng với block nhúng, BER tăng mạnh.

---

# Tải bài lab

```bash
imodule https://github.com/KhoaPTIT/dct-geo-attack/raw/main/dct-geo-attack.tar
```

---

# Khởi động bài lab

```bash
cd ~/labtainer/trunk/scripts/labtainer-student
labtainer dct-geo-attack
```

Nếu cần rebuild:

```bash
rebuild dct-geo-attack
```

---

# Nội dung thực hành

## Task 1 — DCT Embedding

```bash
python3 embed_message.py --alpha 35 --repeat 5
python3 extract_message.py --image stego.png --repeat 5
```

Quan sát:

- PSNR
- BER baseline
- ảnh cover và stego

---

## Task 2 — DCT Visualization

```bash
python3 visualize_dct.py
display dct_spectrum_before.png
```

Quan sát:

- năng lượng tập trung ở low-frequency
- vùng mid-frequency dùng để nhúng

---

## Task 3 — Translation Attack

```bash
python3 attack_translation.py --dx 10 --dy 10
```

Quan sát:

- block DCT bị lệch
- BER tăng theo độ dịch chuyển

---

## Task 4 — Crop Attack

```bash
python3 attack_crop.py --percent 20
```

Quan sát:

- mất vùng biên ảnh
- resize làm sai block alignment

---

## Task 5 — Rotation Attack

```bash
python3 attack_rotation.py --angle 5
```

Quan sát:

- rotation phá đồng bộ mạnh
- BER tăng nhanh

---

## Task 6 — Scaling Attack

```bash
python3 attack_scaling.py --scale 75
```

Quan sát:

- nội suy làm thay đổi hệ số DCT
- BER tăng khi scale giảm

---

## Task 7 — Composite Attack

```bash
python3 attack_composite.py --angle 3 --dx 5 --dy 5 --crop 10
```

Quan sát:

- nhiều geometric transform cùng lúc
- BER thường cao nhất

---

## Task 8 — Resynchronization Defense

```bash
python3 registration_defense.py
```

Quan sát:

- defense thử khôi phục alignment
- BER giảm sau resynchronization

---

# Checkwork

```bash
checkwork
```

Kết quả mong đợi:

```text
Y - task1_embed
Y - task2_visual
Y - task3_translation
Y - task4_crop
Y - task5_rotation
Y - task6_scaling
Y - task7_composite
Y - task8_defense
```

---

# Kết luận mong đợi

Sau bài lab, sinh viên cần rút ra:

- DCT watermarking phụ thuộc mạnh vào block alignment.
- Geometric attack gây tăng BER do mất đồng bộ.
- Rotation và composite attack thường nguy hiểm nhất.
- Tăng alpha và repeat giúp tăng robustness nhưng giảm imperceptibility hoặc capacity.
- Resynchronization defense có thể giảm BER nhưng không khôi phục hoàn hảo.

---

# Thông tin học phần

Hoàng Anh Khoa — B22DCAT164

Lớp: D22CQAT04-B

Học phần: Kỹ thuật giấu tin (INT14102)

Học viện Công nghệ Bưu chính Viễn thông (PTIT)

Giảng viên hướng dẫn: PGS.TS. Đỗ Xuân Chợ
