# flask + vite + svelte

We'll be using Vite to build a Svelte bundle, and then serving it with Flask
so we can still call into Python for API calls.

First, make sure Flask is installed:

```
$ pip install flask
```

Then, we created the svelte bundle inside of the root directory with npm's
create tool, based on the `vite@latest` package with the Svelte template:

```
$ npm create vite@latest frontend -- --template svelte
$ cd frontend
$ npm install
```

Then, we run Vite to compile the Svelte app into a static bundle in the `frontend/dist` directory (telling Vite
to watch for changes):

```
$ npx vite build --watch
```

Back in the root directory, we direct Flask to serve static files from th e `frontend/dist` directory:

```python
app = Flask(
    __name__,
    static_folder='frontend/dist',
    static_url_path="/"
)
```

Additionally, we add a catch-all route so that we can send all requests that
don't hit an API endpoint in Flask to Svelte so we can handle it there:

```python
@app.route('/', defaults={'path': ''})
@app.route('/<path:path>')
def catch_all(path):
    return app.send_static_file("index.html")
```

Now, you can run flask just with:

```
$ flask run
```

