# Buổi 10 – Trả lời câu hỏi bổ sung

## Câu 1. Vì sao khi đổi `numColumns` của FlatList phải đổi cả `key`?

`FlatList` không hỗ trợ đổi trực tiếp `numColumns` khi đang chạy. Đổi `key={String(numColumns)}` khiến React huỷ danh sách cũ và tạo danh sách mới với số cột mới. Nếu không đổi `key`, khi bật/tắt Switch có thể gặp lỗi “Changing numColumns on the fly is not supported”.

## Câu 2. Dữ liệu nào nên để trong useState, dữ liệu nào nên đưa vào Redux?

Dữ liệu chỉ phục vụ một màn hình như `keyword`, `isTile`, `category` nên để trong `useState` của Home. Dữ liệu được nhiều màn hình sử dụng như giỏ hàng (Chi tiết, Giỏ hàng và badge) và danh sách yêu thích (Chi tiết, tab Yêu thích) nên đưa vào Redux để các màn hình cùng đọc và cập nhật.

## Câu 3. Vì sao trong createSlice có thể viết state.items.push(...) và found.quantity += 1?

Redux Toolkit dùng Immer bên trong reducer. Immer cung cấp một bản nháp (draft) của state; thao tác sửa trên draft sẽ được ghi nhận và tạo ra state mới, không sửa state gốc. Trong component thông thường không có Immer bảo vệ nên không được sửa trực tiếp state.

## Câu 4. Vì sao selectIsFavorite(id) trả về một hàm? Chỉ lưu id có lợi gì?

`selectIsFavorite(id)` nhận id rồi trả về selector đọc state, nhờ đó `useAppSelector` có thể kiểm tra đúng sách đang xem. Chỉ lưu id giúp tránh lưu trùng toàn bộ thông tin sách; tên, giá và rating luôn lấy từ một nguồn dữ liệu sách duy nhất nên không bị lệch khi thông tin sách thay đổi.

## Câu 5. Nếu bỏ useCallback ở onSelect thì React.memo còn tác dụng không?

Hiệu quả memoization sẽ bị mất trong trường hợp này. Mỗi lần Home render, `onSelect` sẽ là một hàm mới; props của từng `BookCard` vì thế thay đổi. `React.memo` nhận thấy props khác và vẫn render lại các thẻ.

## Câu 6 (Bonus). Ba hành động của createAsyncThunk cập nhật state nào?

- `pending`: đặt `status = "loading"` và xoá lỗi bằng `error = null`.
- `fulfilled`: đặt `status = "succeeded"` và cập nhật `items = action.payload`.
- `rejected`: đặt `status = "failed"` và lưu thông báo vào `error` (dùng thông báo mặc định nếu không có payload).
