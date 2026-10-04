# Vệ Binh Vương Quốc — UI/UX prototype

## Chạy demo

Mở **index.html** bằng Edge hoặc Chrome (nhấp đúp, không cần cài engine, npm hay internet). Nhạc phát sau thao tác đầu tiên. Có thể mở qua localhost khi phát triển. Toàn màn hình dùng API trình duyệt; thoát bằng Esc. Nút Thoát hiển thị hướng dẫn đóng tab vì trình duyệt không cho trang tự đóng tab do người dùng mở.

Luồng chính: Menu → chọn chiến trường → hướng dẫn → xây trụ → gọi đợt → kết quả → màn tiếp theo. Đổi tướng tại doanh trại. Trong trận: Esc tạm dừng, Space gọi đợt, 1 mưa lửa, 2 băng giá. Bấm trụ đã xây để nâng cấp/bán. Hai nút “Xem UI…” xem trước kết quả mà không lưu phần thưởng hay mở khóa màn.

Trận mô phỏng có 3/4/5 đợt tùy màn. Ngân quỹ đầu trận 350 + 50 hỗ trợ. Trụ tự đánh trong tầm; mục tiêu lọt cổng trừ 1 sinh lực. Hoàn thành mọi đợt với sinh lực > 0 sẽ thắng. Các chân dung trụ không phải sprite trận đấu; địch là ký hiệu ◆ tạm. Chỉ số anh hùng là minh họa, các anh hùng cùng cho +50 vàng; chưa có hệ thống di chuyển tướng/equipment.

Tiến độ, cài đặt, tướng và trận dở được lưu bằng localStorage trên trình duyệt hiện tại. Bản mở bằng file và bản localhost có vùng lưu riêng. Trình duyệt chặn lưu trữ sẽ hiện thông báo. “Bắt đầu” đến chọn màn, “Tiếp tục” khôi phục trận dở; “Chơi lại” tạo trận mới.

## Phân tích tài nguyên gốc

- Chỉ có 5 thư mục asset, không có code, scene engine, cấu hình resolution hay project Unity/Godot/Phaser. Không thể xác định engine/gameplay gốc từ thư mục này.
- 59 PNG: 20 ảnh chiến trường (18 số màn, một số có biến thể), 1 menu, 1 loading, 1 world map; 12 chân dung anh hùng, 15 chân dung trụ, 9 chân dung phép. 45 OGG.
- Portrait: 1180 × 1120 RGBA. Phần lớn stage: 3940 × 2160 (~1.824:1). Menu: 3500 × 2432. Loading: 2732 × 1536. World map: 4852 × 2780. Chi tiết từng tệp ở **asset-audit.json**.
- Art: minh họa 2D fantasy, đường viền đậm, màu bão hòa, khối sáng/tối rõ; không phải pixel art. Bản đồ có đường đi gợi ý tower defense; đây là suy luận từ asset, không phải gameplay gốc đã được xác minh.
- Không thấy enemy/boss sprite, tilemap dữ liệu, equipment/item/weapon độc lập, font, button/frame UI, spritesheet, animation clip, particle, PSD/SVG hoặc script. Lửa/băng được vẽ sẵn trong portrait, không phải effect động riêng.
- Đã xem toàn bộ PNG theo contact sheet và xem riêng 3 chiến trường sử dụng trong demo. Nhạc được kiểm tra tải trên trình duyệt, không đánh giá toàn bộ 45 bản nhạc bằng nghe.

## Hướng thiết kế

UI gỗ nâu, viền vàng và nút xanh rêu theo màu cây cỏ/giáp/vật liệu trong ảnh. Portrait và nền giữ hình ảnh gốc, không tạo bộ art mới. Tên game, tên tướng, luật và chỉ số là tên thử nghiệm của prototype.

| Token | Giá trị |
|---|---|
| Primary / vàng | #E8BD69 |
| Secondary / rêu | #6A8D38 |
| Accent / băng | #72C7D9 |
| Background | #121C1D |
| Panel | #493122, #2A2520 |
| Danger | #BB4941 |
| Success | #6A8D38 |
| Text | #F8EBC6 |

