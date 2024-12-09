Here's a simple Python server using Flask, a lightweight and easy-to-use web framework. This server can serve as the foundation for your website. It includes routing for the home page and an additional endpoint as an example.



### Code: Basic Flask Server



```python

from flask import Flask, render_template, request, jsonify



app = Flask(__name__)



# Home route

@app.route('/')

def home():

    return """

    <html>

        <head>

            <title>My Website</title>

        </head>

        <body>

            <h1>Welcome to My Website</h1>

            <p>This is a simple Python server.</p>

        </body>

    </html>

    """



# Example API route

@app.route('/api/data', methods=['GET'])

def api_data():

    # Return some example JSON data

    return jsonify({

        "message": "Hello, this is your API!",

        "status": "success"

    })



# Run the server

if __name__ == '__main__':

    app.run(debug=True)

```



### Steps to Run the Server



1. **Install Flask**:

   Ensure you have Flask installed. You can install it using pip:

   ```bash

   pip install flask

   ```



2. **Run the Server**:

   Save the code in a file, for example, `server.py`, and run it:

   ```bash

   python server.py

   ```



3. **Access the Server**:

   Open your browser and go to:

   - [http://127.0.0.1:5000/](http://127.0.0.1:5000/) for the homepage.

   - [http://127.0.0.1:5000/api/data](http://127.0.0.1:5000/api/data) for the API response.



### Extending the Server

- Add more routes for different pages using `@app.route('/route')`.

- Use the `render_template` function to serve HTML files by placing them in a `templates` folder.



Would you like to expand on this server, for instance, by adding form handling, database integration, or dynamic templates?
