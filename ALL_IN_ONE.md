# BookStore Buổi 10 – ALL IN ONE

Mỗi tiêu đề là đường dẫn file cần tạo trong Snack.


## `App.tsx`

```tsx
import React from "react";
import { Provider } from "react-redux";
import { NavigationContainer } from "@react-navigation/native";
import { SafeAreaProvider } from "react-native-safe-area-context";
import { RootNavigator } from "./navigation";
import { store } from "./store";

// ===== BÀI 10.1a: SafeAreaProvider =====
// ===== BÀI 10.7c: NavigationContainer + RootNavigator =====
// ===== BÀI 10.8c: Redux Provider =====
export default function App() {
  return (
    <Provider store={store}>
      <SafeAreaProvider>
        <NavigationContainer>
          <RootNavigator />
        </NavigationContainer>
      </SafeAreaProvider>
    </Provider>
  );
}
```


## `components/BookCard.tsx`

```tsx
import React from "react";
import { Image, StyleSheet, Text, TouchableOpacity, View } from "react-native";
import type { Book } from "../types";
import { formatPrice } from "../utils";
import { colors } from "../theme";

// ===== BÀI 10.3a: Props có kiểu =====
export interface BookCardProps {
  book: Book;
  layout?: "row" | "tile";
  onSelect: (id: string) => void;
}

// ===== BÀI 10.3b, 10.4: Một BookCard hỗ trợ row và tile =====
function BookCard({ book, layout = "row", onSelect }: BookCardProps) {
  const isTile = layout === "tile";

  return (
    <TouchableOpacity
      testID={`book-${book.id}`}
      activeOpacity={0.8}
      onPress={() => onSelect(book.id)}
      style={[styles.card, isTile && styles.cardTile]}
    >
      <View style={[styles.coverBox, isTile && styles.coverBoxTile]}>
        <Image source={{ uri: book.cover }} style={styles.cover} resizeMode="cover" />
        {isTile && (
          <Text style={styles.ratingBadge}>⭐ {book.rating.toFixed(1)}</Text>
        )}
      </View>

      <View style={[styles.info, isTile && styles.infoTile]}>
        <Text style={styles.title} numberOfLines={isTile ? 1 : 2}>
          {book.title}
        </Text>

        {!isTile && (
          <Text style={styles.sub}>{book.author} · {book.category}</Text>
        )}

        <View style={styles.row}>
          <Text style={styles.price}>{formatPrice(book.price)}</Text>
          {!isTile && <Text style={styles.sub}>⭐ {book.rating.toFixed(1)}</Text>}
          <Text>{book.inStock ? "✅" : "❌"}</Text>
        </View>
      </View>
    </TouchableOpacity>
  );
}

// ===== BÀI 10.3c: React.memo tránh render lại khi props không đổi =====
export default React.memo(BookCard);

const styles = StyleSheet.create({
  card: { flexDirection: "row", backgroundColor: "#fff", borderRadius: 14, padding: 10, marginBottom: 10, borderWidth: 1, borderColor: colors.line, gap: 12 },
  cardTile: { flexDirection: "column", width: "48%", gap: 8 },
  coverBox: { width: 70, height: 100, borderRadius: 8, overflow: "hidden", backgroundColor: colors.indigo },
  coverBoxTile: { width: "100%", height: "auto", aspectRatio: 2 / 3 },
  cover: { width: "100%", height: "100%" },
  ratingBadge: { position: "absolute", top: 6, right: 6, backgroundColor: "rgba(0,0,0,0.65)", color: "#fff", fontSize: 11, fontWeight: "700", paddingHorizontal: 6, paddingVertical: 2, borderRadius: 8, overflow: "hidden" },
  info: { flex: 1, gap: 4, justifyContent: "center" },
  infoTile: { flexGrow: 0, flexBasis: "auto" },
  title: { fontSize: 15, fontWeight: "800", color: colors.text },
  sub: { fontSize: 12, color: colors.muted },
  row: { flexDirection: "row", alignItems: "center", gap: 10 },
  price: { color: colors.indigo, fontWeight: "800" },
});
```


## `config.ts`

```tsx
// Cấu hình cho Bài 10.11 (Bonus, không bắt buộc).
// USE_API = false: đọc dữ liệu từ mảng trong data/books.ts (mặc định của bài thực hành).
// USE_API = true : gọi json-server qua fetch.
export const USE_API = false;

// Trình duyệt chạy cùng máy với json-server: dùng localhost.
// Điện thoại hoặc Expo Go: thay localhost bằng IP LAN của máy chạy json-server, ví dụ "http://192.168.1.10:3000".
export const API_URL = "http://localhost:3000";
```


## `data/books.ts`

