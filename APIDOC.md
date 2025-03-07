# ECommerce Store API Documentation
*This API allows developers to interact with our ecommerce platform by handling
key functionalities such as account management, processing sales, and storing
important user data.*

## Create account
**Request Format:** /create-account

**Request Type:** POST
**Parameters:** "username" (String), "email" (String), "password" (String)

**Returned Data Format**: Plain Text

**Description:** Creates a new user account if the provided username is unique

**Example Request:**

POST request: /create-account,
{
    "username": "user1",
    "email": "user1@email.com",
    "password": "pass1"
}

**Example Response:**

```
"Account successfully created."
```

**Error Handling:**
- 400 Bad Request Error:
    - If any of the three parameters (username, email, password) are not given, returns error with response: "Missing parameters. Please try again."
    - If there is already a user with the given username, returns an error with response: "There is already a user with the username [inputted username]. Please create a different username."
    - If there is already a user with the given email, returns an error with response: "There is already a user with the email [inputted email]. Please use a different email address."

- 500 Internal Server Error:
    - If an error occurred on the server, returns an error with the response: “An error occurred during account creation. Please try again later.”

## Log into account
**Request Format:** /log-in

**Request Type:** POST
**Parameters:** "username" (String), "password" (String)

**Returned Data Format**: Plain Text

**Description:** Logs the user in upon successful verification

**Example Request:**

POST request: /log-in
{
    "username": "user1",
    "password": "pass1"
}

**Example Response:**

```
“Login successful.”
```

**Error Handling:**
- 400 Bad Request Error:
    - If either of the parameters (username, password) are not given, returns an error with response: "Missing parameters. Please try again."
    - If the password does not match the actual password for the given username, returns an error with response: "Incorrect username or password. Please try again."

- 500 Internal Server Error:
    - If an error occurred on the server, returns an error with the response: "An error occurred during account login. Please try again later."

## Log out of account
**Request Format:** /log-out with POST parameter "username"

**Request Type:** POST

**Returned Data Format**: Plain Text

**Description:** Logs a user of out their account

**Example Request:** /log-out with "username=ilovegardening"

**Example Response:**
```
Successfully logged out.
```
**Error Handling:**
- 400 Bad Request Error:
    - If the user does not exist, returns an error with response: "User does not exist. Please try again."

- 500 Internal Server Error:
    - If an error occurred on the server, returns an error with the response: "An error occurred on the server. Try again later."

## Buy item
**Request Format:** /buy

**Request Type:** POST
**Parameters:** "items" (String)

**Returned Data Format**: JSON

**Description:** Processes a buy order from the user

**Example Request:**

POST request: /buy
{
  "cart": "[\"Stainless Steel Water Bottle\",\"Stainless Steel Water Bottle\"]"
}

**Example Response:**

```
[
  {
    "item": "Stainless Steel Water Bottle",
    "price": 19.99,
    "rating": 4.9,
    "description": "Insulated water bottle to keep drinks cold or hot for hours.",
    "image": "stainless-steel-water-bottle"
  },
  {
    "item": "Stainless Steel Water Bottle",
    "price": 19.99,
    "rating": 4.9,
    "description": "Insulated water bottle to keep drinks cold or hot for hours.",
    "image": "stainless-steel-water-bottle"
  }
]
```

**Error Handling:**
- 400 Bad Request Error:
    - If the given cart is empty, returns an error with response: "Cart is empty."
    - If the username is not given, returns an error with response: "Invalid or missing parameters. Please try again."
    - If the product does not exist within the database, returns an error with response: "item does not exist. Please try again."
    - If the stock is 0, returns an error with response: "Item is currently out of stock."

- 500 Internal Server Error:
    - If an error occured on the server, returns an error with the response: "An error occured during the buy process. Please try again
    later."
    - Errors are logged during failure to update balance or stock.

## Retrieve all products
**Request Format:** /products/all

**Request Type:** GET

**Returned Data Format**: JSON

**Description:** Retrieves all the products in the store

**Example Request:** /products/all

