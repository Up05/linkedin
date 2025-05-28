
Why?




Type System Fun

At first, my type system was incredibly simple, there was only `number`, `bool`, `string`, `char` and a list (dynamic array) type. 
I also had structs and hash maps planned but just never got arround to them. But all simplicity went away once I started interacting with other Java code,
i.e. bindings/foreign functions, so now I have `varargs` as a type, `any` and you can also use `typedef` with `foreign` and I just have this beautiful block of:

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
```

Java's reflection was actually a massive obstacle for my interpreter. Since, firstly, primitive types cannot be passed as a generic 
(that's why you use `List<Float>` and `Map<String, Integer>`). 
When you try to pass them wrapped in, for example, an `Object` the primitives simply decay to their respective wrapper types
and so in the later stages of the project, types sometimes just kind of failed...

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

And even once I realized that this decaying was happening, I still had to deal with primitives and wrappers being different everywhere they could be, so:

I have this (Interpreter):
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
    // I hate this... But just because of 'SyntaxDefinitions'
    if(a.equals(SyntaxDefinitions.TYPE_NUMBER) || a.equals(SyntaxDefinitions.TYPE_BOOLEAN) || a.equals(SyntaxDefinitions.TYPE_CHAR))
        if(SyntaxDefinitions.types.get(b).isPrimitive()) return true;
    if(b.equals(SyntaxDefinitions.TYPE_NUMBER) || b.equals(SyntaxDefinitions.TYPE_BOOLEAN) || b.equals(SyntaxDefinitions.TYPE_CHAR))
        if(SyntaxDefinitions.types.get(a).isPrimitive()) return true;
    return a.equals(b);
}


```


For one, primitive types, like: `float` cannot be passed to a generic function,
which, by the way, is also a small annoyance when, for example, creating a `List<Float>`or `Map<Integer, ..>`. Now, this seems pretty fine at first, I mean,
just use capitalized primitives. Which I actually did, At first, I was using the `Double` type for everything. I also tried the `Number` type, but to do
a binary operation (like addition) requires 2 switch statements over the `Number`. However, whenever I got out of the pure, nice world of the interpreter
and starting using LWJGL (GLFW and OPENGL bindings for Java), well, C doesn't want an `Integer`, it wants an `int`... But it's fine, I can just cast one to
another, when I need to throw it into C land. That's when you try to use the beautiful `glByfferData` which, in fact, requires either a `float []` or a `FloatBuffer`
(just kidding, it requires a `float []`   **CHECK THIS LATER ME**   ). Another fun thing I came across was comparing two types and whether they were castable or not.
