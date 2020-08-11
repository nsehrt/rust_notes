# Rust



### Hello World

```rust
fn main() {
    println!("Hello, world!");
}
```



### Cargo basics

```bash
cargo new project_name
cargo build
cargo check
cargo run
```



### Variables

```rust
let x = 5; //immutable
let mut y = 5; // mutable
y = 6;
```

Variables are immutable by default.

Constant variables are always immutable.

```rust
const MAX: i32 = 100_000;
```

Shadowing can be used to *overwrite* immutable variables.

```rust
fn main(){
    let x = 5;
    let x = x + 1;
}
```



### Data types

**Integer types:**

- i8, u8
- i16, u16
- i32, u32
- i64, u64
- i128, u128

**Literals:**

|           |             |
| :-------- | ----------- |
| Decimal   | 98_222      |
| Hex       | 0xff        |
| Octal     | 0o77        |
| Binary    | 0b1111_0000 |
| Byte (u8) | b'A'        |

**Floating point types:**

- f32
- f64

**Boolean:**

```rust
let t: bool = true;
let f = false;
```

**Character:**

```rust
let c: char = 'c';
let z = 'z';
```

**Tuple:**

Tuples can be used to group values of different (or equal) types together. Tuples have fixed length.

```rust
let tup: (i32, f64, u8) = (500, 6.3, 1);
```

Tuples can be deconstructed.

```rust
let (x, y, z) = tup;
```

Single data points of the tuple can be accessed with the . notation.

```rust
let t1 = tup.0;
let t2 = tup.1;
```

**Array:**

Arrays hold data of the same type and have fixed length and are stored on the stack.

```rust
let array = [1,2,3,4,5,6];
let a: [i32; 5] = [1,2,3,4,5];
let a = [3; 5]; // a will be 5 times the value 3 [3,3,3,3,3]
```

Data can be accessed with [] indexing.

```rust
let first_element = a[0];
let second_element = a[1];
```



### Functions

Function definition and calling:

```rust
fn a_function() {
    println!("Hello there!");
}

fn main() {
    a_function();
}
```

Function parameters/arguments:

```rust
fn a_function2(x: i32){
    println!("Argument is: {}", x);
}

fn a_function3(x: i32, y: i32){
    
}
```

The types of the parameters must be specified.

When a function returns a value, you need to specify the type of the returned value.

```rust
fn five() -> i32{
    5 // last expression is returned
}

fn plus_one(x: i32) -> i32{
    return x + 1; // explicit return statement
}

fn fib(x: i32) -> i32 {
    if x == 0 { return 0; }
    if x == 1 { return 1; }

    fib(x - 1) + fib(x - 2)
}
```



### Comments

Double slashes are used for comments. They comment out one line.

```rust
// comment
let x: f64 = 0.5; // comment
```

Multiline block comments do not exist, use *//* on every line instead.



### Control Flow

Use *if* expression for branching code. Parentheses around the condition are not necessary.

```rust
fn main(){
    let x = 5;
    
    if x >= 5 {
        println!("Condition true!");
    }else{
        println!("Condition false!");
    }
}
```

Rust does not implicitly convert non-bool values to bool.

```rust
let x = 3;

if x { // error
    
}

if x != 0 { // better
    
}
```

Use *else if* for multiple conditions.

```rust
let n = 4;

if n % 4 == 0{
    println!("Divisible by 4.");
}else if n % 3 == 0{
    println!("Divisible by 3.");
}else{
    
}
```

*if* expressions can be used in let statements.

```rust
let condition = true;
let number = if condition { 1 } else { 0 }; // both outcomes need to be of the same type
```

A ternary operator does not exist.



*loop* will repeat a block until it is told to stop, for example by using *break*.

```rust
loop{
    //do something
}
```

*break* can return values from a loop.

```rust
let mut counter = 0;

let result = loop{
    counter += 1;
    
    if counter > 10 {
        break counter + 2;
    }
};
```

Use *while* for a conditional loop. As long as the condition is true the *while* loop continues to run.

```rust
let mut number = 0;

while number < 100 {
    println!("{}", number);
    number += 1;
}
```

