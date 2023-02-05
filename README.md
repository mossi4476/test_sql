# test_sql
1/ <br>
A<br>
SELECT MaNhanVien, HoVaTen, TenPhongBan, TenChucVu, NgaySinh<br>
FROM NhanVien<br>
JOIN PhongBan ON NhanVien.PhongBanID = PhongBan.PhongBanID<br>
JOIN NS_DSChucVu ON NhanVien.ChucVuID = NS_DSChucVu.ChucVuID;<br>

B<br>

SELECT NhanVien.MaNhanVien, HoVaTen, MaPhongBan <br>
FROM NhanVien <br>
JOIN PhongBan ON NhanVien.PhongBanID = PhongBan.PhongBanID <br>
WHERE PhongBan.PhongBanID = (SELECT MIN(PhongBanID) FROM PhongBan) <br>
LIMIT 0, 1000<br>

2/ <br>

A <br>

SELECT NhanVien.MaNhanVien, COUNT(NhanVienID) as SoLuongNhanVien <br>
FROM PhongBan <br>
JOIN NhanVien ON NhanVien.PhongBanID = PhongBan.PhongBanID <br>
GROUP BY MaPhongBan, TenPhongBan; <br>

B <br>
SELECT MaChucVu, TenChucVu FROM NS_DSChucVu WHERE ChucVuID NOT IN (SELECT ChucVuID FROM NhanVien);

3/ <br>

A <br>
SELECT MaNhanVien, HoVaTen, NgaySinh, QueQuan <br>
FROM NhanVien <br>
WHERE QueQuan LIKE '%Thai Binh%' AND YEAR(NgaySinh) > 1990; <br>

B <br>
SELECT MaNhanVien, HoVaTen, NgaySinh <br>
FROM NhanVien <br
WHERE NgaySinh IN (SELECT NgaySinh FROM NhanVien GROUP BY NgaySinh HAVING COUNT(NhanVienID) > 1) <br>

4/ <br>
SELECT NhanVien.NhanVienID, HoVaTen, ns_dsloaihopdong.TenLoaiHopDong, NgayBatDau, NgayKetThuc <br>
FROM NhanVien  <br
JOIN (SELECT NhanVienID, MAX(NgayKyHD) AS MaxNgayKyHD <br>
      FROM ns_hopdong <br>
      GROUP BY NhanVienID) AS HD  <br>
ON NhanVien.NhanVienID = HD.NhanVienID  <br>
JOIN ns_hopdong  <br
ON NhanVien.NhanVienID = ns_hopdong.NhanVienID  <br>
AND HD.MaxNgayKyHD = ns_hopdong.NgayKyHD  <br>
JOIN ns_dsloaihopdong  <br
ON ns_hopdong.LoaiHopDongID = ns_dsloaihopdong.LoaiHopDongID <br>
LIMIT 0, 1000 <br>

5/ <br>
A <br>
SELECT NhanVien.MaNhanVien, NhanVien.HoVaTen, <br>
       SUBSTRING(PhongBan.TenPhongBan, 4) AS PhongBan <br>
FROM NhanVien <br>
INNER JOIN PhongBan ON NhanVien.PhongBanID = PhongBan.PhongBanID <br>

B <br>
SELECT pb.MaPhongBan, SUM(IF(nv.GioiTinh = 'Nam', 1, 0)) AS SoLuongNam, SUM(IF(nv.GioiTinh = 'Nu', 1, 0)) AS SoLuongNu, SUM(IF(cv.TenChucVu = 'Truong_phong', 1, 0)) AS SoLuongTruongPhong, COUNT(nv.NhanVienID) AS SoLuongNhanVien <br>
FROM PhongBan pb <br>
JOIN NhanVien nv ON pb.PhongBanID = nv.PhongBanID <br>
JOIN NS_DSChucVu cv ON nv.ChucVuID = cv.ChucVuID <br>
GROUP BY pb.MaPhongBan; <br>

6/ <br>
A <br>
SELECT NhanVien.MaNhanVien, NhanVien.HoVaTen, PhongBan.TenPhongBan, ns_hopdong.NgayBatDau <br>
FROM NhanVien <br>
JOIN PhongBan ON NhanVien.PhongBanID = PhongBan.PhongBanID <br>
JOIN ns_hopdong ON NhanVien.MaNhanVien = ns_hopdong.NhanVienID <br>
WHERE MONTH(ns_hopdong.NgayBatDau) = @thang AND YEAR(ns_hopdong.NgayBatDau) = @nam <br>
LIMIT 0, 1000 <br>

B <br>
SELECT NhanVien.MaNhanVien, NhanVien.HoVaTen, PhongBan.TenPhongBan, ns_hopdong.NgayBatDau <br>
FROM NhanVien <br>
JOIN PhongBan ON NhanVien.PhongBanID = PhongBan.PhongBanID <br>
JOIN ns_hopdong ON NhanVien.MaNhanVien = ns_hopdong.NhanVienID <br>
WHERE ns_hopdong.NgayBatDau < LAST_DAY(CONCAT(@nam, '-', @thang, '-', '01')) <br>


 
7/ <br>
SELECT PhongBan.MaPhongBan, SUM(IF(MONTH(NhanVien.NgaySinh) = 1, 1, 0)) AS T1, SUM(IF(MONTH(NhanVien.NgaySinh) = 2, 1, 0)) AS T2, SUM(IF(MONTH(NhanVien.NgaySinh) = 3, 1, 0)) AS T3, SUM(IF(MONTH(NhanVien.NgaySinh) = 4, 1, 0)) AS T4, SUM(IF(MONTH(NhanVien.NgaySinh) = 5, 1, 0)) AS T5, SUM(IF(MONTH(NhanVien.NgaySinh) = 6, 1, 0)) AS T6, SUM(IF(MONTH(NhanVien.NgaySinh) = 7, 1, 0)) AS T7, SUM(IF(MONTH(NhanVien.NgaySinh) = 8, 1, 0)) AS T8, SUM(IF(MONTH(NhanVien.NgaySinh) = 9, 1, 0)) AS T9, SUM(IF(MONTH(NhanVien.NgaySinh) = 10, 1, 0)) AS T10, SUM(IF(MONTH(NhanVien.NgaySinh) = 11, 1, 0)) AS T11, SUM(IF(MONTH(NhanVien.NgaySinh) = 12, 1, 0)) AS T12 <br>
FROM NhanVien <br>
JOIN PhongBan ON NhanVien.PhongBanID = PhongBan.PhongBanID <br>
GROUP BY PhongBan.MaPhongBan <br>
LIMIT 0, 1000 <br>

