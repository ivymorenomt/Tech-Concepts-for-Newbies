### REST API Concepts and Examples

A REST API (Representational State Transfer API) allows two computers to communicate over HTTP, typically for data retrieval, creation, or manipulation. RESTful APIs follow a set of design principles for organizing resources and ensuring predictable interactions.

---

### **Core Concepts**

1. **What is an API?**
   - An API is a way for applications to interact with each other.
   - Example: Instead of manually viewing asteroid data on NASA's website, you can fetch the same data programmatically using their REST API in JSON format.

2. **REST Principles**:
   - Resources are identified using **URIs (Uniform Resource Identifiers)**.
   - HTTP request methods (verbs) are used to perform actions:
     - `GET`: Retrieve data.
     - `POST`: Create data.
     - `PATCH`: Update data.
     - `DELETE`: Remove data.

3. **Request Anatomy**:
   - **Start Line**: Specifies the HTTP verb and URI.
   - **Headers**: Metadata (e.g., format preferences, authentication).
   - **Body**: Contains the data payload (for `POST` or `PATCH` requests).

4. **Response Anatomy**:
   - **Status Code**:
     - `200`: Success.
     - `400`: Client error.
     - `500`: Server error.
   - **Headers**: Metadata about the response.
   - **Body**: Contains the data, often in JSON format.

5. **Statelessness**:
   - Each request is independent, requiring no memory of previous interactions.

---

### **Building a REST API with Express.js**

1. **Setup**:
   - Install dependencies:
     ```bash
     npm init -y
     npm install express
     ```
   - Create an `index.js` file and set up the server:
     ```javascript
     const express = require('express');
     const app = express();
     const PORT = 8080;

     app.listen(PORT, () => console.log(`Server running on http://localhost:${PORT}`));
     ```

2. **Defining Endpoints**:
   - Add a `GET` endpoint:
     ```javascript
     app.get('/t-shirt', (req, res) => {
         res.status(200).json({ message: "Hello, World!" });
     });
     ```

   - Add a `POST` endpoint with dynamic parameters:
     ```javascript
     app.use(express.json()); // Middleware for parsing JSON

     app.post('/t-shirt/:id', (req, res) => {
         const { id } = req.params;
         const { logo } = req.body;

         if (!logo) {
             return res.status(418).send({ error: "We need a logo!" });
         }

         res.status(200).send({ id, logo });
     });
     ```

3. **Testing the API**:
   - Use tools like **Postman** or **Insomnia** to send requests and verify responses.
   - Example:
     - Request: `POST http://localhost:8080/t-shirt/1`
       ```json
       {
         "logo": "Cool Logo"
       }
       ```
     - Response:
       ```json
       {
         "id": "1",
         "logo": "Cool Logo"
       }
       ```

---

### **Advanced Concepts**

1. **Middleware**:
   - Shared code executed before endpoint logic.
   - Example: Parsing JSON data.
     ```javascript
     app.use(express.json());
     ```

2. **Error Handling**:
   - Use custom error responses for invalid data.
   - Example:
     ```javascript
     if (!logo) {
         return res.status(418).send({ error: "We need a logo!" });
     }
     ```

3. **OpenAPI Specification**:
   - A standard for documenting APIs in a machine-readable format (e.g., YAML).
   - Tools like SwaggerHub can generate client-side or server-side boilerplate code automatically.

4. **Deployment**:
   - Use cloud tools like AWS API Gateway or Google Cloud to secure, monitor, and integrate your API with backend infrastructure.

---

### **Examples of API Requests**

1. **GET Request**:
   - Fetch data:
     ```javascript
     app.get('/data', (req, res) => {
         res.status(200).json({ data: "Example data" });
     });
     ```

2. **POST Request**:
   - Create a new resource:
     ```javascript
     app.post('/data', (req, res) => {
         const newData = req.body;
         res.status(201).json(newData);
     });
     ```

3. **Error Response**:
   - Invalid input:
     ```javascript
     app.post('/data', (req, res) => {
         if (!req.body.name) {
             res.status(400).json({ error: "Name is required" });
         }
     });
     ```

---

