
Red doesn't know classes. Instead, it only uses objects, which contain data (attributes or fields) and/or functions (methods). You create objects either by cloning existing objects or starting from the base `object!` value. This is why Red has a prototype-based object model (instead of a class-based one)—objects are made by reusing existing objects. This model has the advantage of being simple and efficient. Other languages that work the same way for objects are JavaScript and Lua.

The Red system also has internal objects. An important example is the `system` object we explored in [Chapter 3](#6609439d-2797-41d5-97e3-5b2efe61d4bb.xhtml), *Using Words, Values, and Types*.

# Creating objects

The first way to create an object is to use `make` on the base `object!` datatype. Let's create a `square1` object with a `length` value and a `display` function, as follows:

```
square1: make object! [
    length: 10
    display: does [ print ["Length of square1 is" length] ]
]   ;== make object! [ ... ]
```

Secondly, instead of `make object!`, you can use the shorter `object` or `context`.

To check whether a word refers to an object, use `object?`:

```
object? square1     ;== true
ser: [2 7 42 108]
object? ser         ;== false
```

# Working with fields and functions

Accessing a variable or a function within an object is done using the `/` (path) notation, which we already encountered as the way to access the elements of a series by index, or using the file-path notation:

```
square1/length    ;== 10
square1/display   ; Length of square1 is 10
```

This works the same as the `.` (dot) notation using the `square1.length` and `square1.display()` in other languages.

In the same way, you can change data in the object:

```
square1/length: 5   ;== 5
square1/length      ;== 5
```

Another way to do this uses `get in` or `set in` to access a field using the literal `'` word notation:

```
get in square1 'length      ;== 10
set in square1 'length 5    ;== 5
```

The `set` word can also be used to initialize all fields to `none`, or to give one or more fields a new value:

```
set square1 none         ;== none
square1                  ;== make object! [length: none display: none]
set square1 [3 "Red"]    ;== [3 "Red"]
square1                  ;== make object! [length: 3 display: "Red"]
```

An object can contain executable code outside of its functions. This code is executed when constructing the object, and only then:

```
square1: make object! [
    print "Entering square1"
    length: 10
    display: does [ print ["Length of square1 is" length] ]
]   ;== make object! [ ... ]
; output:
; Entering square1 ;== make object! [ ... ]

square1/length     ;== 10
```

If the value of a field is not known during object creation, simply give it a value of `none`:

```
person: object [
    name: none
    show: does [print name]
]   ;== make object! [ ... ]

person/name: "Johnson"
person/show ;== "Johnson"
```

An object can refer to itself in its own code with `self`:

```
creature: object [
    name: "Amygdala"
    show: does [print self]
]   ;== make object! [ ... ]

creature/show
; name: "Amygdala"
; show: func [][print self]
```

This is analogous to `self` or `this` in other languages.

Objects can be nested—that is, an object can contain other objects. For example, a `person` object could contain an `address` field, which in turn could contain `street`, `city`, `state`, and `postal-code` fields:

```
person1: object [ 
    name: "Mueller" 
    tel: none 
    address: object [
        street: "Kensington Street 28"
        postal-code: 86512
        city: "Burmington"
        state: "OH"
    ]
 ]

person1/address/state    ;== "OH"
```

Nested fields are accessed with a composite `/` notation.

# Copying objects and inheritance

Let's start with a really simple person object—`person1: object \[ name: "Mueller" tel: ""\]`.

The following code applies what we have learned in the *Assigning and copying* section in [Chapter 3](#6609439d-2797-41d5-97e3-5b2efe61d4bb.xhtml), *Using Words, Values, and Types—*namely, `""`:

```
person1: object [ name: "Mueller" tel: none ]
person2: person1
person1/tel: "108-4271"
person2/tel  ;== "108-4271"
person3: copy person1
person3 ;== make object! [ name: "Mueller" tel: "108-4271"]
person1/tel: "333-4271"
person3 ;== make object! [ name: "Mueller" tel: "108-4271"]
```

The `person2` word references the same object as `person1`, so it changes whenever `person1` changes. The `person3` word is a *new object,* with all values copied from `person1` (a clone), and so it is independent from `person1`, but it also consumes additional memory.

In object-oriented languages, you can make a class inherit all its properties and methods from a superclass, and add some new ones of its own. This can be done very easily in Red—Use `make` with the starting object, and add new properties and/or methods. This is shown as follows:

```
person4: make person1 [id-number: #MFG-932-741-A]
person4 
;== make object! [name: "Mueller" tel: "333-4271" id-number: #MFG-932-741-A]
person4/name: "Johnson"
person1 ;== make object! [name: "Mueller" tel: "333-4271"] 
```

The `person4` object is a new object, which has inherited the contents of `person1`, but can change independently. You could also change field values while creating the new object, which is shown as follows:

```
person4: make person1 [name: "Johnson" id-number: #MFG-932-741-A]
```

Now we understand that a `copy` of a `person1` object can also be written as `make person1 \[\]` .

\=> Now answer question 10 from the *Questions* section.

This mechanism can also be used to merge two or more objects, a kind of multiple inheritance:

```
a: object [x: 5]
b: object [y: 10]
c: make a b             ;== make object! [x: 5 y: 10]
d: object [x: 3 y: 10]
c: make a d             ;== make object! [x: 3 y: 10]
```

If the starting objects contain the same field(s), the last object takes priority.

Through copying and inheriting, it is easy to create a lot of objects. Objects are datatypes that consume memory, a lot of memory if the objects are big and you keep many of them around. In such a case, it is better to keep cloning objects at a minimum, and instead work with references to objects. Also, give objects you don't need anymore the `none` value.

So you see, creating objects from existing objects is very flexible!

In the current release (0.6.3), Red has no `garbage collector` (`GC`) to clean up and regain unused memory. That may limit long-running programs, or ones that do a lot of data manipulation. A simple version of a GC will be available in Version 0.6.4, while a full version will integrate with the normal master branch in Version 0.8.5.

# Looking inside an object

Sometimes, you need to find out whether a certain object has a certain field (or function). The `find` phrase returns `true` if the field exists, `none` if it doesn't:

```
find person1 'tel         ;== true
find person1 'id-number   ;== none
```

Again, you have to use the `'` literal word syntax. Once you know the field exists, access its value with the familiar path notation, such as `person1/tel ;== "333-4271"`. However, you could also use `select` with the same notation:

```
select person1 'tel          ;== "333-4271"
select person1 'id-number    ;== none
```

Some reflective words exist to get the contents, the list of fields, or the list of the values of an object, as shown in the following snippet:

```
person1: object [ name: "Mueller" tel: "333-4271" ]
body-of person1      ;== [name: "Mueller" tel: "333-4271"]
words-of person1     ;== [name tel]
values-of person1    ;== ["Mueller" "333-4271"]
```

A powerful `on-change*` function can be defined inside an object, which detects when a field in an object is changed (with the `/` syntax). The changed field is contained in `word`; `old` and `new` contain the former and the changed value. Here is an example that shows that a person's name has been changed:
