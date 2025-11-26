# Python-Request-Cyle
FULL SIMPLE EXPLANATION OF THE CODE
import os

from flask import Flask, request, current_app, g, make_response

✔ What this does

os → allows you to work with your computer’s operating system (e.g., get file paths).

You import several tools from Flask:

Flask → creates your web app.

request → reads details about the incoming request (headers, URL, browser info).

current_app → gives details about the running Flask app.

g → a temporary storage for each request (used to hold data that lasts only during a request).

make_response → manually create a custom response.

🌐 Create the Flask App
app = Flask(__name__)


This line initializes your application.

__name__ tells Flask where your project files are located.

🟦 Before Request Function
@app.before_request
def app_path():
    g.path = os.path.abspath(os.getcwd())

✔ What it does

@app.before_request: This runs before every request automatically.

os.getcwd() → gets the current working directory (where your project folder is).

os.path.abspath() → turns it into a full, absolute path.

g.path → stores this path in Flask’s g object for temporary use.

💡 Why use g?
Because g allows data to be shared inside the current request only.

For example:
g.path = "/home/elijah/myproject"

🏠 Home Route (‘/’)
@app.route('/')
def index():
    host = request.headers.get('Host')
    appname = current_app.name
    response_body = f'''
        <h1>The host for this page is {host}</h1>
        <h2>The name of this application is {appname}</h2>
        <h3>The path of this application on the user's device is {g.path}</h3>
    '''

✔ What these lines do
1️⃣ Get the host
host = request.headers.get('Host')


Reads the "Host" header from the browser request.

Example: "localhost:5555" or "127.0.0.1:5555"

2️⃣ Get the name of the app
appname = current_app.name


Returns the app name.

Usually "__main__" when running directly.

3️⃣ Prepare a HTML response
response_body = f'''
    <h1>The host for this page is {host}</h1>
    <h2>The name of this application is {appname}</h2>
    <h3>The path of this application on the user's device is {g.path}</h3>
'''


This builds an HTML page showing:

✔ Host
✔ App name
✔ Full path of the project folder

📤 Create a Manual Response
status_code = 200
headers = {}

return make_response(response_body, status_code, headers)

✔ Breakdown

status_code = 200 → Everything OK.

headers = {} → No extra headers.

make_response() → Builds a custom response using:

The HTML body

The status code

Headers (none in this case)

This sends the final page back to the browser.

🚀 Run the App
if __name__ == '__main__':
    app.run(port=5555, debug=True)

✔ What it means

The app runs only if this file is executed directly.

Runs on port 5555 → http://localhost:5555

debug=True → shows error messages and auto-restarts on changes.

🎯 IN SUMMARY
This app does the following:

Before every request, it saves your computer’s project folder path in g.path.

When you open the homepage (/), it shows:

The host (localhost:5555)

The application name

The full path of the project on your computer

It manually creates and returns an HTML response.