Typography: Times New Roman serif cho title (65/46px bold), heading (34/23px bold), button (17px bold). Arial cho body/HUD (13–17px), nhãn (10–13px). Font hệ thống hỗ trợ tiếng Việt, không dùng CDN. Spacing: 4/8/12/16/24/32px, safe area 24–42px. Viền 2–3px, bóng đổ và cạnh nổi nhẹ.

Component: primary/secondary/icon buttons; default/hover/pressed/disabled/selected/focus; panel/modal; tabs; range sliders; select; checkbox; tooltip (title); resource counter; health/cooldown; portrait card; tower slot; toast; result card. Animation fade/popup 0.2–0.3s, hover/pressed, thanh sức khỏe; tùy chọn giảm chuyển động. Không có inventory vì thiếu item và cơ chế equipment gốc.

Khung logic 1280 × 720, 16:9; co đồng đều theo viewport, giữ letterbox nếu lệch tỷ lệ. Map phủ toàn vùng trận bằng ảnh nền; vì asset có tỷ lệ 1.824:1, bản map trong trận có điều chỉnh rất nhẹ sang 16:9. Portrait dùng contain. Cài đặt độ phân giải giới hạn kích thước khung lên 1280 hoặc 1920 chiều rộng. Đây là mức hiển thị của prototype, không phải resolution đã tìm thấy trong engine gốc. Desktop/landscape ưu tiên; màn dọc nhỏ vẫn fit toàn khung nhưng chữ nhỏ.

HUD tập trung sinh lực/vàng/đợt ở góc trên, tướng/phép/gọi đợt ở cạnh dưới; vùng trung tâm dành cho đường đi và trụ. Một CTA chính mỗi bước. Pause giữ trận phía sau overlay. Modal chặn mô phỏng và có vòng focus bàn phím. Chọn màn phân biệt locked/unlocked/completed/stars/kỷ lục bằng cả chữ và trạng thái nút.

## Màn hình đã tạo

Main Menu, Level Select (3 màn), Character/Doanh trại (12 tướng), Gameplay HUD + trụ/phép, hướng dẫn, Pause, Settings (Audio/Display/Gameplay), Game Over, Victory, Credits, Exit dialog. Inventory không áp dụng. Không thêm splash bắt người chơi chờ.

## Tái sử dụng asset

- Menu: screen_main_menu_bg-1; doanh trại: loading_images_desktop-1; level select: map_bg-1.
- Trận/thumbnail: go_stage07_bg-1, go_stage03_bg-1, go_stage14_bg-1.
- Tướng: room_tower_hero_portraits_big-1…12.
- Trụ trong demo: room_tower_tower_portraits_big-11/4/9; phép: room_tower_power_portraits_big-2/1.
- Nhạc: kr6_bgmusic_Menu_v5.ogg, kr6_bgmusic_t1_battle1_v3.ogg, kr5_bgmusic_t3_boss_victory.ogg. SFX click được tổng hợp Web Audio.
- Assets/ chứa bản WebP giảm kích thước của cả 59 ảnh để dễ tiếp tục thiết kế, và 3 nhạc dùng. Asset gốc vẫn nguyên vẹn.

## Tệp và kiểm tra

Tệp mới trong UI/: index.html, style.css, app.js, assets.js, asset-audit.json, README.md, Assets/ (59 WebP + 3 OGG), và Screenshots/ (ảnh kiểm tra). Không chỉnh sửa/xóa tệp gốc; chỉ thêm UI/.

JavaScript kiểm tra cú pháp bằng node --check. Kiểm tra mô phỏng bằng Node VM: victory mở khóa, defeat, mục tiêu lọt cổng, pause đóng băng thời gian/sinh lực, hồi chiêu, lưu trận. Kiểm tra trên trình duyệt: menu/chọn màn, xây trụ trừ 100 vàng, gọi đợt cập nhật HUD, Pause/Settings và 3 tab, xem UI thắng/thua, chọn tướng; kiểm tra ảnh tại khung mặc định và viewport 960 × 540. Các kiểm tra không xác minh cân bằng gameplay thực tế hay engine gốc vì chưa có mã nguồn đó.
