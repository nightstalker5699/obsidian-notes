![[Pasted image 20250116193141.png]]


![[Pasted image 20250116193218.png]]


![[Pasted image 20250116193255.png]]

another thing is structs can be declared without the "new" keyword 
example:


![[Pasted image 20250116193818.png]]

a Struct can have fields or properties also a constructor and a method 
same as any class 
NOTE: to assign value to struct variable you must have field not property

![[Pasted image 20250116194430.png]]


![[Pasted image 20250116194501.png]]


as you can see we used a constructor for the first "P"
second one we declared it without the new keyword
and the last one we copied the p1 values to p2 and changed it's y variable to 5 
this one show that structs are value type not reference type because in reference type variables the y variable would be 5 to both p2 and p1 
Common practice is to make the fields or properties read-only either be read-only keyword or by removing the set method in it 