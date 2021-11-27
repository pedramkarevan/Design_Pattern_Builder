# Design_Pattern_Builder

Builder Design Pattern
It is a creational design pattern; means we are going to use builder when we are creating objects of a class.

What problem builder design pattern solves?
-	Class constructor requires a lot of information.

Example: 
Product instances are immutable. An immutable object is an object whose states or properties cannot be changed once that is created. In other word, the value of created object can be changed. String class is a good example. Once you create a string object you cannot change its value.
When you are writing immutable class, you will often end up with constructors with multiple parameters. Because, there is no setters and you have to provide all this information for the object in the constructor itself.
Having a method or constructor with multiple parameters is a bad coding practice.

Class Product {
	Public Product (int weight, double price, int shipVolume, int shipCode)
	//initialize
}

Usually we put this in a jar file for other users and developers and using this type of constructor need a good documentation.
In this case the builder pattern helps us in two ways:
First of all, it will make it easy to use such constructors. And second, it will help us to avoid to writing this constructor in first place and still have an object immutable.

-	Objects that need other objects or “parts” to construct them.
Example:

Class Address {
	Public Address (short houseNumber, String street, …)
	//initialize
}
---------------------------------------------------------------------------------------

Class User {
	Public User (String name, Address address, LocateDate birthdate, List<Role> roles)
	//initialize
}

If you look at constructor of user class, you will see that I need address object and a collection of role objects in order to create one user object. So, these are the “parts” to assemble the user objects.
Second point is that you have to first create the address object and multiple role objects and then we can call our user constructor.
In this case using the builder pattern is better.

What is a Builder?

-	We have a complex process to construct an object involving multiple steps 
-	In builder we remove the logic related to object construction from “client” code and abstract it in separate classes.

UML Diagram:
 
  1-	We want to create object of product class and creating this object by itself is complex process.

  2-	So, we have a builder, the builder will define methods that will allow the user to build parts of our object. 

  In first problem builder allows us to specify these arguments one at the time or It has a method to assemble the final object.
  Builder can also provide a method that users can use to query already built object.

  3-	We have a concreate builder, it is an implementation of builder interface or abstract class.

  4-	Finally, we have a director, now somebody has to call these methods and there could be a sequence in which these methods need to be called. There is a logic for builder and that logic is provided by builder. The director knows how to use a builder object to create our final product.


Step by step Implementation:

1-	Creating a builder
    a.	Identify the “parts” and provide the method to create those parts.
    
    b.	Provide a method to assemble or build final object
    
    c.	Provide a method to get fully built object out
    
2-	Creating director 

  a.	Can be separate class or client can play the role of director

-------------------------------------------------------------------------------------------------------

Another way to implement this pattern
  We have a userDTO class and we want to create object of this class. We do not have a constructor for the class for its arguments and we still want to create an immutable instance of the class.
  We have public getter for properties to allow reading object state, but we have setter method which is declared as private that means that these setter methods are not accessible outside of the class. So, userDTO object will be an immutable object.
  How is affect the builder?  We should declare a static inner class -UserDTOBuilder-and it provides a nice namespace for our builder. Second benefit is that this builder able to use setter methods which are private on the class and in this way we can build an immutable object without writing a complex constructor or constructor with a lot of information. We still have common method which return object of userDTO, we still have a build method but instead of using constructor of class in this method we should use private setter methods of final object.
  We can have a static method inside of the final class for getting builder which return userDTOBuilder object (inner class). It is not necessary but it is a good practice.
  As far as client is concerned, not much has changed, still have a director and instead of using constructor we can use static method which was defined in the final class and rest of implementation remains same.

Implementation Consideration

    -	You can create an immutable class by implementing builder as an inner static class. You will find this type of implementation used quite frequently even if immutability is cot a main concern.
    
Design Consideration
    
    -	The director class rarely implemented as separated class, typically the consumer of object instance or the client handle that role 
    
    -	Abstract builder is also not required if product is not part of any inheritance hierarchy. You can directly create concrete builder
