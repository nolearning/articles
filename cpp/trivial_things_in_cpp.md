# Trivial Things

## c++ standard
* Trivially-copyable types are defined in **[basic.types]/9**
  > Cv-unqualified scalar types, trivially copyable class types, arrays of such types, and nonvolatile const-qualified versions of these types are collectively called trivially copyable types.
* Trivially-copyable classes are defined in [class]/6
  > A trivially copyable class is a class that: has no non-trivial copy constructors, has no non-trivial move constructors, has no non-trivial copy assignment operators, has no non-trivial move assignment operators, and has a trivial destructor.
* Copy/move constructors in [class.copy]/12
  > A copy/move constructor for class X is trivial if it is not user-provided, its parameter-type-list is equivalent to the parameter-type-list of an implicit declaration, and if class X has no virtual functions and no virtual base classes, and class X has no non-static data members of volatile-qualified type, and the constructor selected to copy/move each direct base class subobject is trivial, and for each non-static data member of X that is of class type (or array thereof), the constructor selected to copy/move that member is trivial; otherwise the copy/move constructor is non-trivial.
* Copy/move assignment operators in [class.copy]/25
  > A copy/move assignment operator for class X is trivial if it is not user-provided, its parameter-type-list is equivalent to the parameter-type-list of an implicit declaration, and if class X has no virtual functions and no virtual base classes, and class X has no non-static data members of volatile-qualified type, and the assignment operator selected to copy/move each direct base class subobject is trivial, and for each non-static data member of X that is of class type (or array thereof), the assignment operator selected to copy/move that member is trivial; otherwise the copy/move assignment operator is non-trivial.
* Destructors in [class.dtor]/5
  > A destructor is trivial if it is not user-provided and if: the destructor is not virtual, all of the direct base classes of its class have trivial destructors, for all of the non-static data members of its class that are of class type (or array thereof), each such class has a trivial destructor. Otherwise, the destructor is non-trivial.
* User-provided constructors in [dcl.fct.def.default]/5
  > Explicitly-defaulted functions and implicitly-declared functions are collectively called defaulted functions, and the implementation shall provide implicit definitions for them (12.1 12.4, 12.8), which might mean defining them as deleted. A function is user-provided if it is user-declared and not explicitly defaulted or deleted on its first declaration. A user-provided explicitly-defaulted function (i.e., explicitly defaulted after its first declaration) is defined at the point where it is explicitly defaulted; if such a function is implicitly defined as deleted, the program is ill-formed.
  
  
## References
* [Which rules determine whether an object is trivially copyable](https://stackoverflow.com/questions/30096911/which-rules-determine-whether-an-object-is-trivially-copyable)
* [Why can't std::tuple<int> be trivially copyable?](https://stackoverflow.com/questions/38779985/why-cant-stdtupleint-be-trivially-copyable)