**Example Response:**
```json
[
  {
    "item": "Bamboo Toothbrush",
    "image": "bamboo-toothbrush",
    "price": 4.99,
    "rating": 4.8
  },
  {
    "item": "Reusable Shopping Bag",
    "image": "reusable-shopping-bag",
    "price": 9.99,
    "rating": 4.7
  },
    ...
  {
    "item": "Zero Waste Toothpaste Tablets",
    "image": "biodegradable-toothpaste-tablets",
    "price": 9.99,
    "rating": 4.5
  }
]
```

**Error Handling:**
- 500 Internal Server Error:
    - If an error occured on the server, returns an error with the response: "An error occurred on the server. Try again later."

## Search and (optionally) filter products
**Request Format:** /products/search with 5 parameters ("search-term", "product-category", "max-price", "min-rating", "max-rating")

**Request Type:** GET

**Returned Data Format**: JSON

**Description:** Searches for the products matching for a search term or searches and filters the products matching the requirements

**Example Request 1:** /products/search?search-term=ho&product-category=Home&max-price=25&min-rating=0&max-rating=5

**Example Response 1:**
```json
[
  {
    "item": "Compostable Trash Bags",
    "image": "compostable-trash-bags",
    "price": 12.99,
    "rating": 4.6
  },
  {
    "item": "Soy Wax Candles",
    "image": "soy-wax-candles",
    "price": 19.99,
    "rating": 4.8
  },
    ...
  {
    "item": "Organic Wool Dryer Balls",
    "image": "organic-wool-dryer-balls",
    "price": 14.99,
    "rating": 4.8
  }
]
```

**Example Request 2:** /products/search?search-term=ho

**Example Response 2:**
```json
[
  {
    "item": "Reusable Shopping Bag",
    "image": "reusable-shopping-bag",
    "price": 9.99,
    "rating": 4.7
  },
  {
    "item": "Stainless Steel Water Bottle",
    "image": "stainless-steel-water-bottle",
    "price": 19.99,
    "rating": 4.9
  },
    ...
  {
    "item": "Organic Wool Dryer Balls",
    "image": "organic-wool-dryer-balls",
    "price": 14.99,
    "rating": 4.8
  }
]
```

**Error Handling:**
- 400 Bad Request Error:
    - If min-rating and max-rating is provided but max-rating is less than min-rating, return error with response: "min-rating must be less than or equal to max-rating."
    - If searchTerm is not given, returns error with response: "Missing parameters. Please try again."

- 500 Internal Server Error:
    - If an error occured on the server, returns an error with the response: "An error occured during the buy process. Please try again
    later."

## Applies filters to all products
**Request Format:** /products/filter with four query parameters ("product-category", "max-price", "min-rating", "max-rating")

**Request Type:** GET

**Returned Data Format**: JSON

**Description:** Retrieves the products from the entire store that matches the given filters

**Example Request:** /products/filter?max-price=15&min-rating=1&max-rating=5

**Example Response:**
```json
[
  {
    "item": "Bamboo Toothbrush",
    "image": "bamboo-toothbrush",
    "price": 4.99,
    "rating": 4.8
  },
  {
    "item": "Reusable Shopping Bag",
    "image": "reusable-shopping-bag",
    "price": 9.99,
    "rating": 4.7
  },
    ...
  {
    "item": "Zero Waste Toothpaste Tablets",
    "image": "biodegradable-toothpaste-tablets",
    "price": 9.99,
    "rating": 4.5
  }
]
```

**Error Handling:**
- 400 Bad Request Error:
    - If product-category, or max-price, or min-rating and max-rating are not given, returns error with response: "Missing or invalid parameters. Please try again."
    - If min-rating and max-rating are given but max-rating is less than min-rating, returns error with response: "Missing or invalid parameters. Please try again."

- 500 Internal Server Error:
    - If an error occured on the server, returns an error with the response: "An error occured during the buy process. Please try again
    later."

## Get product information
**Request Format:** /detail/:item

**Request Type:** GET
**Parameters:** "item" (String)

**Returned Data Format**: JSON

**Description:** Retrieves information for a specific product

**Example Request:** /details/Bamboo Toothbrush

