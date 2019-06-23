---
title: "Swift is Kotlin"
description: "Swift is to the Objective-C runtime what Kotlin is to the JVM. They are suprisingly similar."
date: "2014-06-05"
categories: 
    - "Swift"
    - "Kotlin"
    - "Programming"
---

Around the internet people are saying Apple's [Swift][swift] looks like C#, JavaScript etc. This of course is just in the most superficial manner. E.g. both JavaScript and [Swift][swift] has the `var` keyword. But it means completely different things since [Swift][swift] is statically typed and JavaScript is not.

Anyway being generally curious about programming languages I could see similarities in [Swift][swift] with [Rust][rust], [Go][go], Scala and Ruby. However all these languages differ from Swift in quite a number of ways.

But if you actually really want find a language that is close to [Swift][swift], I don't know of anything closer than [Kotlin][kotlin] developed by JetBrains. They both seem to have many of the same goals:

* Both want to integrate very well with their respective platform. Kotlin with Java and Swift with Objective-C. Unlike Scala which seems more focused on the idea of briding OO and functional programming.
* IDE support seems important. JetBrains want a language that works well with their IDE. Apple wants something that works well with xCode. Great tools is created in lockstep like e.g. Swift Playground. Rust and Scala e.g. are not heavily tool focused.
* Simplicity and familiarity are important goals. There are more sofisticated languages out there than [Kotlin][kotlin] and [Swift][swift] such as Scala, Rust and Haskell. But learning these languages fully requires a lot more effort.
* Functional programming is used as a way to enhance the language were it makes sense but it isn't any central goal like in e.g. Clojure and Scala.

## Feature and Syntax similarities
 
Both place type information after the name of the identifier unlike C/C++, C# and Java. This is also similar to Go, Rust, Pascal etc. Kotlin:

    fun sum(a : Int, b : Int) : Int { 
      return a + b 
    }

Swift:

    func sum(a : Int, b : Int) -> Int { 
      return a + b 
    }


Syntax for ranges and for loops is similar:

    for (index in 1...5) {
        println("$(index) times 5 is $(index * 5)")
    }

Swift:

    for index in 1...5 {
        println("\(index) times 5 is \(index * 5)")
    }

### Pattern matching

Both do pattern matching in a smiliar manner, although different keywords is used. In Kotlin you write:

    when (count) {
    in 0 ->
        naturalCount = "no"
    in 1...3 ->
        naturalCount = "a few"
    in 4...9 ->
        naturalCount = "several"
    in 10...99 ->
        naturalCount = "tens of"
    in 100...999 ->
        naturalCount = "hundreds of"
    in 1000...999_999 ->
        naturalCount = "thousands of"
    else ->
        naturalCount = "millions and millions of"
    }
    
Note this is slightly wrong since I don't believe Kotlin support `...`. Just `..`. But it isn't that important with respect to showing the similarities and different between the pattern matching syntax.
Which in Swift turns into:

    when (count) {
    c 0:
        naturalCount = "no"
    case 1...3:
        naturalCount = "a few"
    case 4...9:
        naturalCount = "several"
    case 10...99:
        naturalCount = "tens of"
    case 100...999:
        naturalCount = "hundreds of"
    case 1000...999_999:
        naturalCount = "thousands of"
    default:
        naturalCount = "millions and millions of"
    }

### Type casting

Even type casting looks and behaves in the same way, and the both have Option types to avoid null pointer exceptions:

    val x : String = y as String   // unsafe case. May throw exception.
    val x : String? = y as? String // safe cast. Nil if not working
    
Swift:

    let x : String = y as String
    let x : String? = y as? String
    
### Enum types

Enumerations are quite different. They have some overlapping functionality. Both allow you to do:

    enum class Direction { 
      NORTH 
      SOUTH 
      WEST 
      EAST 
    }

Swift:

    enum  Direction { 
      case NORTH, SOUTH, WEST, EAST 
    }

But for more advance case Kotlin and Swift enums look quite different. I don't understand either of them well enough to reason whether they actually represent the same thing deep down or if they are fundamentally different although I lean towards the latter. In Kotlin you can write:

    enum class Color(val rgb : Int) { 
      RED : Color(0xFF0000) 
      GREEN : Color(0x00FF00) 
      BLUE : Color(0x0000FF) 
    }


In Swift my impression is that enums are not really objects in the normal sense. I believe technically they are called a sum type, which is also called a variant in some languages. You can reproduce the Kotlin example in a bit clunky way:

    struct Color {
        var red : Int
        var green : Int
        var blue : Int
    }

    enum ColorEnum {
        case RED, GREEN, BLUE
        var value : Color {
            switch self {
            case .RED:
                 return Color(red: 1, green: 0, blue: 0)
            case .BLUE:
                return Color(red: 0, green: 0, blue: 1)
            case .GREEN:
                return Color(red: 0, green: 1, blue: 0)
            }
        }
    }

But I believe the more appropriate Swift way of doing it is:

    enum  Color { 
      RED  (0xFF, 0x00, 0x00) 
      GREEN(0x00, 0xFF, 0x00) 
      BLUE (0x00, 0x00, 0xFF) 
    }

Which works better for pattern matching in `switch-case` statements. The advantage of the Swift approach is that it allows us to define variants:

    enum ServerResponse {
      case Result(String, String)
      case Error(String)
    }
    
So you can easily define results from functions which are either a value or error object but which the user is forced to extract with pattern matching using `switch`:

    switch fetchSunriseAndSunsetTimes() {
    case let .Result(sunrise, sunset):
        let serverResponse = "Sunrise is at \(sunrise) and sunset is at \(sunset)."
    case let .Error(error):
        let serverResponse = "Failure...  \(error)"
    }

# Conclusion

I am not going to go through every feature, but it is clear when you look at how the languages work that there are a lot of similarities. In the regard that they are different I believe that is born out of the limitations set by each respective platform JVM or Objective-C runtime. Swift likely chose a more sofisticated enum type system with sum types to deal with the fact that exceptions can not easily be provided in a non GC language with Cocoa not being exception safe. I know there isn't an obvious connection here but I believe enum types will be used for building an error handling system in the future for Apple API's. Kotlin is more traditional in supporting exceptions which already works quite well on the JVM.

Swift also needed away to deal with the peculiar naming of method in iOS where each argument has a name. Cocoa programming is also frequently uses plain C structs for things such as points, rectangles etc. Thus this is explicitly supported in Swift as value types. Java has had no support for this traditionally and thus it is probably harder to get value types to interact with Java API's. Hence Kotlin has omitted value types.

One advantage I can see from an iOS developers point of view is that I ocassionally work on Android development and if Kotlin is well supported on Android then it will make switching between iOS and Android development a smaller overhead languagewise. iOS and Android APIs are of course quite different but it might be possible to write model objects which are quite similar. Perhaps one can create tools which translate between Swift and Kotlin code as long as one stay with a common subset of functionality. The main problem with writing library code which could be shared is that a Swift API will not use exceptions for error handling but likely enum types. Kotlin APIs will likely rely on exceptions. So it is not possible to write shared functionality without sacrificing benefits on one of the platforms.

[kotlin]: http://kotlin.jetbrains.org
[swift]: https://developer.apple.com/swift/
[rust]: http://www.rust-lang.org
[go]: http://golang.org


