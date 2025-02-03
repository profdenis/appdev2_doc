# Functions

## Functions as First-Class Objects

In modern programming languages, functions are treated as first-class citizens, meaning they can be used like any other
value or object[1][14]. This allows functions to be:

- Assigned to variables
- Passed as arguments to other functions
- Returned from other functions
- Stored in data structures

## Function Types and Declarations

To work with functions as values, we need to specify their type. A function type describes:

- The parameter types the function accepts
- The return type of the function

For example, in TypeScript we can declare function types like this[3]:

```typescript
let add: (a: number, b: number) => number;
```

In Kotlin, function types use a similar syntax[7]:

```kotlin
val multiply: (Int, Int) -> Int
```

## Passing and Returning Functions

Functions can be passed as arguments to other functions (called higher-order functions). Here's an example[4]:

```kotlin
fun callMe(greeting: () -> Unit)
{
    greeting()
}

fun sayHello()
{
    println("Hello!")
}

callMe(::sayHello) // Passes sayHello function as argument
```

Functions can also be returned from other functions[5]:

```kotlin
fun makeCounter(start: Int): () -> Int {
    var count = start
    return fun(): Int {
        count++
        return count
    }
}
```

## Lambda Functions

Lambda functions are anonymous functions that can be created without using the formal function declaration syntax[6].
They are especially useful for short, one-off functions.

Basic lambda syntax in Kotlin[7]:

```kotlin
val sum = { x: Int, y: Int -> x + y }
```

Lambda functions can be assigned to variables[61]:

```kotlin
val square: (Int) -> Int = { x -> x * x }
```

Multiple parameters[62]:

```kotlin
val fullName: (String, String) -> String = { firstName, lastName ->
    "$firstName $lastName"
}
```

## Common Lambda Use Cases

**As Function Arguments**[7]:

```kotlin
val numbers = listOf(1, 2, 3, 4)
val doubled = numbers.map { it * 2 }
```

**Returning Lambdas**[7]:

```kotlin
fun makeOperation(op: String): (Int, Int) -> Int {
    return when (op) {
        "add" -> { a, b -> a + b }
        "multiply" -> { a, b -> a * b }
        else -> { a, b -> 0 }
    }
}
```

**Trailing Lambda Syntax**[12]:
If a function's last parameter is a lambda, you can place it outside the parentheses:

```kotlin
fun repeat(times: Int, action: () -> Unit) {
    for (i in 1..times) action()
}

// Called with trailing lambda
repeat(3) {
    println("Hello")
}
```