```tsx
import type { Book } from "../types";

// Dữ liệu cho sẵn trong đề: một mảng, không gọi API.
export const BOOKS: Book[] = [
  { id: "1", title: "Clean Code", author: "Robert C. Martin", price: 389000, rating: 9.1, category: "Lập trình", inStock: true, description: "Cuốn sách kinh điển về cách viết code sạch, dễ đọc và dễ bảo trì.", cover: "https://picsum.photos/seed/book1/300/450" },
  { id: "2", title: "Dế Mèn Phiêu Lưu Ký", author: "Tô Hoài", price: 65000, rating: 8.7, category: "Thiếu nhi", inStock: true, description: "Cuộc phiêu lưu của chú dế mèn, tác phẩm quen thuộc của nhiều thế hệ.", cover: "https://picsum.photos/seed/book2/300/450" },
  { id: "3", title: "Nhà Giả Kim", author: "Paulo Coelho", price: 79000, rating: 8.9, category: "Văn học", inStock: true, description: "Hành trình đi tìm kho báu của cậu bé chăn cừu Santiago.", cover: "https://picsum.photos/seed/book3/300/450" },
  { id: "4", title: "Đắc Nhân Tâm", author: "Dale Carnegie", price: 86000, rating: 9.0, category: "Kinh tế", inStock: true, description: "Những nguyên tắc giao tiếp và ứng xử được đọc nhiều nhất thế giới.", cover: "https://picsum.photos/seed/book4/300/450" },
  { id: "5", title: "React Native in Action", author: "Nader Dabit", price: 259000, rating: 8.2, category: "Lập trình", inStock: false, description: "Xây dựng ứng dụng di động bằng React Native từ cơ bản đến nâng cao.", cover: "https://picsum.photos/seed/book5/300/450" },
  { id: "6", title: "Sapiens", author: "Yuval Noah Harari", price: 189000, rating: 9.2, category: "Khoa học", inStock: true, description: "Lược sử loài người từ thời nguyên thuỷ đến hiện đại.", cover: "https://picsum.photos/seed/book6/300/450" },
  { id: "7", title: "Tôi Thấy Hoa Vàng Trên Cỏ Xanh", author: "Nguyễn Nhật Ánh", price: 95000, rating: 8.8, category: "Văn học", inStock: true, description: "Tuổi thơ ở một làng quê nghèo, nhẹ nhàng mà sâu lắng.", cover: "https://picsum.photos/seed/book7/300/450" },
  { id: "8", title: "Atomic Habits", author: "James Clear", price: 159000, rating: 9.0, category: "Kinh tế", inStock: false, description: "Xây dựng thói quen tốt bằng những thay đổi rất nhỏ mỗi ngày.", cover: "https://picsum.photos/seed/book8/300/450" },
  { id: "9", title: "Vũ Trụ Trong Vỏ Hạt Dẻ", author: "Stephen Hawking", price: 175000, rating: 8.6, category: "Khoa học", inStock: true, description: "Giải thích các ý tưởng vật lý lớn bằng ngôn ngữ dễ hiểu.", cover: "https://picsum.photos/seed/book9/300/450" },
  { id: "10", title: "Truyện Kiều", author: "Nguyễn Du", price: 72000, rating: 8.5, category: "Văn học", inStock: true, description: "Kiệt tác thơ lục bát của văn học Việt Nam.", cover: "https://picsum.photos/seed/book10/300/450" },
];

// Giả lập việc tải dữ liệu: trả về mảng sau một khoảng trễ để thấy ActivityIndicator
// và RefreshControl. Hàm này không truy cập mạng.
export function loadBooks(delay = 700): Promise<Book[]> {
  return new Promise((resolve) => setTimeout(() => resolve([...BOOKS]), delay));
}

export const getBookById = (id: string): Book | undefined => BOOKS.find((b) => b.id === id);
```


## `navigation.tsx`