8/ <br>
SELECT NhanVien.MaNhanVien, NhanVien.HoVaTen, <br>
       HopDongMoiNhat.LoaiHopDongID AS LoaiHopDongMoiNhat, <br>
       HopDongMoiNhat.NgayBatDau AS NgayBatDauMoiNhat, <br>
       HopDongLienTruoc.LoaiHopDongID AS LoaiHopDongLienTruoc, <br>
       HopDongLienTruoc.NgayBatDau AS NgayBatDauLienTruoc <br>
FROM NhanVien  <br>
LEFT JOIN ( <br>
    SELECT NhanVienID, LoaiHopDongID, NgayBatDau <br>
    FROM ns_hopdong <br>
    WHERE NgayBatDau = ( <br>
        SELECT MAX(NgayBatDau) <br>
        FROM ns_hopdong <br>
        WHERE NhanVienID = ns_hopdong.NhanVienID <br>
    ) <br>
) HopDongMoiNhat  <br>
ON NhanVien.MaNhanVien = HopDongMoiNhat.NhanVienID  <br>
LEFT JOIN ( <br>
    SELECT NhanVienID, LoaiHopDongID, NgayBatDau <br>
    FROM ns_hopdong <br>
    WHERE NgayBatDau = ( <br>
        SELECT MAX(NgayBatDau) <br>
        FROM ns_hopdong <br>
        WHERE NhanVienID = ns_hopdong.NhanVienID <br>
        AND NgayBatDau < ns_hopdong.NgayBatDau <br>
    ) <br>
) HopDongLienTruoc  <br>
ON NhanVien.MaNhanVien = HopDongLienTruoc.NhanVienID <br>
LIMIT 0, 1000 <br>

9/ <br>
A <br>
CREATE PROCEDURE GenerateCalendar (@thang INT, @nam INT) <br>
BEGIN <br>
    DECLARE @start_date DATETIME, @end_date DATETIME, @current_date DATETIME <br>
    DECLARE @tbl TABLE (Ngay DATETIME, Thu INT) <br>
    
    SET @start_date = DATEFROMPARTS(@nam, @thang, 1) <br>
    SET @end_date = EOMONTH(@start_date) <br>
    SET @current_date = @start_date <br>
    
    WHILE (@current_date <= @end_date) <br>
    BEGIN <br>
        INSERT INTO @tbl (Ngay, Thu) <br>
        VALUES (@current_date, DATEPART(WEEKDAY, @current_date)) <br>
        
        SET @current_date = DATEADD(DAY, 1, @current_date) <br>
    END <br>
    
    SELECT * FROM @tbl <br>
END <br>
Sau đó dùng Stored Procedures với tham số @thang, @nam như sau: <br>
EXEC GenerateCalendar @thang = 2, @nam = 2022 <br>
B <br>
CREATE PROCEDURE GenerateCalendar (IN p_thang INT, IN p_nam INT) <br>
BEGIN <br>
    DECLARE v_start_date DATE, v_end_date DATE, v_current_date DATE; <br>
    DECLARE v_tbl_count INT DEFAULT 0; <br>

    SET v_start_date = DATE_FORMAT(MAKEDATE(p_nam, 1), '%Y-%m-%d'); <br>
    SET v_end_date = LAST_DAY(CONCAT(p_nam, '-', p_thang, '-01')); <br>
    SET v_current_date = v_start_date; <br>

    CREATE TEMPORARY TABLE tmp_calendar (ngay DATE, thu INT); <br>

    WHILE v_current_date <= v_end_date DO <br>
        INSERT INTO tmp_calendar (ngay, thu) <br>
        VALUES (v_current_date, WEEKDAY(v_current_date) + 1); <br>
        SET v_current_date = DATE_ADD(v_current_date, INTERVAL 1 DAY); <br>
        SET v_tbl_count = v_tbl_count + 1; <br>
    END WHILE; <br>

    INSERT INTO SinhNhat (Ngay, NhanVien) <br>
    SELECT NgaySinh, HoVaTen <br>
    FROM NhanVien <br>
    WHERE MONTH(NgaySinh) = p_thang AND YEAR(NgaySinh) = p_nam; <br>

END <br>


CREATE TABLE IF NOT EXISTS SinhNhat ( <br>
  Ngay datetime, <br>
  NhanVien nvarchar(200) <br>
);
INSERT INTO SinhNhat (Ngay, NhanVien) <br>
SELECT NgaySinh, HoVaTen <br>
FROM NhanVien <br>
WHERE MONTH(NgaySinh) = p_thang AND YEAR(NgaySinh) = p_nam; <br>


SET @p_thang = 2; <br>
SET @p_nam = 2022; <br>

CALL GenerateCalendar(@p_thang, @p_nam); <br>












