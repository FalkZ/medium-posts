# Rich Enums in Typescript?

> In this article we will explore some patterns in typescript, that have the same behavior as "rich enums"

## First of all: what are rich enums?

Let's take a look at a rust example. Rust supports rich enums right out of the box.

```rs
pub enum Option<T> {
    None,
    Some(T),
}
```

This definition of `Option` defines that option can be `None` or `Some`. Where `Some` contains a typed value of generic type `T`. That is the reason why they are called rich enums, because you can add additional information (in this case to `Some`).


```rs
let mut maybeNumber: Option<u32> = Some(5);
maybeNumber = None;
```
In this example we first set the variable `maybeNumber` to a value `Some(5)` and afterwards to `None`. Why is this better than directly assigning `5` and `null` you may ask. The short answer: the handling of the value is way easier later on in the code. The long answer, can be found here: [The Option Enum and Its Advantages Over Null Values](https://doc.rust-lang.org/book/ch06-01-defining-an-enum.html?highlight=option#the-option-enum-and-its-advantages-over-null-values)

## Basic Pattern

Let's explore the basic typescript pattern that enables you to do a similar thing in typescript.

First we need to create a basic enum in typescript, sadly there is no way to define rich enums directly like above. But we still want the benefits of the basic enum so let's define it.
```ts
enum OptionType {
    Some,
    None
}
```
Next up we will use the `OptionType` to define all the variations of the `Option` type. This means that `Option<T>` can ether be:
- `{ type: OptionType.Some, value: T }`
- or `{ type: OptionType.None }`
- 
```ts
type Option<T> = { type: OptionType.Some, value: T } | { type: OptionType.None }
```


### Example Usage

```ts
const handleMaybeNumber = (maybeNumber: Option<Number>)=> {
    switch (maybeNumber.type) {
        case OptionType.Some:
            // maybeNumber.value will always be a Number and also typed as one
            break;

        case OptionType.None:
            // maybeNumber.value will never exists
            break;
        }
}

handleMaybeNumber({ type: OptionType.Some, value: 5 })
handleMaybeNumber({ type: OptionType.None })

```

… not quite perfect but it's a start.

There will be a second article covering some additional "rich enum" patterns. 😉