# Making Your Website (in Someone Else's Website).

## Intro

Over last summer I wanted to make some bigger project in Java as a good {project for work showcase}. I had just finished my TOML parser, which was just a lovely experience of a straight week of coding. And back then, I had no idea how programming language compilers were made; the problem space was just mystical to me and I really don't like that in software. A black box is that thing you use to cook salmon. So I made my own interpreted language in Java called ulang. 

The syntax is somewhat similar to Odin/Go/Pascal: 
```
println(sum_f32([ 1, 2, 3, 4, 5 ]))
func sum_f32(array: ...) f32 {
    result: f32
    for i: num = 0; i < len(array); i = i + 1 {
        result = result + array[i]
    }
    return result
}
```
It does not have more fancy things, like: type inference, foreach or the `+=` operator... Or compound types (structs/classes)... I mean, I guess, you could use arrays for types, since they are heterogeneous here...
```
balls = [ [ 15, 200, 200, 1, -1, "red" ] ]

# struct Ball {
ball_radii = 0          # technically, a circle has an infinite amount of radiuses, it's just that they're all the same
ball_pos_x = 1
ball_pos_y = 2
ball_vel_x = 3
ball_vel_y = 4
ball_color = 5
# }
```

It does however have a foreign function interface, many of the normal math operators, type aliassing, arrays and $insert statements (that work similar to C)


# Type System Fun

At first, my type system was incredibly simple: there was only `number`, `bool`, `string`, `char` and a list (dynamic array) type. 
I also had structs and hash maps planned but just never got around to them. Unfortunately, all simplicity evaporated once I started interacting with other Java code (bindings/foreign functions). So now I have `varargs` as a type, `any` and you can also use `typedef` with `foreign` and I just have this beautiful block of:

```
# Java primitives
typedef u8  = foreign "byte" 
typedef i8  = foreign "char"
typedef i16 = foreign "short"
typedef i32 = foreign "int"
typedef i64 = foreign "long"
typedef f32 = foreign "float"
typedef f64 = foreign "double"
typedef u8_arr  = foreign "[B"
typedef i8_arr  = foreign "[C"
typedef b8_arr  = foreign "[Z"
typedef i16_arr = foreign "[S"
typedef i32_arr = foreign "[I"
typedef i64_arr = foreign "[J"
typedef f32_arr = foreign "[F"
typedef f64_arr = foreign "[D"

# typedef b8_arr_arr_arr = foreign "[[[F"
```

