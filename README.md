# SauceDemo QA Automation Project (Demo link- https://www.saucedemo.com/)

## 1. Project Overview

This project is a **UI test automation framework for SauceDemo**, a sample e-commerce website.

The main goal is to verify important user journeys such as:

- Login
- Product listing and sorting
- Product details
- Add/remove products from cart
- Cart navigation
- Checkout information
- Order completion

The project uses **Python, Selenium, Pytest and the Page Object Model (POM)**.

As a QA Analyst, I would use this automation suite as a **regression test suite** to quickly verify that important e-commerce functionality is working after a new build or code change.

---

## 2. Tech Stack

| Tool / Technology | Purpose |
|---|---|
| Python | Test scripting |
| Selenium WebDriver | Browser automation |
| Pytest | Test execution and assertions |
| Page Object Model | Maintainable test design |
| Chrome | Main browser |
| Chrome Mobile Emulation | Mobile UI testing |
| Headless Chrome | Faster automated execution |
| pytest-xdist / parallel execution | Running tests concurrently |
| pytest-html | HTML test reports |
| webdriver-manager | WebDriver management |

---

## 3. Project Structure

```text
SauceDemo-Automation-master/
│
├── drivers/
│   ├── chromedriver
│   └── geckodriver
│
├── lib/
│   ├── Constants.py
│   └── LoginCreds.py
│
├── pages/
│   ├── Page.py
│   ├── LoginPage.py
│   ├── ProductListPage.py
│   ├── ProductDetailPage.py
│   ├── CartPage.py
│   ├── CheckoutPage.py
│   ├── OverviewPage.py
│   └── OrderConfirmationPage.py
│
├── tests/
│   ├── test_login.py
│   ├── test_product_list_page.py
│   ├── test_product_details_page.py
│   ├── test_cart_page.py
│   ├── test_checkout_page.py
│   ├── test_checkout_process.py
│   └── test_overview_page.py
│
├── conftest.py
├── componentTest.cfg
├── requirements
└── runAutomation.sh
```

---

## 4. Testing Approach

I divided the application into functional areas and created tests for the most important user actions.

### Functional areas covered

1. Authentication
2. Product listing
3. Product sorting
4. Product details
5. Cart
6. Checkout
7. Order completion
8. Basic mobile/browser execution

The tests contain both **positive and negative scenarios**.

### Positive examples

- Valid user can log in.
- Products are displayed.
- Products can be sorted.
- Product can be added to cart.
- Checkout can be completed.
- Order confirmation is displayed.

### Negative examples

- Locked-out user cannot log in.
- Invalid username/password is rejected.
- Blank username is rejected.
- Blank password is rejected.
- Checkout cannot continue without required information.

---

## 5. Test Case Coverage

The project contains **27 test functions**. Because most tests are parameterized for Chrome Mobile and Chrome Headless, the same scenarios can be executed in multiple environments.

### Login Testing

**File:** `tests/test_login.py`

| Test | What is verified |
|---|---|
| Valid login | Standard user can log in |
| Locked-out user | Locked user receives an error |
| Invalid credentials | Invalid login is rejected |
| Missing username | Login is rejected |
| Missing password | Login is rejected |

This gives both positive and negative coverage for authentication.

---

### Product Listing Page Testing

**File:** `tests/test_product_list_page.py`

| Test | What is verified |
|---|---|
| Get products | Products are displayed |
| A-Z sorting | Products are sorted alphabetically |
| Z-A sorting | Products are sorted in reverse order |
| Low-to-high sorting | Prices are sorted ascending |
| High-to-low sorting | Prices are sorted descending |
| Add to cart | Cart count increases correctly |
| Remove from cart | Cart count decreases correctly |
| Product images | Product image sources are checked |
| Product links | PLP links open the correct PDP |
| PLP/PDP prices | Product price is consistent between pages |

These tests cover important product browsing and cart interactions.

---

### Product Detail Page Testing

**File:** `tests/test_product_details_page.py`

