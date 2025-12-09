### Hosting services
+ Frontend: Vercel
+ Backend: Render
+ Database: MongoDB Atlas

### System design
Clerk authentication + MongoDB -> NodeJS -> React

<u>Webhooks used to sync user data from Clerk to MongoDB</u>

#### MongoDB will store:
+ Rental car information, i.e., which cars are available and how many
+ Users current rental & rental history - tied to Clerk user account
+ List of all current active rentals - tied to car information
+ Audit logs

* Technology Stack: A popular choice is the MERN stack:
       * MongoDB: The database to store your car data.
       * Express.js: A Node.js framework to build your API.
       * React: Your existing frontend.
       * Node.js: The runtime environment for your backend.

   * Database Model: In MongoDB, you would define a Car schema with fields like:
       * name (String)
       * description (String)
       * price (Number)
       * image (String - URL to the image)

   * API Endpoints (Routes): You would create several API endpoints to handle CRUD (Create, Read, Update, Delete) operations for your
     cars.
       * GET /api/cars: Fetches a list of all cars from the database.
       * POST /api/cars: Adds a new car to the database. This would be a protected route, only accessible by admins.
       * PUT /api/cars/:id: Updates the details of a specific car. Also a protected route.
       * DELETE /api/cars/:id: Removes a car from the database. Also a protected route.

   * Authentication & Authorization: To protect the admin routes, you would implement:
       * Authentication: A login system for admins. This typically involves a username and password, and upon successful login, the
         server would issue a token (like a JSON Web Token or JWT).
       * Authorization: For the protected routes (POST, PUT, DELETE), the server would check for a valid admin token in the request
         headers. If the token is valid and the user is an admin, the request is processed. Otherwise, it's rejected.

  2. Frontend (Admin Panel)

  You would create a new section of your website, accessible only to logged-in admins.

   * Admin Page: A new page, for example, at /admin, that is protected and only accessible after an admin logs in.

   * Functionality:
       * View Cars: The admin page would fetch all cars from your API (GET /api/cars) and display them in a list or table.
       * Add Car: A form that allows admins to enter the details of a new car (name, description, price, image URL) and submit it to the
         backend (POST /api/cars).
       * Edit Car: Each car in the list would have an "Edit" button that opens a form pre-filled with the car's current details.
         Submitting this form would update the car in the database (PUT /api/cars/:id).
       * Delete Car: Each car would also have a "Delete" button that, when clicked, would send a request to the backend to remove that
         car from the database (DELETE /api/cars/:id).

