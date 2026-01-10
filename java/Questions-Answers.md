abstract & interface --> cant be create Objects 
abstract --> constructor we can create
interface --> constructor we can't create

The **abstract class constructor** helps **initialize common properties** for all subclasses.
example: 
abstract class **Animal** {
    **Animal**() {
        System.out.println("Animal constructor called");
    }
}
class Dog extends **Animal** {
    Dog() {
        System.out.println("Dog constructor called");
    }
}
public class Main {
    public static void main(String[] args) {
        Dog d = new Dog();
    }
}



what is hashing?
: hashing converts data into a fixed length code, often used for passwords.

what is sql injection?
: A security attack where sql quries are manipulated through user input.

how to prevent sql injection?
use prepared statements and input validation.

cors and csrf?

rate limiting
authentication
: verifying user identity before granting access.

authorization
:Determining what resources a user can access