| Test | What is verified |
|---|---|
| Back button | User can return to product listing |
| Add to cart | Product can be added from PDP |
| Remove from cart | Product can be removed from PDP |

The tests select a product and validate the product-detail actions.

---

### Cart Testing

**File:** `tests/test_cart_page.py`

| Test | What is verified |
|---|---|
| Continue shopping | User returns to product listing |
| Checkout button | User can move from cart to checkout |
| Remove from cart | Products can be removed from the cart |

---

### Checkout Page Testing

**File:** `tests/test_checkout_page.py`

| Test | What is verified |
|---|---|
| Information input | Required customer information is validated |
| Cancel button | User can return from checkout to cart |

The checkout test also verifies that the user cannot continue before entering required information.

---

### End-to-End Checkout Testing

**File:** `tests/test_checkout_process.py`

The complete business flow is tested:

```text
Login
  ↓
Product Listing
  ↓
Add Product(s)
  ↓
Cart
  ↓
Checkout
  ↓
Enter Customer Information
  ↓
Checkout Overview
  ↓
Verify Subtotal
  ↓
Finish Order
  ↓
Order Confirmation
  ↓
Cart Should Be Empty
```

Three important scenarios are covered:

1. Checkout with all products
2. Checkout with one product
3. Checkout with no products

The first two are normal business flows. The third is useful because it checks an unusual/edge scenario supported by the demo application.

---

### Checkout Overview Testing

**File:** `tests/test_overview_page.py`

| Test | What is verified |
|---|---|
| Cancel | User can cancel from order overview |
| Finish | User can complete the order |

---

## 6. Page Object Model

One of the most important concepts demonstrated in this project is the **Page Object Model**.

Instead of writing Selenium locators and actions directly inside every test, page-specific actions are placed inside page classes.

For example:

```text
LoginPage
    ├── enter_username()
    ├── enter_password()
    ├── click_login()
    └── perform_complete_login()

ProductListPage
    ├── get_products()
    ├── sort_products()
    ├── add_all_to_cart()
    └── click_cart()

CartPage
    ├── click_checkout()
    ├── click_continue_shopping()
    └── remove_all_from_cart()
```

### Why use POM?

If a locator changes, I can update it in the page class instead of changing every test.

This improves:

- Maintainability
- Reusability
- Readability
- Separation of test logic and UI interaction

---

## 7. Common QA Assertions Used

The tests do not only perform actions. They verify expected results using assertions.

Examples:

```python
assert len(product_names) > 0
```

This verifies that products are displayed.

```python
assert product_names[i] <= product_names[i+1]
```

This verifies A-Z sorting.

```python
assert product_page.get_number_cart_items() == 1
```

This verifies the cart count.

```python
assert overview_page.get_subheader() == "Checkout: Overview"
```

This verifies that the user reached the correct page.

```python
assert cart_total == overview_page.get_subtotal()
```

This verifies that the cart subtotal is consistent with the checkout overview.

---

## 8. Test Data

Test data is separated into:

`lib/LoginCreds.py`

Examples include:

- `standard_user`
- `locked_out_user`
- invalid credentials
- blank credentials

The project also keeps reusable UI constants in:

`lib/Constants.py`

This is useful because test data and constants do not need to be hard-coded repeatedly throughout the test files.

---

## 9. Browser and Environment Coverage

The test cases are parameterized for:

- Chrome Mobile emulation
- Chrome Headless

This means the same functional scenario can be executed in more than one browser configuration.

The framework also contains support for desktop Chrome and Firefox in its constants/configuration, although the current test parameterization mainly uses Chrome Mobile and Chrome Headless.

---

## 10. Test Categorization

Pytest markers are used to organize tests.

Examples:

```text
@login
@product_page
@product_sort
@add_cart
@cart
@checkout
@full_checkout
@plp_images
@plp_links
@plp_prices
```

This allows QA engineers to run a specific group instead of the entire regression suite.

For example:

```bash
pytest -m login
```

can be used to focus on login tests.

---

## 11. Automation Execution Flow

The framework uses `conftest.py` to create and clean up the Selenium WebDriver.