**Example Response:**

```
{
  "item": "Bamboo Toothbrush",
  "image": "bamboo-toothbrush",
  "description": "Eco-friendly toothbrush made from sustainably sourced bamboo.",
  "price": 4.99,
  "rating": 4.8,
  "stock": 150
}
```

**Error Handling:**
- 404 Not Found (plain text):
    - If the product is not in the database, returns an error with response: “Product not found.”

- 500 Internal Server Error (plain text):
    - If an error occured on the server, returns an error with the response: "An error occurred during information retrieval. Please try again later."

## Retrieve cart
**Request Format:** /cart

**Request Type:** POST
**Parameters:** "cart" (String)

**Returned Data Format**: JSON

**Description:** Retrieves the user's shopping cart

**Example Request:**

POST request: /cart
{
  "cart": "[\"Bamboo Toothbrush\",\"Bamboo Toothbrush\"]"
}

**Example Response:**

```
[
  {
    "item": "Bamboo Toothbrush",
    "price": 4.99,
    "rating": 4.8,
    "description": "Eco-friendly toothbrush made from sustainably sourced bamboo.",
    "image": "bamboo-toothbrush"
  },
  {
    "item": "Bamboo Toothbrush",
    "price": 4.99,
    "rating": 4.8,
    "description": "Eco-friendly toothbrush made from sustainably sourced bamboo.",
    "image": "bamboo-toothbrush"
  }
]

```

**Error Handling:**
- 400 Bad Request Error:
    - If the username or product does not exist within the database, returns an error with the response: "Invalid parameters. Please try again."
    - If the cart is empty, returns an error with the response: "Cart is empty."
- 500 Internal Server Error:
    - If an error occured on the server, returns an error with the response: "An error occured when attempting to retrieve the cart. Please try again later."

## Get best sellers
**Request Format:** /best-sellers

**Request Type:** GET

**Returned Data Format**: JSON

**Description:** Retrieves products with the highest rating / popularity

**Example Request:** /best-sellers/

**Example Response:**

```
[
  {
    "item": "Solar Powered Charger",
    "image": "solar-powered-charger",
    "price": 29.99,
    "rating": 5
  },
  {
    "item": "Stainless Steel Water Bottle",
    "image": "stainless-steel-water-bottle",
    "price": 19.99,
    "rating": 4.9
  },
  {
    "item": "Organic Cotton Bed Sheets",
    "image": "organic-cotton-bed-sheets",
    "price": 39.99,
    "rating": 4.9
  },
  {
    "item": "Bamboo Toothbrush",
    "image": "bamboo-toothbrush",
    "price": 4.99,
    "rating": 4.8
  },
  {
    "item": "Recycled Glass Water Bottle",
    "image": "recycled-glass-water-bottle",
    "price": 16.99,
    "rating": 4.8
  }
]
```

**Error Handling:**
- 400 Bad Request Error:
    - If the given column to sort by does not exist within the database, returns an error with the response: "Invalid or missing parameters. Try again."
- 500 Internal Server Error:
    - If an error occurred on the server, returns an error with the response: "An error occurred during account login. Please try again later."

## Upload product review
**Request Format:** /feedback

**Request Type:** POST
**Parameters:** "username" (String), "review" (String), "rating" (double), "item" (String)

**Returned Data Format**: Plain text

**Description:** Submits a user review to the current product page

**Example Request:**
POST request: /feedback
{
    "username": "user1",
    "item": "Bamboo Toothbrush",
    "review": "Works well! Works well! Works well!",
    "rating": 5.0
}

**Example Response:**

```
"Review successfully submitted."
```

**Error Handling:**
- 400 Bad Request Error (plain text):
    - If all parameters (item, username, rating, review) are missing, returns an error with the response: "Missing parameters. Please try again."
    - If the user's username or item is not in the store's system, returns an error with the response: "Invalid parameters. Please try again."

- 500 Internal Server Error (plain text):
    - If an error occured on the server, returns an error with the response: “An error occurred during retrieval. Please try again later.”

## Retrieve reviews of a product
**Request Format:** "/reviews/:item"

**Request Type:** GET

