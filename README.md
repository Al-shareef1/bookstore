# Online Book Store Project in Spring Boot

## System Architecture

The Online Book Store Project is built using the Spring Boot framework, which provides a robust environment for creating web applications with minimal configuration. The following components form the high-level architecture of the system:

- **Web Layer**: Handles incoming HTTP requests and serves the views (Thymeleaf templates).
- **Service Layer**: Encapsulates the business logic of the application, processing data between the web layer and the data access layer.
- **Repository Layer**: Interacts with the database through Spring Data JPA, providing CRUD operations on the data model entities.
- **Database Layer**: Stores data in an H2 database, providing data persistence.

### Major Components

1. **Controllers**: Handle HTTP requests, interact with services to process data, and return views.
2. **Services**: Contain business logic and interact with repositories to manage data operations.
3. **Repositories**: Define methods for accessing database entities, utilizing JPA to perform CRUD operations.
4. **Models**: Represent the data structure of the application, including `Book`, `Customer`, `Order`, and related entities.

## Core Components and Modules

### Main Classes and Methods

- **BookStoreApplication.java**: The main entry point for the Spring Boot application.
    ```java
    public static void main(String[] args) {
        SpringApplication.run(BookStoreApplication.class, args);
    }
    ```

- **BookController.java**: Handles requests related to book operations such as getting all books, adding a new book, editing, and deleting books.
    ```java
    @GetMapping(value = { "", "/" })
    public String getAllBooks(Model model, @RequestParam("page") Optional<Integer> page,
                               @RequestParam("size") Optional<Integer> size) {
        // Implementation to retrieve all books
    }
    ```

- **BookService.java**: Contains business logic for handling books, including pagination and search functionalities.
    ```java
    public Page<Book> findPaginated(Pageable pageable, String term) {
        // Method to find paginated books based on search term
    }
    ```

- **BillingService.java**: Processes customer orders, saves customer information, and retrieves orders based on customer ID.
    ```java
    @Transactional
    public void createOrder(Customer customer, List<Book> books) {
        // Implementation of order creation logic
    }
    ```

- **CheckoutController.java**: Manages the checkout process by validating customer information and finalizing orders.
    ```java
    @PostMapping("/placeOrder")
    public String placeOrder(@Valid Customer customer, BindingResult result, RedirectAttributes redirect) {
        // Logic to handle order placement
    }
    ```

- **EmailService.java**: Sends order confirmation emails to customers upon successful order placement.
    ```java
    public void sendEmail(String to, String subject, String message) {
        // Implementation to send an email
    }
    ```

### Code Snippets and Examples

Here are some key examples demonstrating the usage of the core components:

1. **Creating a new book**:
    ```java
    @PostMapping("/save")
    public String saveBook(@Valid Book book, BindingResult result, RedirectAttributes redirect) {
        // If there are validation errors, return to form
        if (result.hasErrors()) {
            return "form";
        }
        bookService.save(book);
        redirect.addFlashAttribute("successMessage", "Saved book successfully!");
        return "redirect:/book";
    }
    ```

2. **Displaying the shopping cart**:
    ```java
    @GetMapping(value = { "", "/" })
    public String shoppingCart(Model model) {
        model.addAttribute("cart",shoppingCartService.getCart());
        return "cart";
    }
    ```

3. **Processing an order on checkout**:
    ```java
    @PostMapping("/placeOrder")
    public String placeOrder(@Valid Customer customer, BindingResult result, RedirectAttributes redirect) {
        if (result.hasErrors()) {
            return "/checkout";
        }
        billingService.createOrder(customer, shoppingCartService.getCart());
        shoppingCartService.emptyCart();
        redirect.addFlashAttribute("successMessage", "The order is confirmed, check your email.");
        return "redirect:/cart";
    }
    ```

## Features

- User authentication and authorization
- Browse books by category, author, or title
- Add, edit, or delete books
- Search functionality
- Shopping cart management
- Checkout process with order history
- Admin panel for managing books and users

## Offical Website
For more information and to access the source code, visit: [Online Book Store Project](https://projectworlds.in/online-book-store-project-in-spring-boot-with-source-code/)