The general flow is:

```text
Pytest starts
     ↓
Create WebDriver
     ↓
Open SauceDemo
     ↓
Execute test
     ↓
Perform Selenium actions
     ↓
Validate expected result
     ↓
Close browser
```

The framework also supports parallel execution through the Pytest configuration.

---

## 12. What I Would Test Manually as a QA Analyst

Automation covers the main regression scenarios, but I would not depend only on automation.

I would additionally perform exploratory/manual testing for:

- UI layout and alignment
- Responsive behavior on different screen sizes
- Browser compatibility
- Keyboard navigation
- Accessibility
- Session behavior
- Cookies
- Network failures
- Broken/slow network conditions
- Input boundary values
- Special characters
- Very long names
- Invalid postal codes
- Refresh/back-button behavior
- Logout behavior
- Security-related scenarios

This is important because **automation verifies predefined scenarios, while exploratory testing can discover unexpected behavior.**

---

## 13. Important Gaps I Identified

While reviewing the framework, I identified a few areas that I would improve as a QA Analyst.

### 1. Some tests have weak assertions

For example, the cart checkout test mainly clicks the checkout button without explicitly asserting that the checkout page was reached.

**Improvement:** Add an assertion such as:

```python
assert checkout_page.get_subheader() == "Checkout: Your Information"
```

### 2. Payment testing is not implemented

The project contains:

```python
def input_payment_details(self):
    pass
```

The demo application does not provide a real payment flow.

**Improvement:** In a real e-commerce application, I would add payment validation and negative payment scenarios.

### 3. Random product selection

Some PDP tests randomly select a product.

This gives variety, but it can make debugging less deterministic.

**Improvement:** Use a fixed product for repeatable regression tests, or log the selected product.

### 4. Hard-coded waits

Some tests use:

```python
time.sleep()
```

Hard-coded waits can make tests slower and less reliable.

**Improvement:** Prefer Selenium explicit waits based on actual conditions.

### 5. Limited browser coverage in execution

Firefox and desktop Chrome are defined, but the current parameterized tests mainly execute Chrome Mobile and Chrome Headless.

**Improvement:** Add a browser matrix for Chrome, Firefox and relevant mobile configurations.

### 6. No API/backend testing

The project focuses on UI automation.

**Improvement:** If APIs were available, I would add API tests for product, cart and order services.

---

## 14. QA Concepts Demonstrated

Through this project, I can demonstrate knowledge of:

- Functional testing
- Positive testing
- Negative testing
- Regression testing
- End-to-end testing
- Integration-style UI flow testing
- Boundary/validation thinking
- Cross-environment testing
- Test data management
- Test automation
- Assertions
- Page Object Model
- Test parametrization
- Test categorization
- Parallel execution
- Defect-oriented thinking
- Identifying automation gaps

---

## 15. How I Would Explain This Project in an Interview

> "This is a Selenium-based UI automation framework for SauceDemo. I structured the tests around the main e-commerce modules such as login, product listing, product details, cart and checkout.
>
> I used Pytest for test execution and assertions, and Page Object Model to separate test logic from page interactions. I covered both positive and negative scenarios, such as valid login, invalid login, locked users, sorting, cart operations and checkout validation.
>
> I also created end-to-end tests where I add products to the cart, verify the cart total, enter checkout information, verify the overview subtotal and finally verify order completion.
>
> The tests are parameterized so the same scenarios can run in different Chrome configurations. I also reviewed the automation from a QA perspective and identified gaps such as weak assertions, hard-coded waits, random test data and missing payment/API testing.
>
> So the main purpose of this project is not just Selenium scripting; it demonstrates how I think about test coverage, expected results, maintainability and regression testing."

---

## 16. Key Takeaway

This project demonstrates a basic but practical QA automation approach:

**Understand the user journey → identify test scenarios → automate important scenarios → add assertions → organize tests → execute regression tests → identify gaps and improve coverage.**

The most important QA mindset I would demonstrate is:

> **Do not only test whether a button works. Test whether the user gets the correct business result after using it.**
