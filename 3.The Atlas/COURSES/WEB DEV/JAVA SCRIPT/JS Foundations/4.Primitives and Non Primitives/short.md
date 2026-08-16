### 4. Primitives vs Non-Primitives

- **Primitive:** Basic data types that store a **single value**.
    
    - `number`
        
    - `string`
        
    - `boolean`
        
    - `undefined`
        
    - `null`
        
    - `bigint`
        
    - `symbol`
        
- **Non-Primitive (Reference):** More complex data types that can store **multiple values / structures**.
    
    - `Object`
        
    - `Array`
        
    - `Function`
        

**Key difference:**  
Primitive → value itself is stored/copied.  
Non-primitive → reference to the object is stored/copied.

**Example:**

```js
let a = 10;
let b = a;     // copies value

let x = [1, 2];
let y = x;     // copies reference
```

So changing `y` can also affect `x`, because both refer to the **same array**.