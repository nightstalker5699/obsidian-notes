take a look at this example 
![[Pasted image 20250115191754.png]]
as we see the builder class is defining his own tools like the saw and hammer 
and that means he is depend on the tools to work without them he can't work 

so we use technique called **Dependency injections**   
![[Pasted image 20250115192650.png]]
instead of the builder bring his tools , someone (dependency injector) bring them to the builder
and there 3 there methods : 
Constructor / Setter / interface Dependency injectors 

#### Constructor:
![[Pasted image 20250115192943.png]]


the Dependency is provided through the Constructor Parameters instead of creating one in the class itself 
but dependency can't be changed afterward (bad thing)


![[Pasted image 20250115193525.png]]


#### Setter:
![[Pasted image 20250115193036.png]]

in this one we can create a property that will hold the dependency and set it outside of the class definition Code


class definition:
![[Pasted image 20250115194304.png]]

program Main :
![[Pasted image 20250115194315.png]]

this approach give us more flexibility than constructor one because the dependencies are created outside and builder class dependency can be changed after it creation
but dependencies is not set at object creation so might lead to potential null reference errors 

#### Interface:
![[Pasted image 20250115193111.png]]


in this one we set the dependencies by creating a interface that will set the dependencies for the class and create the class with this interface after that we call the interface methods to inject the dependencies 
interface definition:

![[Pasted image 20250115200121.png]]


Class definition: 
![[Pasted image 20250115200200.png]]

program main:
![[Pasted image 20250115200224.png]]
this approach provide us with interface advantages but the cons are it requires additional interfaces to accommodate each class dependencies  





