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
2. Kết hợp `typeof` với `keyof`
```js
const Colors = { red: "#ff0000", blue: "#0000ff" };
type ColorKeys = keyof typeof Colors; // "red" | "blue"
```

### V. Types
1. `ReturnType<T>`: Lấy kiểu trả về của một hàm
```js
function add(a: number, b: number) {
    return a + b;
}
type AddReturnType = ReturnType<typeof add>; // number
```
2. `InstanceType<T>`: Lấy kiểu thực thể của một class
```js
class Person {
    name: string;
    age: number;
    constructor(name: string, age: number) {
        this.name = name;
        this.age = age;
    }
}
type PersonInstance = InstanceType<typeof Person>; // Person
```
3. `NonNullable<T>`: Loại bỏ `null` và `undefined` khỏi một kiểu

### VI. Infer
1. `infer`: Chỉ dùng bên trong vế `extends` của một `Conditional Type`
```js
type IsArray<T> = T extends Array<infer U> ? U : never;
type ArrayExample = IsArray<number[]>; // number
```

---
## TS Configurations
1. --noEmitOnError: Chặn hành vi gen file js khi lỗi kiểu
2. noImplicitAny: Chặn dùng any
3. strictNullChecks: Xử lý undefined/null chặt chẽ