```tsx
import React from "react";
import { Text } from "react-native";
import { createBottomTabNavigator } from "@react-navigation/bottom-tabs";
import { createNativeStackNavigator } from "@react-navigation/native-stack";
import { HomeScreen } from "./screens/HomeScreen";
import { DetailScreen } from "./screens/DetailScreen";
import { CartScreen } from "./screens/CartScreen";
import { FavoritesScreen } from "./screens/FavoritesScreen";
import { useAppSelector } from "./store/hooks";
import { selectCartCount } from "./store/cartSlice";
import { colors } from "./theme";

export type RootStackParamList = {
  Tabs: undefined;
  Detail: { id: string };
};
export type TabParamList = {
  Home: undefined;
  Cart: undefined;
  Favorites: undefined;
};

const Stack = createNativeStackNavigator<RootStackParamList>();
const Tab = createBottomTabNavigator<TabParamList>();

// ===== BÀI 10.7b, 10.9c: Bottom Tabs và badge giỏ hàng =====
function Tabs() {
  const count = useAppSelector(selectCartCount);

  return (
    <Tab.Navigator
      screenOptions={{
        headerShown: false,
        tabBarActiveTintColor: colors.indigo,
      }}
    >
      <Tab.Screen
        name="Home"
        component={HomeScreen}
        options={{ title: "Sách", tabBarIcon: () => <Text>📚</Text> }}
      />
      <Tab.Screen
        name="Cart"
        component={CartScreen}
        options={{
          title: "Giỏ hàng",
          tabBarIcon: () => <Text>🛒</Text>,
          tabBarBadge: count > 0 ? count : undefined,
        }}
      />
      <Tab.Screen
        name="Favorites"
        component={FavoritesScreen}
        options={{ title: "Yêu thích", tabBarIcon: () => <Text>♥</Text> }}
      />
    </Tab.Navigator>
  );
}

// ===== BÀI 10.7a: Native Stack gồm Tabs và Detail =====
export function RootNavigator() {
  return (
    <Stack.Navigator
      screenOptions={{
        headerStyle: { backgroundColor: colors.navy },
        headerTintColor: "#fff",
      }}
    >
      <Stack.Screen
        name="Tabs"
        component={Tabs}
        options={{ headerShown: false }}
      />
      <Stack.Screen
        name="Detail"
        component={DetailScreen}
        options={{ title: "Chi tiết sách" }}
      />
    </Stack.Navigator>
  );
}
```


## `screens/CartScreen.tsx`

```tsx
import React from "react";
import { FlatList, StyleSheet, Text, TouchableOpacity, View } from "react-native";
import { SafeAreaView } from "react-native-safe-area-context";
import { useAppDispatch, useAppSelector } from "../store/hooks";
import { addToCart, clearCart, decrease, removeFromCart, selectCartItems, selectCartTotal } from "../store/cartSlice";
import { formatPrice } from "../utils";
import { colors } from "../theme";

export function CartScreen() {
  const dispatch = useAppDispatch();
  const items = useAppSelector(selectCartItems);
  const total = useAppSelector(selectCartTotal);

  return (
    <SafeAreaView style={styles.root} edges={["top"]}>
      <View style={styles.header}><Text style={styles.title}>🛒 Giỏ hàng</Text></View>
      <FlatList
        data={items}
        keyExtractor={(i) => i.bookId}
        contentContainerStyle={{ padding: 14, flexGrow: 1 }}
        ListEmptyComponent={<Text style={styles.empty}>Giỏ hàng trống.</Text>}
        renderItem={({ item }) => (
          <View style={styles.row} testID={`cart-${item.bookId}`}>
            <View style={{ flex: 1 }}>
              <Text style={styles.name} numberOfLines={2}>{item.title}</Text>
              <Text style={styles.price}>{formatPrice(item.price)}</Text>
            </View>
            <TouchableOpacity style={styles.qtyBtn} onPress={() => dispatch(decrease(item.bookId))} testID={`dec-${item.bookId}`}><Text style={styles.qtyText}>−</Text></TouchableOpacity>
            <Text style={styles.qty}>{item.quantity}</Text>
            <TouchableOpacity style={styles.qtyBtn} onPress={() => dispatch(addToCart({ id: item.bookId, title: item.title, price: item.price }))} testID={`inc-${item.bookId}`}><Text style={styles.qtyText}>＋</Text></TouchableOpacity>
            <TouchableOpacity onPress={() => dispatch(removeFromCart(item.bookId))}><Text style={{ fontSize: 18 }}>🗑️</Text></TouchableOpacity>
          </View>
        )}
      />
      <View style={styles.footer}>
        <Text style={styles.total} testID="cart-total">Tổng cộng: {formatPrice(total)}</Text>
        <TouchableOpacity style={[styles.clear, items.length === 0 && { opacity: 0.4 }]} disabled={items.length === 0} onPress={() => dispatch(clearCart())} testID="clear-cart">
          <Text style={styles.clearText}>Xoá giỏ</Text>
        </TouchableOpacity>
      </View>
    </SafeAreaView>
  );
}

const styles = StyleSheet.create({
  root: { flex: 1, backgroundColor: colors.bg },
  header: { backgroundColor: colors.navy, padding: 14 },
  title: { color: "#fff", fontSize: 20, fontWeight: "800" },
  empty: { textAlign: "center", color: colors.muted, marginTop: 40 },
  row: { flexDirection: "row", alignItems: "center", gap: 10, backgroundColor: "#fff", padding: 12, borderRadius: 12, marginBottom: 10, borderWidth: 1, borderColor: colors.line },
  name: { fontWeight: "700", color: colors.text },
  price: { color: colors.indigo, fontWeight: "700" },
  qtyBtn: { width: 30, height: 30, borderRadius: 15, backgroundColor: colors.bg, alignItems: "center", justifyContent: "center" },
  qtyText: { fontSize: 18, fontWeight: "800", color: colors.navy },
  qty: { minWidth: 18, textAlign: "center", fontWeight: "800" },
  footer: { flexDirection: "row", alignItems: "center", justifyContent: "space-between", backgroundColor: "#fff", padding: 14, borderTopWidth: 1, borderTopColor: colors.line },
  total: { fontSize: 17, fontWeight: "800", color: colors.navy },
  clear: { borderWidth: 1.5, borderColor: colors.red, paddingHorizontal: 14, paddingVertical: 8, borderRadius: 10 },
  clearText: { color: colors.red, fontWeight: "800" },
});
```