??? note "Citations"
      - [1] https://blog.bitsrc.io/functional-programming-part-1-first-class-functions-791103984dfb?gi=1006a4787aa1
      - [2] https://en.wikipedia.org/wiki/Function_object
      - [3] https://www.typescripttutorial.net/typescript-tutorial/typescript-function-types/
      - [4] https://education.launchcode.org/intro-to-professional-web-dev/chapters/more-on-functions/passing-functions.html
      - [5] https://stackoverflow.com/questions/7629891/functions-that-return-a-function-what-is-the-difference-between-return-func
      - [6] https://www.freecodecamp.org/news/python-lambda-function-explained/
      - [7] https://www.dhiwise.com/post/kotlin-lambda-expressions-everything-you-need-to-know
      - [8] https://developer.mozilla.org/en-US/docs/Glossary/First-class_Function
      - [9] https://www.programiz.com/kotlin-programming/lambdas
      - [10] https://www.baeldung.com/kotlin/lambda-expressions
      - [11] https://developer.android.com/codelabs/basic-android-kotlin-compose-function-types-and-lambda
      - [12] https://kotlinlang.org/docs/lambdas.html
      - [13] https://fr.wikipedia.org/wiki/Objet_de_premi%C3%A8re_classe
      - [14] https://en.wikipedia.org/wiki/First-class_function
      - [15] https://www.reddit.com/r/learnpython/comments/1aggl8q/why_should_i_use_objects_instead_of_variables_and/
      - [16] https://www.studysmarter.co.uk/explanations/computer-science/functional-programming/first-class-functions/
      - [17] https://stackoverflow.com/questions/245192/what-are-first-class-objects
      - [18] https://developer.ibm.com/tutorials/oo-v-functional-programming/
      - [19] https://www.reddit.com/r/learnprogramming/comments/cudods/first_class_functions_definition_meaning/
      - [20] https://www.pluralsight.com/resources/blog/ai-and-data/javascript-functions-as-first-class-objects
      - [21] https://www.techtarget.com/searchapparchitecture/definition/object-oriented-programming-OOP
      - [22] https://www.studyplan.dev/pro-cpp/first-class-functions
      - [23] https://www.cs.fsu.edu/~myers/c++/notes/classes.html
      - [24] https://mostly-adequate.gitbook.io/mostly-adequate-guide/ch02
      - [25] https://www.php.net/manual/en/language.types.declarations.php
      - [26] https://www.typescriptlang.org/docs/handbook/2/functions.html
      - [27] https://en.cppreference.com/w/cpp/language/function
      - [28] https://dmitripavlutin.com/typescript-function-type/
      - [29] https://www.codecademy.com/forum_questions/549f635fd3292f1bec0111af
      - [30] https://go101.org/article/function-declarations-and-calls.html
      - [31] https://learn.microsoft.com/en-us/office/vba/language/reference/user-interface-help/function-statement
      - [32] https://stackoverflow.com/questions/57450544/declaring-functions-as-variables-in-c
      - [33] https://www.reddit.com/r/ProgrammingLanguages/comments/u9mm3x/lets_talk_function_declarations/
      - [34] https://softwareengineering.stackexchange.com/questions/300029/should-i-declare-variables-at-the-top-of-the-function-for-reasons-other-than-the
      - [35] https://forum.arduino.cc/t/how-do-you-initiate-functions-in-the-variable-declaration-area/500552
      - [36] https://codeofcode.org/lessons/function-arguments-and-return-values/
      - [37] https://stackoverflow.com/questions/13286233/pass-a-javascript-function-as-parameter
      - [38] https://support.freedomscientific.com/Content/Documents/Other/ScriptManual/12-4_FunctionsThatReturnValues.htm
      - [39] https://www.tutorjoes.in/c_programming_tutorial/return_with_arg_function_in_c
      - [40] https://www.w3schools.com/python/gloss_python_function_arguments.asp
      - [41] https://www.reddit.com/r/learnjavascript/comments/ttsfa8/what_does_it_mean_to_return_function_inside/
      - [42] https://runestone.academy/ns/books/published/thinkcspy/Functions/Functionsthatreturnvalues.html
      - [43] https://www.ibm.com/docs/en/zos/2.4.0?topic=performance-passing-function-arguments
      - [44] https://www.ibm.com/docs/en/i/7.4?topic=statement-examples-return-statements
      - [45] https://stackoverflow.com/questions/42461229/when-to-return-value-from-function-and-when-to-use-out-parameter
      - [46] https://softwareengineering.stackexchange.com/questions/253868/passing-functions-into-other-functions-as-parameters-bad-practice
      - [47] https://www.cs.fsu.edu/~cop3014p/lectures/ch7/index.html
      - [48] https://docs.aws.amazon.com/lambda/latest/dg/foundation-progmodel.html
      - [49] https://www.reddit.com/r/learnprogramming/comments/123bgpo/when_should_i_use_lambda_functions/
      - [50] https://www.datacamp.com/tutorial/python-lambda-functions
      - [51] https://learn.microsoft.com/en-us/cpp/cpp/lambda-expressions-in-cpp?view=msvc-170&viewFallbackFrom=vs-2019
      - [52] https://docs.aws.amazon.com/lambda/latest/dg/welcome.html
      - [53] https://en.wikipedia.org/wiki/Anonymous_function
      - [54] https://docs.aws.amazon.com/lambda/latest/dg/getting-started.html
      - [55] https://stackoverflow.com/questions/47110879/kotlin-when-and-how-should-one-use-lambda-expressions
      - [56] https://stackoverflow.com/questions/77859479/lambda-value-assignment-with-and-without-braces-into-composable-function
      - [57] https://kotlinlang.org/docs/functions.html
      - [58] https://www.linen.dev/bot
      - [59] https://www.reddit.com/r/Kotlin/comments/z2al5d/kotlin_theory_behind_jetpack_compose_what_are/
      - [60] https://www.reddit.com/r/androiddev/comments/vp1td5/best_way_to_pass_lambdas_down_composables_in/
      - [61] https://blog.stackademic.com/functions-lambdas-in-kotlin-android-bringing-your-code-to-life-with-everyday-magic-c73143e32ef3
      - [62] https://www.javatpoint.com/kotlin-lambdas
      - [63] https://dev.to/nozibul_islam_113b1d5334f/functions-as-first-class-citizens-in-javascript-4fji
      - [64] https://dillionmegida.com/p/functions-are-also-objects/
      - [65] https://en.wikipedia.org/wiki/First-class_data_type
      - [66] https://wiingy.com/learn/python/first-class-functions-in-python/
      - [67] https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions
      - [68] https://www.typescriptlang.org/docs/handbook/variable-declarations.html
      - [69] https://www.ibm.com/docs/en/i/7.4?topic=definitions-examples-function-declarations
      - [70] https://discourse.julialang.org/t/function-type-declaration/26203
      - [71] https://www.w3schools.com/c/c_functions_decl.php
      - [72] https://www.typescriptlang.org/docs/handbook/functions.html
      - [73] https://docs.swift.org/swift-book/documentation/the-swift-programming-language/functions/
      - [74] https://www.dhiwise.com/post/in-depth-look-at-typescript-function-types-best-practices
      - [75] https://treyhunner.com/2020/01/passing-functions-as-arguments/
      - [76] https://www.w3schools.com/cpp/cpp_function_return.asp
      - [77] https://www.reddit.com/r/C_Programming/comments/1b6klq5/when_to_return_values_in_the_arguments_of_a/
      - [78] https://www.reddit.com/r/learnjavascript/comments/m8vlsb/can_someone_explain_how_to_pass_a_function_as_an/
      - [79] https://www.labs.cs.uregina.ca/110/OER2022/labs/lab9/valuereturning.php
      - [80] https://www.w3schools.com/python/python_lambda.asp
      - [81] https://discuss.python.org/t/what-is-the-purpose-of-lambda-expressions/12415
      - [82] https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/operators/lambda-expressions
      - [83] https://realpython.com/python-lambda/
      - [84] https://www.dataquest.io/blog/tutorial-lambda-functions-in-python/
      - [85] https://www.youtube.com/watch?v=HQNiSfb795A
      - [86] https://stackoverflow.com/questions/16501/what-is-a-lambda-function
      - [87] https://developer.android.com/develop/ui/compose/kotlin
      - [88] https://www.youtube.com/watch?v=unCjOegBSMI

