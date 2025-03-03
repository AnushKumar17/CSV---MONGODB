
## 1. Project Overview
This project provides a REST API for managing data. It allows users to upload product data from a CSV file, retrieve all products, filter products based on various criteria, and group products by option code. The API is built using Node.js, Express, and MongoDB.
**Key Features:**
*   **CSV Data Upload:** Upload product data from a CSV file to the database.
*   **Product Retrieval:** Retrieve all products from the database.
*   **Product Filtering:** Filter products based on style code, option code, MRP, Brick, and Sleeve.
*   **Product Grouping:** Group products by option code.
*   **Unique EAN Code Generation:** Automatically generates a unique EAN code for each product if not provided.
**Requirements:**
*   Node.js (v14 or higher)
*   MongoDB
## 2. Getting Started
### Installation
1.  **Clone the repository:**
    ```bash
    git clone <repository_url>
    cd backend-assignment
    
2.  **Install dependencies:**
    ```bash
    npm install
    
### Configuration
1.  **Create a `.env` file** in the root directory.
2.  **Add the following environment variables:**
        MONGO_URI=<your_mongodb_connection_string>
        PORT=<port_number> (e.g., 3000)
    
    Replace `<your_mongodb_connection_string>` with your MongoDB connection string and `<port_number>` with the desired port number.
### Running the Application
1.  **Start the server:**
    ```bash
    npm start
    
    Or, for automatic restarts on file changes:
    ```bash
    npm install -g nodemon
    nodemon app.js
    
## 3. Usage
### CSV Data Upload
1.  Prepare a CSV file with the following headers: `style_code`, `option_code`, `MRP`, `Brick`, `Sleeve`.  An example `sample_data.csv` is provided in the `public/uploads/` directory.
2.  Send a `POST` request to the `/dataupload` endpoint with the CSV file attached using the `file` field name.
### API Endpoints
#### 3.1. Upload Data (`/dataupload`)
*   **Method:** `POST`
*   **Description:** Uploads product data from a CSV file.
*   **Request Body:** `multipart/form-data` with a file field named `file`.
*   **Response:**
    *   **Success (200 OK):**
        ```json
        {
            "success": true,
            "message": "Successful data exported."
        }
        
    *   **Error (400 Bad Request):**
        ```json
        {
            "success": false,
            "message": "<error_message>"
        }
        
    **Example Request (using `curl`):**
    ```bash
    curl -X POST -F "file=@public/uploads/sample_data.csv" http://localhost:3000/dataupload
    
#### 3.2. Get All Products (`/get-all-products`)
*   **Method:** `GET`
*   **Description:** Retrieves all products from the database.
*   **Request Parameters:** None
*   **Response:**
    *   **Success (200 OK):**
        ```json
        {
            "success": true,
            "data": [
                {
                    "_id": "<product_id>",
                    "style_code": "SC001",
                    "option_code": "OPT001",
                    "EAN_code": "...",
                    "MRP": 999,
                    "Brick": "Shirt",
                    "Sleeve": "Full Sleeve",
                    "__v": 0
                },
                // ... more products
            ]
        }
        
    *   **Error (500 Internal Server Error):**
        ```json
        {
            "success": false,
            "message": "<error_message>"
        }
        
    **Example Request:**
    ```bash
    curl http://localhost:3000/get-all-products
    
#### 3.3. Filter Products (`/products`)
*   **Method:** `GET`
*   **Description:** Retrieves products based on specified filters.
*   **Request Parameters:**
    *   `style_code` (optional): String
    *   `option_code` (optional): String
    *   `MRP` (optional): Number
    *   `Brick` (optional): String (Shirt, T-shirt, Jeans, Trouser)
    *   `Sleeve` (optional): String (Full Sleeve, Half Sleeve, Sleeveless)
*   **Response:**
    *   **Success (200 OK):**
        ```json
        {
            "success": true,
            "data": [
                {
                    "_id": "<product_id>",
                    "style_code": "SC001",
                    "option_code": "OPT001",
                    "EAN_code": "...",
                    "MRP": 999,
                    "Brick": "Shirt",
                    "Sleeve": "Full Sleeve",
                    "__v": 0
                },
                // ... more products matching the filter
            ]
        }
        
    *   **Error (500 Internal Server Error):**
        ```json
        {
            "success": false,
            "message": "<error_message>"
        }
        
    **Example Request:**
    ```bash
    curl "http://localhost:3000/products?style_code=SC001&Brick=Shirt"
    
#### 3.4. Grouped Products (`/grouped-products`)
*   **Method:** `GET`
*   **Description:** Retrieves products grouped by `option_code`, optionally filtered by other parameters.
*   **Request Parameters:**
    *   `style_code` (optional): String
    *   `option_code` (optional): String
    *   `MRP` (optional): Number
    *   `Brick` (optional): String (Shirt, T-shirt, Jeans, Trouser)
    *   `Sleeve` (optional): String (Full Sleeve, Half Sleeve, Sleeveless)
*   **Response:**
    *   **Success (200 OK):**
        ```json
        {
            "success": true,
            "data": {
                "OPT001": [
                    {
                        "_id": "<product_id>",
                        "style_code": "SC001",
                        "option_code": "OPT001",
                        "EAN_code": "...",
                        "MRP": 999,
                        "Brick": "Shirt",
                        "Sleeve": "Full Sleeve",
                        "__v": 0
                    },
                    // ... more products with option_code OPT001
                ],
                "OPT002": [
                    // ... products with option_code OPT002
                ]
            }
        }
        
    *   **Error (500 Internal Server Error):**
        ```json
        {
            "success": false,
            "message": "<error_message>"
        }
        
    **Example Request:**
    ```bash
    curl "http://localhost:3000/grouped-products?Brick=T-shirt"
    
## 4. Code Structure
backend-assignment/
├── models/
│   └── product-model.js       # Mongoose schema for Product
├── public/
│   └── uploads/
│       └── sample_data.csv    # Example CSV data file
├── .gitignore                 # Specifies intentionally untracked files that Git should ignore
├── app.js                     # Main application file
├── package.json               # Project dependencies and scripts
**Key Components:**
*   **`app.js`:**  The main application file.  It handles routing, middleware configuration, and database connection.
*   **`models/product-model.js`:** Defines the Mongoose schema for the `Product` model, including data types, validation rules, and default values.
*   **`public/uploads/sample_data.csv`:** An example CSV file that can be used to upload product data.
*   **`.gitignore`:** Specifies files and directories that Git should ignore (e.g., `node_modules`, `.env`).
## 5. API Documentation (Detailed)
See section 3 for detailed API endpoint documentation, including request parameters, response formats, and examples.