Java's reflection was actually a massive obstacle for my interpreter. Since, firstly, primitive types cannot be passed as a generic 
(that's why you use `List<Float>` and `Map<String, Integer>`). 
When you try to pass them wrapped in, for example, an `Object` the primitives simply decay to their respective wrapper types
and so in the later stages of the project, type validation sometimes just kind of failed...

Take this function:
```
public static Object try_cast_some(Object any, Class needed) {
    if(needed.equals(byte.class))    return ((Number) any).byteValue();
    if(needed.equals(short.class))   return ((Number) any).shortValue();
    if(needed.equals(int.class))     return ((Number) any).intValue();
    if(needed.equals(long.class))    return ((Number) any).longValue();
    if(needed.equals(float.class))   return ((Number) any).floatValue();
    if(needed.equals(double.class))  return ((Number) any).doubleValue();
    if(needed.equals(Byte.class))    return Byte.   valueOf(((Number) any).byteValue());
    if(needed.equals(Short.class))   return Short.  valueOf(((Number) any).shortValue());
    if(needed.equals(Integer.class)) return Integer.valueOf(((Number) any).intValue());
    ...
```
I wrote this function, because, I thought, it would convert `Integer` to `int`. It does not do that. 
But it does cast between different numbers, which is nice when all my literals are Doubles. 

And even once I realized that this decaying was happening, I still had to deal with primitives and wrappers being mixed in certain places so,
I have this beautiful little piece of code...
```
private boolean is_of_type(Class a, Class b, String b_typename) {
    return a.isAssignableFrom(b) || b.isAssignableFrom(a)
        || b_typename.equals(SyntaxDefinitions.TYPE_ANY)
        || (is_number(a) && is_number(b))
        || a.getSimpleName().equalsIgnoreCase(b.getSimpleName());
}
private boolean is_number(Class c) {
    return Number.class.isAssignableFrom(c) || c.isPrimitive();
}
```

as well as this (TypeValidation):
```
private boolean types_match(String a, String b) {
    if(a.equals(SyntaxDefinitions.TYPE_ANY) || b.equals(SyntaxDefinitions.TYPE_ANY)) return true;
    if(a.startsWith("[] ") || b.startsWith("[] "))
        if(a.endsWith("any") || b.endsWith("any")) return true;
    if(a.equals(SyntaxDefinitions.TYPE_NUMBER) || a.equals(SyntaxDefinitions.TYPE_BOOLEAN) || a.equals(SyntaxDefinitions.TYPE_CHAR))
        if(SyntaxDefinitions.types.get(b).isPrimitive()) return true;
    if(b.equals(SyntaxDefinitions.TYPE_NUMBER) || b.equals(SyntaxDefinitions.TYPE_BOOLEAN) || b.equals(SyntaxDefinitions.TYPE_CHAR))
        if(SyntaxDefinitions.types.get(a).isPrimitive()) return true;
    return a.equals(b);
}
```

Nowadays, I would avoid this problem by not using Java for an interpreter, but back then it took me a whole lot of time to grok what was happening and fix it.

## Operator Precedence

Operator precedence (or "order of operations") is relatively easy to miss in math. We quickly learn that multiplication goes before addition and raising something to the power goes before multiplication. And so, because I learned it so soon, I never actually questioned why this happens.

Now then, why does operator precedence exist? Umm... I dunno ¯\\\_(ツ)\_/¯... I guess, someone REALLY wanted to write `3 + 4 * x` instead of `4 * x + 3`... And for what gets precedence over what? Well... there is the nice and logical chain of: parentheses -> ... -> exponentiation -> multiplication -> addition -> ..., but, I guess, `log` in `log x * 4` has higher precedence than the integral in `∫xdx`? And what about set operations? What about logical operations? What about bitwise operators in C!? Oh, so `window_options & RESIZABLE != 0` is `window_options & (RESIZABLE != 0)`. Okay...

So, I either use the superior (reverse) Polish notation that no one knows, or I structure my parser around this archaic, mostly illogical, but widely accepted set of rules...
I chose the second option.

One reason for my choice was that, a little while ago, I had watched Jonathan Blow talk about his parser, and he was teaching how to solve this exact problem. Now I know that the algorithm is darn similar to a way earlier one by Vaughan Pratt, but I don't really care who discovered it and I would not be surprised if there was a rediscovery here. Anyways, this algorithm is actually really quite beautiful. You parse binary operations with increasing precedence recursively and decreasing (or equal) precedence iteratively. Since, parsing recursively naturally results in a right-leaning tree and iteratively in a left-leaning tree... I do not work with trees much, but that is still the only place where I have seen that sort of a strategy.

## Other Deceptively Simple Problems

When I was first starting on the interpreter, it was very hard for me to wrap my head around how to evaluate functions. But the answer is "just do it." A function is made up of its declaration (arguments, return parameter, and so on...) and also its body. The body in my interpreter is just an array of statements, basically an array of lines of code and a scope.
The hard part for me was actually making sure that a function would not need anything else. I eventually had to just go with it and hope, that I would not need to rewrite much.

Something that I did make more difficult for myself than it needed to be was types vs values. I tried to detect types in my lexical analyzer, so they are, weirdly, not hoisted and I need to declare them before hand and I don't really care about them in the parser. Basically, the lexer is, simply, not the time to differentiate between what is a type and what is a value. Just leave them as an "identifier." In my opinion, you should actually determine what is a type and what is not in a stage after the parser. I have seen this called the: "semantic analysis" stage in the Zig compiler. And that is what I did in my later language. But this is all language dependent.

Otherwise, for variables, I just store a `Stack<HashMap<String, Object>> scopes`. For actual binary operators (like adding 2 numbers, and so on...) I simply cast 2 values to Double-s. If statements are just if statements in java: `... if((Boolean) eval(statement.condition)) interpret(statement.body); ...`. Variable arguments just slightly worsen the possible compiler error specificity. There are more various smaller problems that were hard for me the first time around, but for the sake of brevity, I won't mention them.

## Final words

All in all, the project ended up having around 3000 lines of code. I have a small standard library, a small (second degree) wrapper over OpenGL and GLFW, and program that draws a spinning sphere of points. I further facilitated my addiction of writing parsers. I also learned a lot about Java's reflection, for the first time in my life actually saw a reason to update Java for 1.8 (huge mistake), and I became far more syntax agnostic, to the point where the difference between Rust's and Java's syntaxes is that Rust's takes longer to learn... (But do not get me wrong. I still write `#define STRUCT(NAME, ...) typedef struct { __VA_ARGS__ } N##NAME;` if only I can).

  
