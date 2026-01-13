Implements the [[Model View Controller (MVC)|MVC]] pattern using the [[Front Controller Pattern]] strategy for [[Spring Framework|Spring]]. Used for *web applications*.

It involves a single "master" [[Servlets|servlet]] (the `DispatcherServlet`) that receives all incoming requests and routes them to a `Controller`. The model is represented by a POJO. The view is the UI technology that generates the HTML.

**The Model**: A POJO to hold data
```java
package com.example.demo.model;

public class User {
    private String name;
    private String email;
	
    public String getName() { return name; }
    public void setName(String name) { this.name = name; }
    public String getEmail() { return email; }
    public void setEmail(String email) { this.email = email; }
}
```

**The Controller**: Handles the traffic
```java
@Controller
public class UserController {
	
    // 1. Show the Form (GET Request)
    @GetMapping("/register")
    public String showForm(Model model) {
        // Create a blank user object to hold form data
        model.addAttribute("user", new User());
        return "user-form"; // Looks for resources/templates/user-form.html
    }

    // 2. Process the Form (POST Request)
    @PostMapping("/register")
    public String submitForm(@ModelAttribute User user, Model model) {
        // 'user' now contains the data typed in the form
        // We add it back to the model to display it on the result page
        model.addAttribute("user", user);
        return "user-result"; // Looks for resources/templates/user-result.html
    }
}
```