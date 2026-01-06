A tool and format to generate [[API]] docs from comments in [[Java]] source code.

Javadoc will parse comments that:
1. Look like this: `/** ... */`
2. Are placed *immediately* before the class, method, or field they document
## Tags
You use tags to define certain metadata.

| Tag           | Used For | Description                                                     |
| ------------- | -------- | --------------------------------------------------------------- |
| `@param`      | Methods  | Describes a specific parameter passed to the method.            |
| `@return`     | Methods  | Describes what the method returns (not used for `void`).        |
| `@throws`     | Methods  | Lists exceptions the method might throw and why.                |
| `@see`        | All      | Adds a "See Also" link to another class or method.              |
| `@deprecated` | All      | Marks code that should no longer be used.                       |
| `@author`     | Classes  | Names the author of the code                                    |
| `{@link}`     | Inline   | Inserts a clickable link to another class within a description. |
## Example
```java
/**  
 * Represents a user in the system. * @author Michael Antar  
 * @version 1.0  
 */
 public class User {  
	/**  
     * Calculates the total price of items in the user's cart including tax.
     * <br>  
     * This method iterates through all items currently in the cart.  
     * Use {@link #clearCart} to empty the cart after purchase.  
     *
     * @param taxRate The tax rate to apply (e.g., 0.08 for *%)  
     * @param discount A fixed discount amount to subtract  
     * @return The final calculated price as a double  
     * @throws IllegalArgumentException if taxRate is negative  
     */
    public double calculateTotal(double taxRate, double discount) {  
	    if (taxRate < 0) {  
            throw new IllegalArgumentException("Tax rate cannot be negative");  
        }  
        return 100.00;  
	}  
}
```

You can then generate the javadoc using:
```bash
javadoc -d doc src/*.java
```
- Where `-d doc` puts the files in a folder named `doc`
- `src/*.java` tells which Java files to process