## `screens/DetailScreen.tsx`

```tsx
import React from "react";
import { Image, ScrollView, StyleSheet, Text, TouchableOpacity, View } from "react-native";
import type { NativeStackScreenProps } from "@react-navigation/native-stack";
import type { RootStackParamList } from "../navigation";
import { useAppDispatch, useAppSelector } from "../store/hooks";
import { addToCart } from "../store/cartSlice";
import { selectIsFavorite, toggleFavorite } from "../store/favoritesSlice";
import { getBookById } from "../data/books";
import { formatPrice } from "../utils";
import { colors } from "../theme";

export function DetailScreen({ route }: NativeStackScreenProps<RootStackParamList, "Detail">) {
  const { id } = route.params; // ===== BÀI 10.7d: Nhận id từ Home =====
  const dispatch = useAppDispatch(); // ===== BÀI 10.9b =====
  const favorite = useAppSelector(selectIsFavorite(id)); // ===== BÀI 10.10b =====
  const book = getBookById(id);

  if (!book) return <Text style={{ padding: 20 }}>Không tìm thấy sách.</Text>;

  return (
    <ScrollView contentContainerStyle={styles.box}>
      <Image source={{ uri: book.cover }} style={styles.cover} resizeMode="cover" />
      <Text style={styles.title}>{book.title}</Text>
      <Text style={styles.sub}>{book.author} · {book.category}</Text>
      <Text style={styles.sub}>⭐ {book.rating.toFixed(1)} / 10</Text>
      <Text style={styles.price}>
        {formatPrice(book.price)}  {book.inStock ? "✅ Còn hàng" : "❌ Hết hàng"}
      </Text>
      <Text style={styles.desc}>{book.description}</Text>

      <View style={styles.actions}>
        <TouchableOpacity
          style={[styles.btn, !book.inStock && { opacity: 0.4 }]}
          disabled={!book.inStock}
          // ===== BÀI 10.9b: Dispatch thêm sách vào Redux cart =====
          onPress={() => dispatch(addToCart(book))}
          testID="add-cart"
        >
          <Text style={styles.btnText}>Thêm vào giỏ</Text>
        </TouchableOpacity>

        <TouchableOpacity
          style={[styles.btn, styles.btnFav, favorite && styles.btnFavOn]}
          // ===== BÀI 10.10b: Thêm / bỏ yêu thích =====
          onPress={() => dispatch(toggleFavorite(book.id))}
          testID="toggle-fav"
        >
          <Text style={[styles.btnText, !favorite && { color: colors.indigo }]}>
            {favorite ? "♥ Đã yêu thích" : "♡ Yêu thích"}
          </Text>
        </TouchableOpacity>
      </View>
    </ScrollView>
  );
}

const styles = StyleSheet.create({
  box: { padding: 16, gap: 8, backgroundColor: colors.bg },
  cover: { width: "100%", aspectRatio: 2 / 3, maxHeight: 360, borderRadius: 12, backgroundColor: colors.indigo },
  title: { fontSize: 22, fontWeight: "800", color: colors.navy, marginTop: 6 },
  sub: { color: colors.muted },
  price: { fontSize: 18, fontWeight: "800", color: colors.indigo },
  desc: { color: colors.text, lineHeight: 21 },
  actions: { flexDirection: "row", gap: 10, marginTop: 10 },
  btn: { flex: 1, backgroundColor: colors.indigo, paddingVertical: 12, borderRadius: 10, alignItems: "center" },
  btnFav: { backgroundColor: "#fff", borderWidth: 1.5, borderColor: colors.indigo },
  btnFavOn: { backgroundColor: colors.red, borderColor: colors.red },
  btnText: { color: "#fff", fontWeight: "800" },
});
```


## `screens/FavoritesScreen.tsx`

