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

Thanks to [REST API 101](https://www.youtube.com/watch?v=-MTSQjw5DrM&t=5s) for the visuals!
