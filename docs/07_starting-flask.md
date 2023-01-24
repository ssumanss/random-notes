---
layout: default
title: Quickstart Flask
nav_order: 7
---

#  Quickstart Flask

It is always advisable to create a new virtual environment to 
start building new type of library files. 

Boilerplate of a flask app is 

```python
# importing the flask library
from flask import Flask

# creating a flask app instance 
app = Flask(__name__)

# setting url route of homepage
@app.route("/")
def hello_world():
    return "<p>Hello, World!</p>"

# to run the program from `Run` button we have to add
if __name__ == '__main__':
    app.run(debug=True)
```

## Using html template files in flask

Flask crate the backend of the app. It is good idea to keep
the html files separately from the app logic. 

The `html` files are served from the `template` folder from the 
flask app and static files like images and javascript stored in
the `static` folder. 

To use the `html` files we have use the `render_html` function. 
```python
from flask import Flask
from flask import render_template

app = Flask(__name__)


@app.route("/")
def hello_world():
    return render_template("index.html")


if __name__ == '__main__':
    app.run(debug=True)
```

Note that the link of any static files will be 
`static/<filename>`. 