```tsx
import React, { useCallback } from "react";
import { FlatList, StyleSheet, Text, View } from "react-native";
import { SafeAreaView } from "react-native-safe-area-context";
import { useNavigation } from "@react-navigation/native";
import type { NativeStackNavigationProp } from "@react-navigation/native-stack";
import type { RootStackParamList } from "../navigation";
import { useAppSelector } from "../store/hooks";
import { selectFavoriteIds } from "../store/favoritesSlice";
import { BOOKS } from "../data/books";
import BookCard from "../components/BookCard";
import { colors } from "../theme";

export function FavoritesScreen() {
  const navigation = useNavigation<NativeStackNavigationProp<RootStackParamList>>();
  const ids = useAppSelector(selectFavoriteIds);

  // ===== BÀI 10.10c: Redux chỉ lưu id, thông tin sách lấy từ mảng gốc =====
  const favorites = BOOKS.filter((book) => ids.includes(book.id));
  const onSelect = useCallback(
    (id: string) => navigation.navigate("Detail", { id }),
    [navigation]
  );

  return (
    <SafeAreaView style={styles.root} edges={["top"]}>
      <View style={styles.header}>
        <Text style={styles.title}>♥ Yêu thích</Text>
      </View>
      <FlatList
        data={favorites}
        keyExtractor={(book) => book.id}
        contentContainerStyle={{ padding: 14, flexGrow: 1 }}
        renderItem={({ item }) => <BookCard book={item} onSelect={onSelect} />}
        ListEmptyComponent={<Text style={styles.empty}>Chưa có sách yêu thích.</Text>}
      />
    </SafeAreaView>
  );
}

const styles = StyleSheet.create({
  root: { flex: 1, backgroundColor: colors.bg },
  header: { backgroundColor: colors.navy, padding: 14 },
  title: { color: "#fff", fontSize: 20, fontWeight: "800" },
  empty: { textAlign: "center", color: colors.muted, marginTop: 40 },
});
```


## `screens/HomeScreen.tsx`

