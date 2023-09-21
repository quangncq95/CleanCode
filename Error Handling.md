## Error handling
### Use exceptions rather than return codes
The caller must check for errors immediately after the call. Unfortunately, it’s easy to forget. For this reason it is better to throw an exception when you encounter an error.
### Provide context with exceptions
Each exception that you throw should provide enough context to determine the **source and location of an error**. In Java, you can get a stack trace from any exception; however, a stack trace can’t tell you the intent of the operation that failed. 
**Create informative error messages** and pass them along with your exceptions. Mention the **operation that failed and the type of failure**. If you are logging in your application, pass along enough information to be able to log the error in your catch.
### Don't return null
If you are tempted to return null from a method, consider throwing an exception or returning a SPECIAL CASE object instead.
Consider:
```
List<Employee> employees = getEmployees();
if (employees != null) {
 for(Employee e : employees) {
 totalPay += e.getPay();
 }
}
```
Right now, `getEmployees` can return null, but does it have to? If we change `getEmployee` so that it returns an empty list, we can clean up the code:
```
List<Employee> employees = getEmployees();
for(Employee e : employees) {
 totalPay += e.getPay();
}

public List<Employee> getEmployees() {
 if( .. there are no employees .. )
 return Collections.emptyList();
}
}
```
### Don't pass null

