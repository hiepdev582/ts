# TS

## TS Basics

### I. Note

1. Cần thực hiện "narrowing" (thu hẹp kiểu) bằng các câu lệnh điều kiện (như typeof) trước khi sử dụng các phương thức đặc thù của từng kiểu

### II. Overloading
1. Khai báo các dạng thức của hàm
```js
function reverse(s: string): string;
function reverse(s: string[]): string[];
function reverse(s: string | string[]): string | string[] {
  if (typeof s === "string") {
    return s.split("").reverse().join("");
  } else {
    return s.slice().reverse();
  }
}
```

### III. Index Signatures
1. `[index: string]: number;`

### IV. Keyof Type Operator
1. `keyof`: lấy một kiểu đối tượng và tạo ra một Union của các tên thuộc tính (keys) của đối tượng đó
```js
type Person = {
    name: string;
    age: number;
}
type K = keyof Person; // "name" | "age"
```

---
## TS Configurations
1. --noEmitOnError: Chặn hành vi gen file js khi lỗi kiểu
2. noImplicitAny: Chặn dùng any
3. strictNullChecks: Xử lý undefined/null chặt chẽ