Use *for* for an iterative loop.

```rust
let a = [10,20,30,40,50];

for elem in a.iter() {
    println!("Value: {}", elem);
}
```

A *range* can also be used.

```rust
for i in 0..100 {
    println!("{}", i);
}

for i in (0..100).rev() { // count down
    println!("{}", i);
}
```



###  Ownership

- Each value has a variable that is called its *owner*
- There can only be one *owner* at a time
- When the owner goes out of scope, the value will be dropped

Rust is a block scoped language.

Create a string from a literal:

```rust
let mut s = String::from("Hello");
s.push_str(", world!"); //append literal to string
println!("{}", s);
```

Deallocation of heap memory happens when *s* goes out of scope; the special function *drop* is called automatically.

Copying heap allocated data results in a move operation instead of a copy.

```rust
let x = 5;
let y = x; // works as expected, data is copied
```

Basic data types are deep copied hence no ownership issues.

```rust
let s1 = String::from("hello"); // String s1 is heap allocated
let s2 = s1;					// s2 becomes the new owner, s1 is invalid

println!("{}, world!", s1);		// error because s1 is invalid
println!("{}, world!", s2);		// works
```

A deep copy operation can be forced by using the *clone()* method.

```rust
let s1 = String::from("hello");
let s2 = s1.clone();		// deep copy

println!("{} {}", s1, s2); // both strings are valid
```

Passing a value to a function works the same way, value will be moved or copied.

```rust
fn main(){
    let s = String::from("hello");
    takes_ownership(s); // value of s moves into function and s is no longer owner
    //println!("{}", s); <- error, s invalid because value moved
    
    let x = 5;
    makes_copy(x); // x still usable after this because x (i32) is copied
}

fn takes_ownership(s : String){
    println!("{}", s);
}

fn makes_copy(x : i32){
    println!("{}", x);
}
```

The return value of a function can also give ownership.

```rust
fn main(){
    let s1 = gives_ownership(); //s1 is owner of return value of the function
    let s2 = String::from("Hello"); // new string allocated
    let s3 = takes_and_gives(s2); //function takes ownership of the value of s2 and then gives it back to s3
}

fn gives_ownership()-> String{
    let some_string = String::from("Hello");
    some_string
}

fn takes_and_gives(s : String) -> String{
    s
}
```



### References and Borrowing

If you do not want to give a function ownership of a value, you can use a reference. References as function parameters are call *borrowing*.

```rust
fn main(){
    let s1 = String::from("Hello");
    let len = calc_length(&s1); // & = reference -> borrow s1 to the function
}

fn calc_length(s : &String) -> usize{ // s = reference to a String
    s.len()
} // s goes out of scope but it is just a reference so the value is not dropped
```

Dereferencing is achieved with the * operator.

References are immutable by default. *mut* can be used to make a reference mutable.

```rust
fn main(){
    let mut s = String::from("hello");
    change(&s);
}

fn change(s : &mut String){
    s.push_str(", world!");
}
```

You can have only one mutable reference to a value in a particular scope.

```rust
let mut s = String::from("hellO");
let r1 = &mut s;
let r2 = &mut s; //this will throw an error
```

This eliminates data races.

```rust
let mut s = String::from("Hello");
{
    let r1 = &mut s; 
}	// reference goes out of scope
let r2 = &mut s; // so this is legal
```

It is also not possible to have a mutable reference and an immutable reference simultaneously. Multiple immutable reference do work though.

```rust
let mut s = String::from("hello");

let r1 = &s; // Okay
let r2 = &s; // Okay
let r3 = &mut s; //Error, because r1 and r2 are used after this
println!("{}, {}, and {}", r1, r2, r3);
```

A reference's scope starts on introduction and ends on the last usage.

```rust
let mut s = String::from("hello");

let r1 = &s; // okay
let r2 = &s; // okay
println!("{} and {}", r1, r2);
// r1 and r2 are not used after this -> their scope ends

let r3 = &mut s; // okay
println!("{}", r3);
```

Dangling references throw an error at compile time.

