# TS

## TS Basics

### I. Note

1. Cần thực hiện "narrowing" (thu hẹp kiểu) bằng các câu lệnh điều kiện (như typeof) trước khi sử dụng các phương thức đặc thù của từng kiểu
2. `const enum` không gen code ra file js, sử dụng khi cần tối ưu kích thước file và không cần truy cập runtime

### II. Overloading
1. Khai báo các dạng thức của hàm
```ts
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
```ts
type Person = {
    name: string;
    age: number;
}
type K = keyof Person; // "name" | "age"
```
2. Kết hợp `typeof` với `keyof`
```ts
const Colors = { red: "#ff0000", blue: "#0000ff" };
type ColorKeys = keyof typeof Colors; // "red" | "blue"
```

### V. Types
1. `ReturnType<T>`: Lấy kiểu trả về của một hàm
```ts
function add(a: number, b: number) {
    return a + b;
}
type AddReturnType = ReturnType<typeof add>; // number
```
2. `Parameters<Type>`: Lấy kiểu tham số của một hàm
```ts
function add(a: number, b: number) {
    return a + b;
}
type AddParameters = Parameters<typeof add>; // [number, number]
```
3. `Awaited<Type>`: Lấy kiểu của giá trị được return bởi một Promise (hoặc Union của các Promise).
4. `InstanceType<T>`: Lấy kiểu thực thể của một class
```ts
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
5. `NonNullable<T>`: Loại bỏ `null` và `undefined` khỏi một kiểu
6. `Partial<Type>`: Tạo ra một kiểu mới với tất cả các thuộc tính của `Type` là `optional`
7. `Required<Type>`: Tạo ra một kiểu mới với tất cả các thuộc tính của `Type` là `required`
8. `Readonly<Type>`: Tạo ra một kiểu mới với tất cả các thuộc tính của `Type` là `readonly`
9. `Pick<Type, Keys>`: Tạo ra một kiểu mới với các thuộc tính của `Type` được chọn bởi `Keys`
```ts
type PickPerson = Pick<Person, "name" | "age">;
// Kết quả:
// type PickPerson = {
//     name: string;
//     age: number;
// }
```
10. `Omit<Type, Keys>`: Loại bỏ các thuộc tính của `Type` được chọn bởi `Keys`
```ts
type OmitPerson = Omit<Person, "name" | "age">;
// Kết quả:
// type OmitPerson = {
//     location: string;
// }
```
11. `Exclude<UnionType, ExcludedMembers>`: Loại bỏ các kiểu dữ liệu cụ thể ra khỏi một Union Type
12. `Extract<UnionType, ExtractMembers>`: Chỉ lấy ra những kiểu dữ liệu có mặt trong cả hai Union
13. `Record<Keys, Type>`: Tạo ra một kiểu mới với các thuộc tính được chọn bởi `Keys`
```ts
type RecordPerson = Record<"name" | "age", string>;
// Kết quả:
// type RecordPerson = {
//     name: string;
//     age: string;
// }
```

### VI. Infer
1. `infer`: Chỉ dùng bên trong vế `extends` của một `Conditional Type`
```ts
type IsArray<T> = T extends Array<infer U> ? U : never;
type ArrayExample = IsArray<number[]>; // number
```

### VII. Mapped Types
```ts
type Getters<Type> = {
    [Property in keyof Type as `get${Capitalize<string & Property>}`]: () => Type[Property]
};
 
interface Person {
    name: string;
    age: number;
    location: string;
}
 
type LazyPerson = Getters<Person>;
// Kết quả:
// type LazyPerson = {
//     getName: () => string;
//     getAge: () => number;
//     getLocation: () => string;
// }
```

### VIII. Decorators
```ts
// 1. Định nghĩa Decorator
function Report(target: any, context: ClassMethodDecoratorContext) {
    const methodName = String(context.name);
    
    // Trả về hàm mới bọc lấy hàm cũ
    return function (this: any, ...args: any[]) {
        console.log(`[LOG] Đang gọi hàm: ${methodName}`);
        return target.call(this, ...args); // Thực thi hàm gốc
    };
}

// 2. Sử dụng Decorator
class MyApp {
    @Report
    saveData(data: string) {
        console.log(`Đang lưu dữ liệu: ${data}`);
    }
}

// 3. Chạy thử
const app = new MyApp();
app.saveData("Thông tin đơn hàng"); 

// Output:
// [LOG] Đang gọi hàm: saveData
// Đang lưu dữ liệu: Thông tin đơn hàng
```

### IX. Mixins
```ts
type Constructor = new (...args: any[]) => {};

function Scale<TBase extends Constructor>(Base: TBase) {
  return class extends Base {
    _scale = 1;
    setScale(scale: number) {
      this._scale = scale;
    }
  };
}
```

---
## TS Configurations
1. --noEmitOnError: Chặn hành vi gen file js khi lỗi kiểu
2. noImplicitAny: Chặn dùng any
3. strictNullChecks: Xử lý undefined/null chặt chẽ
