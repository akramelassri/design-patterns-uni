# The Single Responsibility Principle (SRP)

The **Single Responsibility Principle (SRP)** is one of the five SOLID principles. As "Uncle Bob" (Robert C. Martin) mentions in _Clean Architecture_, the standard definition is:

> "A class should do **one and only one thing.**"

However, I find this definition incredibly confusing. When I first read it, I fell into a common trap: I started dividing classes into multiple tiny subclasses, thinking I was making the code cleaner. In reality, I was just making the project harder to read and navigate.

## A Better Definition

To truly understand SRP, we need to look at a better definition:

> "A class should have **only one reason to change.**"

This definition solves the problem of the old one. It gives us a concrete standard to measure our code against. To test if your code respects SRP, just ask:

**"If I want to change X, where do I go?"**

If the answer involves a class that also handles Y and Z, you have likely violated the principle.

---

## Example 1: The Classic Book Problem

Consider this simple Java class:

```java
public class Book {

    private String name;
    private String author;
    private String text;

    // Methods that directly relate to the book properties
    public String replaceWordInText(String word, String replacementWord){
        return text.replaceAll(word, replacementWord);
    }

    public boolean isWordInText(String word){
        return text.contains(word);
    }

    void printTextToConsole(){
        // Our code for formatting and printing the text
    }
}

```

At first glance, this looks fine. But it violates SRP because it has **two reasons to change**:

1. **Business Logic:** If we want to change how we manipulate the text (e.g., word replacement logic).
2. **Display Logic:** If we want to change how the text is displayed (e.g., printing to a GUI instead of the console).

Because these two responsibilities meet in the same class, the solution is to extract the printing logic:

```java
public class BookPrinter {
    // Methods for outputting text
    void printTextToConsole(String text){
        // Our code for formatting and printing the text
    }
}

```

---

## Example 2: The "Hidden" Violation

The `Book` example is easy, but real-world violations are often subtler. Let's look at a `UserProfileManager`.

```java
import java.io.File;

public class UserProfileManager {

    public UserProfile getProfile(int userId) {
        // Fetch user from DB
        return new UserProfile();
    }

    public void updateProfile(int userId, UserProfile data) {
        // Save new user profile
    }

    public void uploadProfilePicture(int userId, File image) {
        // Save image to S3
    }

    public void deleteAccount(int userId) {
        // Delete user from DB and clear sessions
    }
}

```

If I asked you, _"Does this violate SRP?"_, what would you say?

If you said _"No, because it only does one thing: manage profiles,"_ I am sorry, but **you are wrong.**

This class actually handles three different things, which means it has **three different reasons to change**:

1. **Data Management** (`getProfile`, `updateProfile`): These methods only change if the user data structure changes.
2. **Infrastructure** (`uploadProfilePicture`): This changes if your infrastructure changes. For example, if you switch from storing images in AWS S3 to Cloudflare R2, you have to edit this class.
3. **Business Policy** (`deleteAccount`): Account deletion is often tied to business rules (policy violations, inactivity, strict GDPR compliance). If the business policy changes, this method changes.

### The Solution

Since the class has three reasons to change, it should be split into three separate classes:

```java
// Handles basic user data (Data Management)
public class UserProfileService {
    public UserProfile get(int userId) {
        // ... logic ...
        return new UserProfile();
    }

    public void update(int userId, UserProfile data) {
        // ... logic ...
    }
}

// Handles file storage (Infrastructure)
public class UserPictureService {
    public void upload(int userId, File file) {
        // upload to S3
    }
}

// Handles account lifecycle and rules (Business Policy)
public class UserAccountManager {
    public void delete(int userId) {
        // delete from DB, clear sessions, send goodbye email
    }
}

```

---

## Conclusion

Stop asking if your class "does one thing." That is too vague. Instead, ask yourself:

> **"Does this class have more than one reason to change?"**

If the answer is yes, break it apart. Your future self (and your team) will thank you.