```rust
fn main() {
    let reference_to_nothing = dangle();
}

fn dangle() -> &String {
    let s = String::from("hello"); //s is owner

    &s
} // s goes out of scope here so that the returned reference is invalid -> error

fn no_dangle() -> String {
    let s = String::from("hello");

    s //returning s unreferenced moves the value out of the function -> no error
}
```



### Slice Type

Slices refer to a part of a collection and do not have ownership of the data.

A string slice is a reference to a portion of a string. They can be created by indexing a reference to a string.

```rust
let s = String::from("Hello, world!");
let slice = &s[6..11];
let slice2 = &s[..2]; // 0 .. 2
let slice3 = &s[2..] // 2 .. len() of string
let slice4 = &s[..] // 0 .. len() of string
```

A function to return the first word in a string.

```rust
fn first_word(s: &String) -> &str {
    let bytes = s.as_bytes();
    
    for (i, &item) in bytes.iter().enumerate() {
        if item == b' '{
            return &s[0..i]
        }
    }
    
    &s[..] // return the whole string as slice
}
```

String literals are slices because they are a reference to the data in the binary.

```rust
let s = "Hello, World!"; // s is of type &str -> an immutable reference
```

The function parameter of *first_word()* can be changed to accept a string slice &str instead of a &String, so that we can pass both a String or a string slice.

```rust
fn first_word(s: &str) -> &str{
    
}

fn main(){
    let my_string = String::from("hello, world!");
    let word = first_word(&my_string[..]); // pass string slice of the whole string
    let my_string_literal = "hello world!";
    let word = first_word(&my_string_literal[..]);
    let word = first_word(my_string_literal); //this syntax works too, because it is already a string slice
}
```

Slices can also be used to refer to a part of an array.

```rust
let arr = [4,5,6,7,8];
let slice = &arr[1..3]; // slice is of type &[i32]
```



### Structs

Definition

```rust
struct Person{
    first_name: String,
    last_name: String,
    age: u32,
    active: bool
}
```

Create an instance of the struct by specifying values for each of the fields. The order does not need to be the same.

```rust
let mut person = Person{
    first_name: String::from("Orlando"),
    last_name: String::from("the dog"),
    active: false,
    age: 14
};

person.active = true;
```

Entire instance must be mutable even if you only want to change one field.

A function that returns an instance.

```rust
fn build_person(first: String, last_name: String, act: bool, age: u32) -> Person {
    Person{
        first_name: first,
        last_name, // no repetition needed when variable name equals the field name
        active: act,
        age
    }
}
```

When creating a second instance that only differs in some spots, you can use a shortcut.

```rust
let user2 = User {
        email: String::from("another@example.com"),
        username: String::from("anotherusername567"),
        active: user1.active,
        sign_in_count: user1.sign_in_count,
    };

let user3 = User {
        email: String::from("another@example.com"),
        username: String::from("anotherusername567"),
        ..user1 // use the rest of the fields from user1
    };
```

Tuple structs are structs that do not have named fields, only types.

```rust
struct Color(i32,i32,i32);

let black = Color(0,0,0);
let f = black.0;
```

It is also possible to define structs without any data fields.

Struct example program:

```rust
struct Rectangle{
    width: u32,
    height: u32
}

fn main(){
    let rect = Rectangle{
        width: 30,
        height: 50
    };
    
    println!("The area is {}.", area(&rect));
}

fn area(r: &Rectangle) -> u32{
    r.width * r.height
}
```

To print a struct like this

```rust
println!("Rectangle: {:?}", rect);
```

the struct needs to implement the *Debug* trait.

```rust
#[derive(Debug)]
struct Rectangle{
    ...
}
```

