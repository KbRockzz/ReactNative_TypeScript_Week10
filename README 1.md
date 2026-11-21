# BookStore · Buổi 10 · Bản hoàn thiện

Ứng dụng React Native dùng TypeScript, React Navigation và Redux Toolkit. Phần bắt buộc (Bài 10.1–10.10) dùng mảng dữ liệu có sẵn, không gọi API. Bài 10.11 là Bonus dùng `createAsyncThunk` và `json-server`. Hai thử thách lọc sách còn hàng và sắp xếp giá cũng đã được thêm vào Home.

## Chạy trên Snack

1. Mở https://snack.expo.dev và tạo Snack mới.
2. Tạo `App.tsx`, dán nội dung file tương ứng; xoá `App.js` mặc định nếu còn.
3. Tạo các file/thư mục theo cấu trúc project và dán nội dung từ các file trong gói.
4. Thêm các Dependencies sau:

```text
react-native-safe-area-context, react-native-screens, @react-navigation/native, @react-navigation/native-stack, @react-navigation/bottom-tabs, @reduxjs/toolkit, react-redux
```

5. Chọn Web hoặc Preview để chạy. `RefreshControl` cần kiểm tra bằng Preview iOS/Android hoặc Expo Go; bản Web không hỗ trợ thao tác kéo-để-làm-mới.
6. Chụp minh chứng sau từng bài theo tên `10.1.png` đến `10.10.png`. Câu hỏi bổ sung đã có trong `TraLoi.md`.

## Các bài đã triển khai

| Bài | Nội dung | File chính |
|---|---|---|
| 10.1 | SafeAreaProvider, SafeAreaView, header | `App.tsx`, `screens/HomeScreen.tsx` |
| 10.2 | FlatList, keyExtractor, ActivityIndicator | `screens/HomeScreen.tsx` |
| 10.3 | BookCard, props có kiểu, React.memo | `components/BookCard.tsx` |
| 10.4 | BookCard row/tile | `components/BookCard.tsx` |
| 10.5 | Switch lưới 2 cột, RefreshControl | `screens/HomeScreen.tsx` |
| 10.6 | Tìm kiếm không dấu, lọc thể loại | `screens/HomeScreen.tsx`, `utils.ts` |
| 10.7 | Native Stack, Bottom Tabs, route params | `navigation.tsx`, `screens/DetailScreen.tsx` |
| 10.8 | Redux store, cartSlice, Provider | `store/index.ts`, `store/cartSlice.ts`, `App.tsx` |
| 10.9 | Thêm vào giỏ, tăng/giảm, badge | `store/cartSlice.ts`, `screens/DetailScreen.tsx`, `navigation.tsx` |
| 10.10 | Favorites slice, selector theo id | `store/favoritesSlice.ts`, `screens/FavoritesScreen.tsx` |
| 10.11 Bonus | json-server, createAsyncThunk, trạng thái lỗi | `config.ts`, `store/booksSlice.ts`, `screens/HomeScreen.tsx` |
| Thử thách T1/T2 | Lọc còn hàng, sắp xếp giá tăng dần | `screens/HomeScreen.tsx` |

## Bonus 10.11 – chạy json-server

Mặc định `config.ts` có `USE_API = false`, nên ứng dụng chạy được không cần server. Nếu muốn thử Bonus:

1. Cài Node.js trên máy.
2. Mở terminal tại thư mục có `db.json` và chạy:

```bash
npx json-server@0.17.4 --watch db.json --port 3000 --host 0.0.0.0
```

3. Kiểm tra `http://localhost:3000/books`.
4. Đổi `USE_API = true` trong `config.ts`, sau đó reload Snack.
5. Để nộp bài và chạy không cần server, đổi lại `USE_API = false`.

Nếu chạy trên điện thoại/Expo Go, thay `API_URL` trong `config.ts` bằng địa chỉ IP LAN của máy đang chạy json-server, ví dụ `http://192.168.1.10:3000`.

## Lưu ý

- Ảnh bìa lấy từ URL internet; cần kết nối mạng để hiển thị.
- Nếu Snack báo thiếu package, thêm đúng các Dependencies ở trên.
- Các đoạn code đã được chú thích theo mã bài `BÀI 10.x` hoặc `BONUS`.