### Summary
- REST APIs enable communication between applications using HTTP.
- Express.js simplifies API development in Node.js.
- Key concepts include statelessness, HTTP methods, and structured responses.
- Advanced features like OpenAPI improve documentation and deployment. 

---
When implementing **pagination** in a web page using a REST API to handle 300 records, you typically need to:

1. **Divide the records into smaller chunks** (e.g., 10, 20, or 50 records per page).
2. **Use query parameters in the REST API** to specify which records to retrieve.

Here’s how to handle it:

---

### **Common Pagination Parameters**
1. **`page`**: The current page number.
2. **`limit`** (or `size`): The number of records to return per page.
3. **`offset`** (optional): The starting point for fetching records (alternative to `page`).

---

### **API Arguments**
#### Example:
- `GET /records?page=2&limit=10`

| Parameter | Description                                | Example Value |
|-----------|--------------------------------------------|---------------|
| `page`    | The page number to retrieve.               | `2`           |
| `limit`   | Number of records per page.                | `10`          |
| `offset`  | Starting index for records (alternative).  | `10`          |

---

### **Backend Implementation**

#### **With `page` and `limit`**:
The backend calculates the starting point based on `page` and `limit`:
```javascript
function getPaginatedRecords(req, res) {
    const page = parseInt(req.query.page) || 1; // Default to page 1
    const limit = parseInt(req.query.limit) || 10; // Default to 10 records per page
    const startIndex = (page - 1) * limit;
    const endIndex = page * limit;

    // Simulated database records
    const records = [
        /* Array of 300 records */
    ];

    const paginatedRecords = records.slice(startIndex, endIndex);

    res.json({
        page: page,
        limit: limit,
        totalRecords: records.length,
        totalPages: Math.ceil(records.length / limit),
        data: paginatedRecords,
    });
}
```

#### **With `offset`**:
The backend uses `offset` to fetch records directly:
```javascript
function getPaginatedRecords(req, res) {
    const offset = parseInt(req.query.offset) || 0; // Default to 0
    const limit = parseInt(req.query.limit) || 10;

    // Simulated database records
    const records = [
        /* Array of 300 records */
    ];

    const paginatedRecords = records.slice(offset, offset + limit);

    res.json({
        offset: offset,
        limit: limit,
        totalRecords: records.length,
        data: paginatedRecords,
    });
}
```

---

### **Frontend Implementation**
1. **Make API Requests**:
   - For example, fetch the second page with 10 records per page:
     ```javascript
     fetch('/api/records?page=2&limit=10')
         .then(response => response.json())
         .then(data => {
             console.log(data); // Handle and display paginated data
         });
     ```

2. **Handle Pagination Controls**:
   - Use buttons (Next, Previous) or page numbers to update the `page` or `offset` parameter in API requests.

---

### **API Response Format**
A good pagination response includes metadata for navigation:

```json
{
    "page": 2,
    "limit": 10,
    "totalRecords": 300,
    "totalPages": 30,
    "data": [
        { "id": 11, "name": "Record 11" },
        { "id": 12, "name": "Record 12" },
        // ...
    ]
}
```

---

### **Pagination Types**

1. **Offset-Based**:
   - Use `offset` and `limit`.
   - Good for databases that allow direct offset-based querying (e.g., SQL).

2. **Page-Based**:
   - Use `page` and `limit`.
   - Easier to understand for frontend developers.

3. **Cursor-Based (Advanced)**:
   - Use a `cursor` to fetch records.
   - Suitable for large datasets to avoid performance issues with `OFFSET`.

---

### **SQL Example**
Fetching paginated records directly from the database:
```sql
SELECT * FROM records
LIMIT 10 OFFSET 10; -- Fetch 10 records starting from the 11th record
```

---

### **Considerations**
1. **Large Datasets**:
   - For very large datasets, use cursor-based pagination to avoid performance bottlenecks.
2. **Sorting**:
   - Ensure consistent sorting to avoid duplicate or missing records between pages.
   - Example: `GET /records?page=1&limit=10&sort=name&order=asc`

3. **Error Handling**:
   - Validate `page` and `limit` to prevent invalid requests (e.g., negative or excessively large values).

---

Thanks to [REST API 101](https://www.youtube.com/watch?v=-MTSQjw5DrM&t=5s) for the visuals!