**Returned Data Format**: JSON

**Description:** Retrieves all reviews for a product

**Example Request:** /reviews/Bamboo Toothbrush

**Example Response:**

```
[
  {
    "username": "keith123",
    "review": "Nice",
    "rating": 5,
    "date": "2024-06-03 01:23:06"
  },
  {
    "username": "joselynd",
    "review": "10/10 would recommend",
    "rating": 5,
    "date": "2024-06-01 21:48:59"
  },
  {
    "username": "duckduckgoose",
    "review": "AMAZING",
    "rating": 5,
    "date": "2024-06-01 21:18:37"
  }
]
```

**Error Handling:**
- 400 Bad Request Error (plain text):
    - If the given item does not exist, returns an error with the response: "Missing parameters. Please try again."

- 500 Internal Server Error (plain text):
    - If an error occured on the server, returns an error with the response: “An error occurred during retrieval. Please try again later.”

## Transactions
**Request Format:** /transactions

**Request Type:** POST
**Parameters:** "username" (String)

**Returned Data Format**: JSON

**Description:** Returns the user's past transactions

**Example Request:**

POST request: /transactions
{
  "username": "keith123"
}

**Example Response:**

```
[
  {
    "transaction_code": 1577,
    "user_id": 1,
    "cart": "[\"Stainless Steel Water Bottle\",\"Stainless Steel Water Bottle\"]",
    "total": 39.98
  },
  {
    "transaction_code": 1591,
    "user_id": 1,
    "cart": "[\"Bamboo Toothbrush\",\"Bamboo Toothbrush\"]",
    "total": 9.98
  },
  {
    "transaction_code": 1952,
    "user_id": 1,
    "cart": "[\"Bamboo Toothbrush\",\"Reusable Shopping Bag\"]",
    "total": 14.98
  },
  {
    "transaction_code": 2187,
    "user_id": 1,
    "cart": "[\"Organic Cotton T-Shirt\"]",
    "total": 15.99
  },
  {
    "transaction_code": 2525,
    "user_id": 1,
    "cart": "[\"Bamboo Toothbrush\"]",
    "total": 4.99
  },
  {
    "transaction_code": 3353,
    "user_id": 1,
    "cart": "[\"Organic Cotton T-Shirt\",\"Organic Cotton T-Shirt\",\"Organic Cotton T-Shirt\"]",
    "total": 47.97
  }
]
```

**Error Handling:**
- 404 Not Found Error (plain text):
    - If user has no recorded transactions available, returns an error with response: "No previous transactions found."
    - If user does not exist or not provided, returns an error with response: "Invalid or missing parameters. Please try again."
- 500 Internal Server Error (plain text):
    - If an error occurred on the server, returns an error with the response: "An error occured during retrieval. Please try again later"

## New Transaction
**Request Format:** /account/new-transaction

**Request Type:** POST
**Parameters:** "username" (String), "item" (String), "quantity" (int)

**Returned Data Format**: Plain Text

**Description:** Adds the user’s newest transaction to their account records

**Example Request:**
POST request: /account/new-transaction
{
    "username": "user1",
    "item": "bag1",
    "quantity": 2
}

**Example Response:**

```
"Transaction successfully recorded."
```

**Error Handling:**
- 400 Bad Request Error (plain text):
    - If any of the parameters do not exist within the database, returns an error with response: "Incorrect parameters. Please try again."

- 500 Internal Server Error (plain text):
    - If an error occurred on the server, returns an error with the response: "An error occurred processing the transaction. Please try again later."

## Retrieve average rating for a product
**Request Format:** /details/rating/:item

**Request Type:** GET

**Returned Data Format**: Plain Text

**Description:** Retrieves the average rating for a product

**Example Request:** /details/rating/Bamboo Toothbrush

**Example Response:**
```
3.5
```

**Error Handling:**
- 404 Not Found Error (plain text):
    - If the item does not exist in the store, returns an error with response: "Item does not exist. Please try again."
- 500 Internal Server Error (plain text):
    - If an error occurred on the server, returns an error with the response: "An error occurred on the server. Try again later."