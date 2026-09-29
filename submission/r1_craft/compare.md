# So sánh L với R

Chỉ số L/R là thứ tự box cao ≥ H=40 trong từng frame, theo thứ tự XML; bắt đầu từ 1.
Box L trong ignore_region được báo IGNORE_SCOPE, không tính SPURIOUS.

## adasind_069450.jpg
- L4 edge IGNORE_SCOPE
## adasind_082170.jpg
- L2 edge IGNORE_SCOPE
- R5 edge MISSING
## adasind_102750.jpg
- L2 center SPURIOUS
- L4+R2 edge WRONG_CLASS
- R5 center MISSING

## Theo zone
| zone | n_ref | matched | missing | spurious |
|---|---|---|---|---|
| center | 6 | 5 | 1 | 1 |
| mid | 6 | 6 | 0 | 0 |
| edge | 6 | 4 | 2 | 1 |
