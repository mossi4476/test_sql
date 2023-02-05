# test_sql
1/ <br>
A<br>
SELECT MaNhanVien, HoVaTen, TenPhongBan, TenChucVu, NgaySinh<br>
FROM NhanVien<br>
JOIN PhongBan ON NhanVien.PhongBanID = PhongBan.PhongBanID<br>
JOIN NS_DSChucVu ON NhanVien.ChucVuID = NS_DSChucVu.ChucVuID;<br>
