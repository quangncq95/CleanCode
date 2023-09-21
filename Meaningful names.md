## Meaningful names
### Intention-revealing names
Choosing names that reveal intent can make it much easier to understand and change code.

Bad:
`var d; // elapsed time in days`
Good:
```
var elapsedTimeInDays;
var daysSinceCreation; 
var daysSinceModification; 
var fileAgeInDays;
```
### Avoid disinformation
Programmers must avoid leaving false clues that obscure the meaning of code.
```
Do not refer to a grouping of accounts as an accountList unless it’s actually a List. 
The word list means something specific to programmers. 
If the container holding the accounts is not actually a List, 
it may lead to false conclusions. 
So accountGroup or bunchOfAccounts or just plain accounts would be better.
```
### Make meaningful distinctions
Because you can’t use the same name to refer to two different things in the same scope, you might be tempted to change one name in an arbitrary way.
It is not sufficient to add number series (example : name1 , name2...) or noise words ( example: ProductInfo , ProductData ), even though the compiler is satisfied. If names must be different, then they should also mean something different.
Bad:
```
class UserInfo{}
class UserData{}
```
Good:
```
class UserPersonalInfo()
class UserBankingData()
```
### Use searchable names
Single-letter names can ONLY be used as local variables inside short methods. _The length of a name should correspond to the size of its scope._
Bad:
```
for (int j=0; j<34; j++) { 
	s += (t[j]*4)/5; 
}
```
Good:
```
int realDaysPerIdealDay = 4; 
const int WORK_DAYS_PER_WEEK = 5;
 int sum = 0; 
 for (int j=0; j < NUMBER_OF_TASKS; j++) {
	  int realTaskDays = taskEstimate[j] * realDaysPerIdealDay;
	   int realTaskWeeks = (realdays / WORK_DAYS_PER_WEEK); sum += realTaskWeeks; 
 }
 ```

consider how much easier it will be to find WORK_DAYS_PER_WEEK than to find all the places where 5 was used.
### Member prefixes
You also don’t need to prefix member variables with `m_` anymore. Your classes and functions should be small enough that you don’t need them. And you should be using an editing environment that highlights or colorizes members to make them distinct.
Use ( and editor support code highlight) :
```
class Part { 
	String description; 
	void setDescription(String description) { 
		this.description = description; 
	}
}
```
Instead:
```
class Part { 
	String m_description; 
	void setDescription(String description) { 
		this.description = description; 
	}
}
```
### Class names
Classes and objects should have noun or noun phrase names like `Customer`, `WikiPage`,` Account`, and `AddressParser`. Avoid words like `Manager`, `Processor`, `Data`, or `Info` in the name of a class. A class name should not be a verb.
### Method names
Methods should have verb or verb phrase names like `postPayment`, `deletePage`, or `save`.
### Pick one word per concept
Pick one word for one abstract concept and stick with it. For instance, it’s confusing to have `fetch`, `retrieve`, and `get` as equivalent methods of different classes.

Likewise, it’s confusing to have a `controller` and a` manager` and a `driver` in the same code base. What is the essential difference between a `DeviceManager` and a `ProtocolController`? Why are both not `controllers` or both not `managers`? Are they both Drivers really? The name leads you to expect two objects that have very different type as well as having different classes.
### Avoid using the same word for two purposes
### Use solution domain names
Remember that the people who read your code will be programmers. So go ahead and use computer science (CS) terms, algorithm names, pattern names, math terms, and so forth.
### Add meaningful context
There are a few names which are meaningful in and of themselves—most are not. Instead, you need to place names in context for your reader by enclosing them in well-named classes, functions, or namespaces.