Use {:#?} instead of {:?} for printing of bigger structs because the output gets expanded over several lines.



### Methods

Methods are functions that are defined within the context of a struct. Their first parameter is always  *self*, which represents the instance of the struct the method is being called on.

```rust
impl Rectangle{ //implementation block for the struct Rectangle
    fn area(&self) -> u32{ 	// take reference to the struct as parameter -> no ownership taken
        self.width * self.height
    }
    
    fn can_hold(&self, other: &Rectangle) -> bool {
        self.width > other.width && self.height > other.height
    }
}

fn main(){
    let rect1 = Rectangle {
        width: 30,
        height: 50,
    };
    let rect2 = Rectangle {
        width: 10,
        height: 40,
    };
    
    let area = rect1.area();
    let hold = rect1.can_hold(&rect2);
}
```

Associated functions do not take *self* as a parameter and are often used as a constructor.

```rust
impl Rectangle{
    fn square(size: u32) -> Rectangle {
        Rectangle{
            width: size,
            height: size,
        }
    }
}

fn main(){
    let sq = Rectangle::square(5);
}
```

A struct can have multiple *impl* blocks.



### Enumerations

Definition

```rust
enum IpAddressKind{
    v4,
    v6,
}

struct IpAddr{
    kind: IpAddressKind,
    address: String,
}

fn main(){
    let four = IpAddressKind::v4;
    
    let local = IpAddr{
        kind; IpAddressKind::v4,
        address: String::from("127.0.0.1"),
    }
}
```

It is also possible to store data in an enumeration.

```rust
enum IPAddr {
    v4(String),
    v6(String),
}

let home = IPAddr::v4(String::from("127.0.0.1"));
```

Each variant can have different data types.

```rust
enum IPAddr{
    v4(u8,u8,u8,u8),
    v6(String),
}

enum Message {
    Quit,
    Move {x: i32, y: i32}, // using an anonymous struct
    Write (String),
    ChangeColor (i32, i32, i32),
}
```

We are also able to define methods on *enums* using an *impl* block.

```rust
impl Message {
    fn call(&self) {
        
    }
}

let m = Message::Write(String::from("abc"));
m.call();
```

The standard library provides the *Option* enum that either holds a value or it does not.

```rust
enum Option<T> {
    Some(T),
    None,
}

let number = Some(5);
let no_number: Option<i32> = None;

let t: bool = number.is_some() // number.is_none();
```



### match Operator

The match operator is kind of similar to the switch operator.

```rust
enum Coin {
    Penny,
    Nickel,
    Dime,
    Quarter,
}

fn value(c: Coin) -> i32 {
    match coin {
        Coin::Penny => 1,
        Coin::Nickel => 5,
        Coin::Dime => 10,
        Coin::Quarter => 25,
    }
}
```

The structure is:

- *match* key word
- expression
- match arms consisting of a pattern and some code separated by =>
- the code part must be in brackets {} if it is more than one line
- each arm is separated by a comma

```rust
fn value_in_cents(coin: Coin) -> u8 {
    match coin {
        Coin::Penny => {
            println!("Lucky penny!");
            1
        }
        Coin::Nickel => 5,
        Coin::Dime => 10,
        Coin::Quarter => 25,
    }
}
```

match arms can extract values from enum variants.

```rust
#[derive(Debug)] // so we can inspect the state in a minute
enum UsState {
    Alabama,
    Alaska,
    // --snip--
}

enum Coin {
    Penny,
    Nickel,
    Dime,
    Quarter(UsState),
}

fn value_in_cents(coin: Coin) -> u8 {
    match coin {
        Coin::Penny => 1,
        Coin::Nickel => 5,
        Coin::Dime => 10,
        Coin::Quarter(state) => {
            println!("State quarter from {:?}!", state);
            25
        }
    }
}

value_in_cents(Coin::Quarter(UsState::Alaska));
```

The match operator can be used to handle Option<T> values.

```rust
fn plus_one(x: Option<i32>) -> Option<i32> {
    match x {
        Some(i) => Some(i + 1),
        None => None,
    }
}

let six plus_one(Some(5));
let n = plus_one(None);
```

match needs to cover all possible outcomes. You can use _ as the default case, as it always matches.

```rust
    let some_u8_value = 0u8;
    match some_u8_value {
        1 => println!("one"),
        3 => println!("three"),
        5 => println!("five"),
        7 => println!("seven"),
        _ => (),
    }
```

() is just the unit value, so nothing will happen.



### if let

if let can be used as an alternative to the match operator, if you only care about one match arm.

```rust
let some_value = Some(0u8);

match some_value {
    Some(3) => println!("three!"),
    _ => (),
}

// doing the same thing with if let

if let Some(3) = some_value{
    println!("three!");
}
```

if let takes a pattern and an expression separated by an equal sign. It is also possible to have an else arm.

```rust
if let Coin::Quarter(state) = coin {
        println!("State quarter from {:?}!", state);
    } else {
        count += 1;
    }
```



### Modules

the *use* keyword brings a path into scope.

the *pub* keyword makes items public. All items are private by default.

Modules are used to organize code in other files. The *mod* keyword defines a mod with a name.

```rust
mod module{
    
}
```

With *as* can paths be brought into scope under a different name.

```rust
use std::collection::HashMap; // brings HashMap into scope
use std::io::Result as IoResult;
use std::{cmp::Ordering, io};
use std::io::{self, Write}; // std::io and std::io::Write
use std::collections::* // all public items under this path
```

*mod* is also used to refer to code in a different file.

```rust
mod other_file; // loads other_file.rs
```



### Vectors

Vectors are like in C++ dynamic arrays, which can hold values of the same data type.

Creating a new vector:

```rust
let vec: Vec<i32> = Vec::new(); // explicit data type because the vector is empty
let vec = vec![1,2,3]; // shorter creation with macro
```

A new element can be added to the vector with the *push* method:

```rust
let mut v = Vec::new();
v.push(3);
```

The data can be accessed with indexing syntax or the *get* method:

```rust
let third_element = &v[2];
let second_element = v.get(1); // get returns an Option<T>

match v.get(5) {
    Some(t) => println!("{}", t),
    None => (),
}
```

A for loop can be used to conveniently iterate over the values in a vector.

```rust
let v = vec![1,2,3,4,5];

for i in &v { // iterate through reference because you do not want ownership
    println!("{}", i);
}

for i in &mut v {
    *i += 1; // dereference before applying action
}
```

Because *enums* can be used similar to a *union*, meaning it can have several data types, vectors also can have multiple data types when using a vector of such an enum.

```rust
enum Cell {
    Int(i32),
    Float(f64),
    Text(String),
}

let row = vec![Cell::Int(4), Cell::Float(5.5), Cell::Text(String::from("test"))];
```



### String

String operations:

```rust
let mut s = String::new();
let data = "initial"; // string literal, data is &str
let s = data.to_string(); 
let s = "initial".to_string();
let s = String::from("initial"); // all 3 result in the same
```

Strings are UTF-8 encoded, so a lot of different alphabets and things like emojis are possible to store in them.

A string can be appended to by using *push_str()* for string slices or *push()* for a single character.

```rust
let mut s1 = String::from("foo");
let s2 = "bar";
s1.push_str(s2);
s1.push('!');
```

Strings can be added together either by using the *+* operator or using the *format!* macro.

```rust
let s1 = String::from("hello");
let s2 = String::from(", world!");
let s3 = s1 + &s2; // s1 is moved and can no longer be used

let s = format!("{}-{}-{}", s1, s2, s3);
```

Strings can not be accessed via indexing, because different alphabets require different byte lengths for encoding in memory.

```rust
let hello = "Здравствуйте";
let answer = &hello[0]; // does not work!
```

String slices can be used for this operation.

```rust
let hello = "Здравствуйте";

let s = &hello[0..3]; // s will be Зд because each character takes two bytes in memory
```

Using wrong indices here will cause Rust to panic at runtime.

The *chars()* method can be used to iterate over the characters in a string.

The *bytes()* method can be used to iterate over the actual bytes in a string.

```rust
for c in "hello".chars() {
    println!("{}", c);
}

for b in "hello".bytes() {
    println!("{}", b);
}
```



### Hash maps

A hash map stores key value pairs. In contrast to a vector the index (key) could be of any type.

Creating a hash map:

```rust
use std::collection::HashMap;

let mut scores = HashMap::new();
scores.insert(String::from("blue"), 10);
scores.insert(String::from("yellow"), 50);
```

All keys must be of the same type and so must be all of the values. Owned values like string will have their ownership moved to the hash map, while types that implement the Copy trait will be copied.

Data can be accessed via the *get()* method. It returns an Option<&T>.

```rust
...
let team_name = String::from("blue");
let score = scores.get(&team_name);
```

Iterating over a hash map:

```rust
for (key, value) in &scores {
    println!("{}: {}", key, value);
}
```

Inserting the same key twice will overwrite the old one.

You can check for the existence of a key before inserting and thus possibly overwriting an old value. 

```rust
use std::collections::HashMap;

let mut scores = HashMap::new();
scores.insert(String::from("blue"),10);

scores.entry(String::from("yellow")).or_insert(50); // key does not exist -> okay
scores.entry(String::from("blue")).or_insert(50); // blue already exists -> nothing happens
```

Updating a hash map value based on the old value:

```rust
use std::collections::HashMap;

let text = "hello world hello hello";
let mut map = HashMap::new();

for word in text.split_whitespace() {
    let count = map.entry(word).or_insert(0); // return mutable reference or create new key value pair with value 0
    *count += 1;
}
```



### Error Handling

Rust distinguishes between two types of errors: recoverable and unrecoverable. Recoverable errors are handled with the Result<T,E> type and unrecoverable errors are covered calling the panic! macro.

Using *panic!* prints an error message, unwinds the stack and quits the program. Unwinding the stack can be avoided by editing the TOML file.

```rust
[profile.release]
panic = 'abort'
```

```rust
fn main() {
    panic!("crash the program!");
}
```

You can set the RUST_BACKTRACE environment variable to see where the panic originiated.

```rust
fn main() {
    let v = vec![1, 2, 3];

    v[99]; // causes panic
}

//
$Env:RUST_BACKTRACE=1
cargo run
```

Less serious errors create a Result<T, E>.

```rust
enum Result<T, E> {
    Ok(T),
    Err(E),
}
```

```rust
use std::fs::File;

fn main() {
    let f = File::open("log.txt"); // returns a Result<T, E>
    
    let f = match f {
        Ok(file) => file,
        Err(error) => panic!("Can not open file: {:?}", error),
    }
}
```

In order to have better control over the errors a match can be used on the error type.

```rust
let f = match f {
    Ok(file) => file,
    Err(error) => match error.kind() {
        ErrorKind::NotFound => match File::create("log.txt") {
            Ok(fc) => fc,
            Err(e) => panic!("Problem creating file: {:?}", e),
        },
        other_error => {
            panic!("Problem opening file: {:?}", other_error)
        }
    }
}
```
The *unwrap()* method of the Result<T, E> type unwraps the value or it calls the panic! macro.

```rust
let f = File::open("log.txt").unwrap(); // f = file handle or if error panic
```

*expect()* is a similar method which let's us specify an error message.

```rust
let f = File::open("log.txt").expect("Can not open file!");
```

It is often useful to propagate the error back to the caller of a method instead of handling it directly.

```rust
fn read_log() -> Result<String, io::Error> {
    let f = File::open("log.txt");
    
    let mut f = match f {
        Ok(file) => file,
        Err(e) => return Err(e),
    };
    
    let mut s = String::new();
    
    match f.read_to_string(&mut s) {
        Ok(_) => Ok(s),
        Err(e) => Err(e),
    }
}
```

Propagating errors can be shortened using the *?* operator.

```rust
fn read_log() -> Result<String, io::Error> {
    let mut f = File::open("log.txt")?;
    let mut s = String::new();
    f.read_to_string(&mut s)?;
    Ok(s)
}
```

If the value of the Result is an Ok the value inside the Ok will get returned from the expression. If the value is an Err, the Err will be returned from the whole function as if we had used the return keyword.

Even shorter:

```rust
fn read_log() -> Result<String, io::Error> {
    let mut s = String::new();
    File::open("log.txt")?.read_to_string(&mut s)?;
    Ok(s)
}

fn read_log2() -> Result<String, io::Error> {
    fs::read_to_string("log.txt")
}
```

The *?* operator can only be used in functions that return a Result<T, E>, Option<T>.



### Generics

