# C++ declartion and definition

## in c
> A declaration specifies the interpretation and attributes of a set of identifiers. A definition of an identifier is a declaration for that identifier that:  
> * for an object, causes storage to be reserved for that object;
> * or a function, includes the function body;
> * for an enumeration constant, is the (only) declaration of the identifier;
> * for a typedef name, is the first (or only) declaration of the identifier.  
> An initializer specifies the initial value stored in an object ... if an object that has automatic storage is not initialized explicitly, its value is indeterminate.

> Referencing The C programming language by Dennis Ritchie  
> A declaration announces the properties of a variable while,  
> A definition also causes storage to be set aside. 

```
excerpt from Statements and blocks

statement:
    labeled-statement
    compound-statement
    expression-statement
    selection-statement
    iteration-statement
    jump-statement
```

## c++ standard
From the C++ standard section 3.1:
> A declaration introduces names into a translation unit or redeclares names introduced by previous declarations. A declaration specifies the interpretation and attributes of these names.  
> A declaration is a definition unless it declares a function without specifying the function's body, it contains the extern specifier or a linkage-specification and neither an initializer nor a function-body, it declares a static data member in a class declaration, it is a class name declaration, or it is a typedef declaration, a using-declaration, or a using-directive.


One Definition Rule. From section 3.2.1 of the C++ standard:
> No translation unit shall contain more than one definition of any variable, function, class type, enumeration type, or template.

# c++ 17
```
excerpt from [gram.stmt]

statement:
    labeled-statement
    attribute-specifier-seqopt expression-statement
    attribute-specifier-seqopt compound-statement
    attribute-specifier-seqopt selection-statement
    attribute-specifier-seqopt iteration-statement
    attribute-specifier-seqopt jump-statement
    declaration-statement
    attribute-specifier-seqopt try-block

init-statement:
    expression-statement
    simple-declaration

declaration-statement:
    block-declaration

excerpt from [gram.dcl]

declaration:
    block-declaration
    nodeclspec-function-declaration
    function-definition
    template-declaration
    deduction-guide
    explicit-instantiation
    explicit-specialization
    linkage-specification
    namespace-definition
    empty-declaration
    attribute-declaration

block-declaration:
    simple-declaration
    asm-definition
    namespace-alias-definition
    using-declaration
    using-directive
    static_assert-declaration
    alias-declaration
    opaque-enum-declaration

simple-declaration:
    decl-specifier-seq init-declarator-listopt ;
    attribute-specifier-seq decl-specifier-seq init-declarator-list ;
    attribute-specifier-seqopt decl-specifier-seq ref-qualifieropt [ identifier-list ] initializer ;

```
see more in
[cppreference Declarations](https://en.cppreference.com/w/cpp/language/declarations)

## const variables
C++03 Standard Annex C Compatibility C.1.2 Clause 3: basic concepts
> Change: A name of file scope that is explicitly declared const, and not explicitly declared extern, has internal linkage, while in C it would have external linkage   
> Change: A name of file scope that is explicitly declared const, and not explicitly declared extern, has internal linkage, while in C it would have external linkage

| internal linkage | external linkage |
| ---------------- | ---------------- |
|        const     | extern const     |
|  static type     | type             |

## references
* [Declarations/definitions as statements in C and C++](https://stackoverflow.com/questions/49861965/declarations-definitions-as-statements-in-c-and-c)
* [What is the difference between a definition and a declaration?](https://stackoverflow.com/questions/1410563/what-is-the-difference-between-a-definition-and-a-declaration)
* [const and global](https://stackoverflow.com/questions/9032475/const-and-global)