## Collection Operations with Lambda Functions

**Map Transformations**

The `map` function transforms each element in a collection using a lambda expression:

```kotlin
val numbers = listOf(1, 2, 3, 4, 5)
val squared = numbers.map { it * it }
println(squared) // [1, 4, 9, 16, 25]

// With custom objects
data class Person(val name: String, val age: Int)

val people = listOf(Person("Alice", 25), Person("Bob", 30))
val names = people.map { it.name }
println(names) // [Alice, Bob]
```

**Filter Operations**

Filter creates subsets of collections based on predicates:

```kotlin
val numbers = listOf(1, 2, 3, 4, 5, 6)
val evenNumbers = numbers.filter { it % 2 == 0 }
println(evenNumbers) // [2, 4, 6]

// Filtering objects
val adults = people.filter { it.age >= 18 }
```

## Advanced Collection Operations

**Chaining Operations**

```kotlin
val result = numbers
    .filter { it > 2 }
    .map { it * 2 }
    .filter { it < 10 }
```

**Working with Maps**

```kotlin
val ages = mapOf("Alice" to 25, "Bob" to 30, "Carol" to 35)
val adults = ages.filterValues { it > 30 } //[4]
val namesWithA = ages.filterKeys { it.startsWith("A") } //[4]
```

**Special Filters**

```kotlin
// Remove null values
val mixedList = listOf(1, null, 2, null, 3)
val nonNullValues = mixedList.filterNotNull() //[3]

// Filter with index
val indexed = numbers.filterIndexed { index, value ->
    index % 2 == 0 && value > 2
} //[3]
```

**Testing Predicates**

```kotlin
val numbers = listOf(1, 2, 3, 4, 5)
val hasEven = numbers.any { it % 2 == 0 } //[3]
val allPositive = numbers.all { it > 0 } //[3]
val noNegatives = numbers.none { it < 0 } //[3]
```

**Partitioning**

```kotlin
val (evens, odds) = numbers.partition { it % 2 == 0 } //[3]
```

These operations are highly optimized in Kotlin - lambda functions passed to these collection operations are inlined,
meaning there's no extra function call overhead at runtime[7].

??? note "Citations"
      - [1] https://developer.android.com/codelabs/basic-android-kotlin-compose-higher-order-functions
      - [2] https://www.baeldung.com/kotlin/filter-collection
      - [3] https://kotlinlang.org/docs/collection-filtering.html
      - [4] https://kotlinlang.org/docs/map-operations.html
      - [5] https://kotlinlang.org/docs/collection-transformations.html
      - [6] https://www.dhiwise.com/post/kotlin-list-filter-a-key-to-effective-data-filtering
      - [7] https://www.dhiwise.com/post/kotlin-filter-simplifying-the-search-for-collection-elements
      - [8] https://stackoverflow.com/questions/73635893/log-statements-and-collection-functions-map-filter
      - [9] https://kotlinlang.org/docs/lambdas.html
      - [10] https://appcircle.io/blog/transforming-kotlin-collections-functions-with-examples
      - [11] https://stackoverflow.com/questions/68429908/how-to-use-a-predefined-lambda-in-kotlin

## Example with Jetpack Compose

```kotlin
import android.util.Log

@Composable
fun ActionButton(
    text: String,
    onButtonClick: () -> Unit
) {
    Button(
        onClick = onButtonClick
    ) {
        Text(text)
    }
}
```

And here's how to use it with proper logging:

```kotlin
ActionButton(
    text = "Click Me",
    onButtonClick = {
        Log.d("ActionButton", "Button clicked!")
    }
)
```

The Log class provides different logging levels:

- `Log.d()` for debug messages
- `Log.i()` for informational messages
- `Log.e()` for error messages
- `Log.w()` for warning messages
- `Log.v()` for verbose messages

These messages will appear in Android Studio's Logcat window, where you can:

- Filter by tag ("ActionButton" in this example)
- Filter by log level
- Search through logs
- See the exact timestamp of each event