```tsx
import React, { useCallback, useEffect, useMemo, useState } from "react";
import {
  ActivityIndicator, FlatList, RefreshControl, StyleSheet, Switch,
  Text, TextInput, TouchableOpacity, View,
} from "react-native";
import { SafeAreaView } from "react-native-safe-area-context";
import type { CompositeScreenProps } from "@react-navigation/native";
import type { BottomTabScreenProps } from "@react-navigation/bottom-tabs";
import type { NativeStackScreenProps } from "@react-navigation/native-stack";
import type { RootStackParamList, TabParamList } from "../navigation";
import { loadBooks } from "../data/books";
import { normalize } from "../utils";
import type { Book } from "../types";
import BookCard from "../components/BookCard";
import { colors } from "../theme";
import { useAppDispatch, useAppSelector } from "../store/hooks";
import { fetchBooks, selectBooksState } from "../store/booksSlice";
import { USE_API } from "../config";

type Props = CompositeScreenProps<
  BottomTabScreenProps<TabParamList, "Home">,
  NativeStackScreenProps<RootStackParamList>
>;

export function HomeScreen({ navigation }: Props) {
  const dispatch = useAppDispatch();
  const api = useAppSelector(selectBooksState);

  const [books, setBooks] = useState<Book[]>([]);
  const [loading, setLoading] = useState(true);
  const [refreshing, setRefreshing] = useState(false);
  const [keyword, setKeyword] = useState("");
  const [category, setCategory] = useState<string | null>(null);
  const [isTile, setIsTile] = useState(false); // ===== BÀI 10.4, 10.5: Switch =====
  const [onlyInStock, setOnlyInStock] = useState(false); // ===== THỬ THÁCH T1 =====
  const [sortByPrice, setSortByPrice] = useState(false); // ===== THỬ THÁCH T2 =====
  const [useArray, setUseArray] = useState(!USE_API); // ===== BÀI 10.11c BONUS =====

  // ===== BÀI 10.11c BONUS: Tải dữ liệu từ mảng khi chọn chế độ mặc định =====
  const loadFromArray = useCallback(async () => {
    setLoading(true);
    const data = await loadBooks();
    setBooks(data);
    setLoading(false);
  }, []);

  useEffect(() => {
    if (USE_API) {
      dispatch(fetchBooks());
    } else {
      loadFromArray();
    }
  }, [dispatch, loadFromArray]);

  // ===== BÀI 10.5b: Kéo để làm mới; giữ nguyên layout đang chọn =====
  // ===== BÀI 10.11c BONUS: Refresh theo nguồn dữ liệu hiện tại =====
  const onRefresh = useCallback(async () => {
    setRefreshing(true);
    try {
      if (useArray) {
        setBooks(await loadBooks());
      } else {
        await dispatch(fetchBooks());
      }
    } finally {
      setRefreshing(false);
    }
  }, [dispatch, useArray]);

  // ===== BÀI 10.11c BONUS: Cho phép quay lại dùng mảng khi API lỗi =====
  const switchToArray = useCallback(() => {
    setUseArray(true);
    loadFromArray();
  }, [loadFromArray]);

  const sourceBooks = useArray ? books : api.items;
  const isLoading = useArray
    ? loading
    : api.status === "idle" || (api.status === "loading" && api.items.length === 0);
  const apiFailed = !useArray && api.status === "failed";

  // ===== BÀI 10.6: useMemo tạo danh sách thể loại =====
  const categories = useMemo(
    () => Array.from(new Set(sourceBooks.map((book) => book.category))),
    [sourceBooks]
  );

  // ===== BÀI 10.6: Tìm không dấu theo tên/tác giả và lọc category =====
  const visible = useMemo(() => {
    const normalizedKeyword = normalize(keyword);
    const filtered = sourceBooks.filter(
      (book) =>
        (category === null || book.category === category) &&
        (!onlyInStock || book.inStock) &&
        (
          normalizedKeyword === "" ||
          normalize(book.title).includes(normalizedKeyword) ||
          normalize(book.author).includes(normalizedKeyword)
        )
    );
    // THỬ THÁCH T2: sort bản sao, không mutate mảng state/store.
    return sortByPrice
      ? [...filtered].sort((a, b) => a.price - b.price)
      : filtered;
  }, [sourceBooks, keyword, category, onlyInStock, sortByPrice]);

  // ===== BÀI 10.7d: Điều hướng sang Chi tiết và truyền id =====
  const onSelect = useCallback(
    (id: string) => navigation.navigate("Detail", { id }),
    [navigation]
  );

  // ===== BÀI 10.5a: numColumns thay đổi theo Switch =====
  const numColumns = isTile ? 2 : 1;

  return (
    // ===== BÀI 10.1b: SafeAreaView chừa vùng an toàn phía trên =====
    <SafeAreaView style={styles.root} edges={["top"]}>
      <View style={styles.header}>
        <Text style={styles.title}>📚 BookStore</Text>
        <TextInput
          style={styles.search}
          placeholder="Tìm theo tên sách hoặc tác giả…"
          placeholderTextColor="#9AA3B8"
          value={keyword}
          onChangeText={setKeyword}
          testID="book-search"
        />
        <View style={styles.switchRow}>
          <Text style={styles.switchLabel}>Dạng lưới</Text>
          <Switch value={isTile} onValueChange={setIsTile} testID="grid-switch" />
        </View>
      </View>

      <View style={styles.chips}>
        {[null, ...categories].map((c) => (
          <TouchableOpacity
            key={c ?? "all"}
            style={[styles.chip, category === c && styles.chipOn]}
            onPress={() => setCategory(c)}
            testID={`chip-${c ?? "all"}`}
          >
            <Text style={[styles.chipText, category === c && styles.chipTextOn]}>
              {c ?? "Tất cả"}
            </Text>
          </TouchableOpacity>
        ))}
      </View>

      {/* ===== THỬ THÁCH T1, T2: Lọc còn hàng và sắp xếp giá tăng dần ===== */}
      <View style={styles.extraFilters}>
        <View style={styles.stockFilter}>
          <Text style={styles.filterLabel}>Chỉ còn hàng</Text>
          <Switch value={onlyInStock} onValueChange={setOnlyInStock} testID="stock-switch" />
        </View>
        <TouchableOpacity
          style={[styles.chip, sortByPrice && styles.chipOn]}
          onPress={() => setSortByPrice((value) => !value)}
          testID="sort-price"
        >
          <Text style={[styles.chipText, sortByPrice && styles.chipTextOn]}>
            {sortByPrice ? "Giá tăng dần ✓" : "Giá tăng dần"}
          </Text>
        </TouchableOpacity>
      </View>

      {/* ===== BÀI 10.11c BONUS: Trạng thái lỗi API và nút thử lại / dùng mảng ===== */}
      {apiFailed && sourceBooks.length === 0 ? (
        <View style={styles.center} testID="api-error">
          <Text style={styles.errorTitle}>Không tải được danh sách sách</Text>
          <Text style={styles.errorText}>{api.error}</Text>
          <View style={styles.errorActions}>
            <TouchableOpacity
              style={styles.errBtn}
              onPress={() => dispatch(fetchBooks())}
              testID="retry"
            >
              <Text style={styles.errBtnText}>Thử lại</Text>
            </TouchableOpacity>
            <TouchableOpacity
              style={[styles.errBtn, styles.errBtnAlt]}
              onPress={switchToArray}
              testID="use-array"
            >
              <Text style={[styles.errBtnText, { color: colors.indigo }]}>
                Dùng dữ liệu mảng
              </Text>
            </TouchableOpacity>
          </View>
        </View>
      ) : isLoading ? (
        // ===== BÀI 10.2b: ActivityIndicator trong lúc tải =====
        <View style={styles.center}>
          <ActivityIndicator size="large" color={colors.indigo} testID="loading" />
          <Text style={styles.empty}>Đang tải sách…</Text>
        </View>
      ) : (
        // ===== BÀI 10.2a, 10.3d, 10.5a-b: FlatList, BookCard, grid và refresh =====
        <FlatList
          key={String(numColumns)} // đổi key để FlatList được dựng lại khi đổi số cột
          data={visible}
          keyExtractor={(book) => book.id}
          numColumns={numColumns}
          columnWrapperStyle={isTile ? styles.columnWrapper : undefined}
          renderItem={({ item }) => (
            <BookCard
              book={item}
              layout={isTile ? "tile" : "row"}
              onSelect={onSelect}
            />
          )}
          contentContainerStyle={{ padding: 14, paddingBottom: 30 }}
          refreshControl={
            <RefreshControl refreshing={refreshing} onRefresh={onRefresh} />
          }
          ListHeaderComponent={
            apiFailed ? (
              <Text style={styles.banner} testID="api-banner">
                Không làm mới được: {api.error}
              </Text>
            ) : null
          }
          ListEmptyComponent={
            <Text style={styles.empty}>Không tìm thấy sách phù hợp.</Text>
          }
        />
      )}
    </SafeAreaView>
  );
}

const styles = StyleSheet.create({
  root: { flex: 1, backgroundColor: colors.bg },
  header: { backgroundColor: colors.navy, paddingHorizontal: 14, paddingBottom: 12, paddingTop: 6, gap: 8 },
  title: { color: "#fff", fontSize: 22, fontWeight: "800" },
  search: { backgroundColor: "#fff", borderRadius: 10, paddingHorizontal: 12, paddingVertical: 9 },
  switchRow: { flexDirection: "row", alignItems: "center", justifyContent: "flex-end", gap: 8 },
  switchLabel: { color: "#D5DDF7", fontWeight: "600" },
  chips: { flexDirection: "row", flexWrap: "wrap", gap: 8, paddingHorizontal: 14, paddingTop: 10 },
  chip: { paddingHorizontal: 12, paddingVertical: 6, borderRadius: 16, backgroundColor: "#fff", borderWidth: 1, borderColor: colors.line },
  chipOn: { backgroundColor: colors.indigo, borderColor: colors.indigo },
  chipText: { color: colors.text, fontWeight: "600", fontSize: 13 },
  chipTextOn: { color: "#fff" },
  columnWrapper: { justifyContent: "space-between" },
  extraFilters: { flexDirection: "row", alignItems: "center", justifyContent: "space-between", paddingHorizontal: 14, paddingTop: 8 },
  stockFilter: { flexDirection: "row", alignItems: "center", gap: 6 },
  filterLabel: { color: colors.text, fontSize: 12, fontWeight: "600" },
  center: { flex: 1, alignItems: "center", justifyContent: "center", gap: 10 },
  errorTitle: { fontSize: 16, fontWeight: "700", color: colors.text },
  errorText: { color: colors.muted, textAlign: "center", paddingHorizontal: 24 },
  errorActions: { flexDirection: "row", gap: 10, marginTop: 6 },
  errBtn: { backgroundColor: colors.indigo, paddingHorizontal: 16, paddingVertical: 10, borderRadius: 10, borderWidth: 1, borderColor: colors.indigo },
  errBtnAlt: { backgroundColor: "#fff" },
  errBtnText: { color: "#fff", fontWeight: "700" },
  banner: { color: "#B42318", marginBottom: 8, textAlign: "center" },
  empty: { textAlign: "center", color: colors.muted, marginTop: 30 },
});
```


