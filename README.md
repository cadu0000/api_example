
# Mango API

Sample API for managing **products** and **carts**, integrating with the [FakeStoreAPI](https://fakestoreapi.com/).

---

## How to Run

### 1. **Clone the project**
```bash
$ git clone [https://github.com/cadu0000/teste_crp_mango.git](https://github.com/cadu0000/teste_crp_mango.git)
````

### 2\. **Build and run with Docker**

```bash
$docker build -t fake-store-api .$ docker run -p 3333:3333 fake-store-api
```

> The API will be available at: [http://localhost:3333](https://www.google.com/search?q=http://localhost:3333)

-----

## Main Endpoints

### Products

  - **GET `/products`**
      - Paginated list of products
      - Parameters: `limit`, `offset`, `sort`
      - Example: `/products?limit=5&offset=0`
      - HTTP: 200

### Users

  - **GET `/users`**
      - Paginated list of users
      - Parameters: `limit`, `offset`, `sort`
      - Example: `/users?limit=5&offset=0`
      - HTTP: 200

### Carts

  - **POST `/carts`**

      - Creates a cart for a user.
      - Payload: (Note that the official documentation includes an `id`, but it doesn't make sense to send it, as it will only be generated after the POST request)
        ```json
        {
          "userId": integer,
          "products": Product[]
        }
        ```
      - HTTP: 201 Created

  - **POST `/carts/any-first-products`**

      - Creates a cart with the first N products. (This is the route used for the challenge solution, sending the request with the `limit=3` parameter, just like the route's example request).
      - Payload
        ```json
        {
          "userId": integer,
          "products": Product[]
        }
        ```
      - Parameters: `limit`
      - Example: `/carts/any-first-products?limit=3`
      - HTTP: 201 Created

-----

## Internal Flow (Example: POST `/carts`)

1.  **Receives the payload** with `userId` and `products`.
2.  **Fetches the products** from FakeStoreAPI.
3.  **Selects the sent products** (or the first N products, if using `/any-first-products`).
4.  **Builds the payload** in the format expected by FakeStoreAPI.
5.  **Sends the payload** to `https://fakestoreapi.com/carts`.
6.  **Receives the response** from FakeStoreAPI.
7.  **Fetches full product data** to enrich the response.
8.  **Returns the created cart** with all the product data included.

-----

## Assessment Solution:

### Create a cart with the first 3 products from [FakeStoreAPI](https://fakestoreapi.com/products)

```bash
curl -X POST "http://localhost:3333/carts/any-first-products?limit=3"
```

-----

## Technologies

  - Node.js
  - TypeScript
  - Express
  - Docker