## `store/booksSlice.ts`

```tsx
import { createAsyncThunk, createSlice } from "@reduxjs/toolkit";
import type { Book } from "../types";
import { API_URL } from "../config";
import type { RootState } from "./index";

type Status = "idle" | "loading" | "succeeded" | "failed";

// ===== BÀI 10.11a (BONUS): Gọi json-server bằng createAsyncThunk =====
export const fetchBooks = createAsyncThunk<Book[], void, { rejectValue: string }>(
  "books/fetchBooks",
  async (_, { rejectWithValue }) => {
    try {
      const response = await fetch(`${API_URL}/books`);
      if (!response.ok) {
        return rejectWithValue(`Máy chủ trả về lỗi ${response.status}`);
      }
      return (await response.json()) as Book[];
    } catch {
      return rejectWithValue(`Không kết nối được tới ${API_URL}`);
    }
  }
);

const booksSlice = createSlice({
  name: "books",
  initialState: {
    items: [] as Book[],
    status: "idle" as Status,
    error: null as string | null,
  },
  reducers: {},
  // ===== BÀI 10.11b (BONUS): Xử lý pending / fulfilled / rejected =====
  extraReducers: (builder) => {
    builder
      .addCase(fetchBooks.pending, (state) => {
        state.status = "loading";
        state.error = null;
      })
      .addCase(fetchBooks.fulfilled, (state, action) => {
        state.status = "succeeded";
        state.items = action.payload;
      })
      .addCase(fetchBooks.rejected, (state, action) => {
        state.status = "failed";
        state.error = action.payload ?? "Không tải được dữ liệu";
      });
  },
});

export default booksSlice.reducer;
export const selectBooksState = (s: RootState) => s.books;
```


## `store/cartSlice.ts`

```tsx
import { createSlice, type PayloadAction } from "@reduxjs/toolkit";
import type { Book, CartItem } from "../types";
import type { RootState } from "./index";

const cartSlice = createSlice({
  name: "cart",
  initialState: { items: [] as CartItem[] },
  reducers: {
    // ===== BÀI 10.8a, 10.9b: Thêm sách; sách trùng thì tăng số lượng =====
    addToCart(state, action: PayloadAction<Pick<Book, "id" | "title" | "price">>) {
      const { id, title, price } = action.payload;
      const found = state.items.find((item) => item.bookId === id);
      if (found) {
        found.quantity += 1;
      } else {
        state.items.push({ bookId: id, title, price, quantity: 1 });
      }
    },

    // ===== BÀI 10.9a: Giảm số lượng; nếu còn 1 thì xoá khỏi giỏ =====
    decrease(state, action: PayloadAction<string>) {
      const found = state.items.find((item) => item.bookId === action.payload);
      if (!found) return;
      if (found.quantity > 1) {
        found.quantity -= 1;
      } else {
        state.items = state.items.filter((item) => item.bookId !== action.payload);
      }
    },

    removeFromCart(state, action: PayloadAction<string>) {
      state.items = state.items.filter((item) => item.bookId !== action.payload);
    },
    clearCart(state) {
      state.items = [];
    },
  },
});

export const { addToCart, decrease, removeFromCart, clearCart } = cartSlice.actions;
export default cartSlice.reducer;

export const selectCartItems = (s: RootState) => s.cart.items;

// ===== BÀI 10.8b, 10.9c: Selector tổng số lượng và tổng tiền =====
export const selectCartCount = (s: RootState) =>
  s.cart.items.reduce((count, item) => count + item.quantity, 0);

export const selectCartTotal = (s: RootState) =>
  s.cart.items.reduce((total, item) => total + item.price * item.quantity, 0);
```


## `store/favoritesSlice.ts`

```tsx
import { createSlice, type PayloadAction } from "@reduxjs/toolkit";
import type { RootState } from "./index";

const favoritesSlice = createSlice({
  name: "favorites",
  initialState: { ids: [] as string[] },
  reducers: {
    // ===== BÀI 10.10a: Chỉ lưu id; có rồi thì bỏ, chưa có thì thêm =====
    toggleFavorite(state, action: PayloadAction<string>) {
      const index = state.ids.indexOf(action.payload);
      if (index >= 0) state.ids.splice(index, 1);
      else state.ids.push(action.payload);
    },
  },
});

export const { toggleFavorite } = favoritesSlice.actions;
export default favoritesSlice.reducer;

export const selectFavoriteIds = (s: RootState) => s.favorites.ids;

// ===== BÀI 10.10b: Selector có tham số id =====
export const selectIsFavorite = (id: string) =>
  (s: RootState) => s.favorites.ids.includes(id);
```


## `store/hooks.ts`

```tsx
import { useDispatch, useSelector, type TypedUseSelectorHook } from "react-redux";
import type { AppDispatch, RootState } from "./index";

export const useAppDispatch = () => useDispatch<AppDispatch>();
export const useAppSelector: TypedUseSelectorHook<RootState> = useSelector;
```


## `store/index.ts`

```tsx
import { configureStore } from "@reduxjs/toolkit";
import cartReducer from "./cartSlice";
import favoritesReducer from "./favoritesSlice";
import booksReducer from "./booksSlice";

export const store = configureStore({
  reducer: { cart: cartReducer, favorites: favoritesReducer, books: booksReducer },
});

export type RootState = ReturnType<typeof store.getState>;
export type AppDispatch = typeof store.dispatch;
```


## `theme.ts`

```tsx
export const colors = {
  navy: "#0B1F5C", indigo: "#3B3FD8", cyan: "#00B8D9", bg: "#F4F6FB",
  text: "#1F2430", muted: "#6B7280", line: "#E2E6F0", red: "#DC2626", green: "#16A34A",
};
```


## `types.ts`

```tsx
export interface Book {
  id: string;
  title: string;
  author: string;
  price: number;
  rating: number; // thang 0 đến 10
  category: string;
  inStock: boolean;
  description: string;
  cover: string; // URL ảnh bìa
}

export interface CartItem {
  bookId: string;
  title: string;
  price: number;
  quantity: number;
}
```


## `utils.ts`

```tsx
// 389000 -> "389.000 đ"
export const formatPrice = (n: number) => n.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ".") + " đ";

// Bỏ dấu tiếng Việt để "nha gia" tìm ra "Nhà Giả Kim"
export const normalize = (s: string) =>
  s.normalize("NFD").replace(/[̀-ͯ]/g, "").replace(/đ/g, "d").replace(/Đ/g, "D").toLowerCase().trim();
```
