[Stash](#stash)

<hr />

#### [Stash](#stash)

```bash
ls -l
```

```bash
total 24
-rw-r--r--  1 gbhujan  staff    14B Mar 18 14:24 README.md
-rw-r--r--  1 gbhujan  staff   1.0K Mar 18 14:24 test.py
-rw-r--r--  1 gbhujan  staff   791B Mar 18 14:24 testV2.py
```

```bash
git status
```

```bash
On branch master
Your branch is up to date with 'origin/master'.

nothing to commit, working tree clean
```

`testV2.py`

```python
import ssl
import json
import requests
from flask import Flask, request
from flask import jsonify
from flask import Response
from flask import make_response
import logging
import logging.handlers
import pprint
import syslog

syslog.syslog("this is a test message")

app = Flask(__name__)


@app.route('/')
def hello():
    return 'Hello, World!'


@app.route('/testV2', methods=['GET'])
def test():
    data = {"x": 123, "y": 456}
    response = jsonify(data)
    response.headers.add("Access-Control-Allow-Origin", "*")
    response.headers.add('Access-Control-Allow-Credentials', 'true')
    response.headers.add('Access-Control-Allow-Methods', '*')
    return response


if __name__ == "__main__":
    syslog.syslog("starting api...")
    app.run(debug=True, port=9000, host='127.0.0.1')
```

`test.py`

```python
import ssl
import json
import requests
from flask import Flask, request
from flask import jsonify
from flask import Response
from flask import make_response
import logging
import logging.handlers
import pprint
import syslog

syslog.syslog("this is a test message")

app = Flask(__name__)


@app.route('/')
def hello():
    return 'Hello, World!'


@app.route("/api/v1/getData", methods=['GET'])
def get_data():
    if request.method == "GET":
        syslog.syslog("request : GET : /api/v1/getData")
        response = make_response()
        data = {"a": 123, "b": "hello world", "c": [1, 2, 3, 4, 5, 6], "d": {"a1": 11, "a2": 22}}
        response = jsonify(data)
        response.headers.add("Access-Control-Allow-Origin", "*")
        response.headers.add('Access-Control-Allow-Credentials', 'true')
        response.headers.add('Access-Control-Allow-Methods', '*')
        return response


if __name__ == "__main__":
    syslog.syslog("starting api...")
    app.run(debug=True, port=8000, host='127.0.0.1')
```

Now make changes to both the files : `testV2.py` and `test.py`

`testV2.py`

```python
import ssl
import json
import requests
from flask import Flask, request
from flask import jsonify
from flask import Response
from flask import make_response
import logging
import logging.handlers
import pprint
import syslog

syslog.syslog("this is a test message")

app = Flask(__name__)


@app.route('/')
def hello():
    return 'Hello, World!'


@app.route('/testV2', methods=['GET'])
def test():
    data = {"x": 123, "y": 456}
    response = jsonify(data)
    response.headers.add("Access-Control-Allow-Origin", "*")
    response.headers.add('Access-Control-Allow-Credentials', 'true')
    response.headers.add('Access-Control-Allow-Methods', '*')
    return response


@app.route('/testV3', methods=['GET'])
def testNew():
    data = {"x": 123, "y": 456}
    response = jsonify(data)
    response.headers.add("Access-Control-Allow-Origin", "*")
    response.headers.add('Access-Control-Allow-Credentials', 'true')
    response.headers.add('Access-Control-Allow-Methods', '*')
    return response

if __name__ == "__main__":
    syslog.syslog("starting api...")
    app.run(debug=True, port=9000, host='127.0.0.1')

```

`test.py`

```python
import ssl
import json
import requests
from flask import Flask, request
from flask import jsonify
from flask import Response
from flask import make_response
import logging
import logging.handlers
import pprint
import syslog

syslog.syslog("this is a test message")

app = Flask(__name__)


@app.route('/')
def hello():
    return 'Hello, World!'


@app.route('/test')
def test():
    return 'Hello, World : Testing'


@app.route("/api/v1/getData", methods=['GET'])
def get_data():
    if request.method == "GET":
        syslog.syslog("request : GET : /api/v1/getData")
        response = make_response()
        data = {"a": 123, "b": "hello world", "c": [1, 2, 3, 4, 5, 6], "d": {"a1": 11, "a2": 22}}
        response = jsonify(data)
        response.headers.add("Access-Control-Allow-Origin", "*")
        response.headers.add('Access-Control-Allow-Credentials', 'true')
        response.headers.add('Access-Control-Allow-Methods', '*')
        return response


if __name__ == "__main__":
    syslog.syslog("starting api...")
    app.run(debug=True, port=8000, host='127.0.0.1')
```

```bash
git status
```

```bash
On branch master
Your branch is up to date with 'origin/master'.

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
    modified:   test.py
    modified:   testV2.py

no changes added to commit (use "git add" and/or "git commit -a")
```

```bash
git diff
```

```bash
diff --git a/test.py b/test.py
index 0b0d180..4804925 100644
--- a/test.py
+++ b/test.py
@@ -20,6 +20,11 @@ def hello():
     return 'Hello, World!'


+@app.route('/test')
+def test():
+    return 'Hello, World : Testing'
+
+
 @app.route("/api/v1/getData", methods=['GET'])
 def get_data():
     if request.method == "GET":
diff --git a/testV2.py b/testV2.py
index 7f32cb5..4ac580e 100644
--- a/testV2.py
+++ b/testV2.py
@@ -30,6 +30,15 @@ def test():
     return response


+@app.route('/testV3', methods=['GET'])
+def testNew():
+    data = {"x": 123, "y": 456}
+    response = jsonify(data)
+    response.headers.add("Access-Control-Allow-Origin", "*")
+    response.headers.add('Access-Control-Allow-Credentials', 'true')
+    response.headers.add('Access-Control-Allow-Methods', '*')
+    return response
+
 if __name__ == "__main__":
     syslog.syslog("starting api...")
     app.run(debug=True, port=9000, host='127.0.0.1')
```

```bash
git stash save "was working on a feature"
```

```bash
# git status
On branch master
Your branch is up to date with 'origin/master'.

nothing to commit, working tree clean

# git diff
```

```bash
git stash list

stash@{0}: On master: was working on a feature
```

#### After `git stash save "message"` , files will be back to its original state.

Now to get back the changes, you must use `git stash apply <STASH>`

In the above example, our stash was `stash@{0}`

```bash
git stash apply stash@{0}
```

Now we will get back the changes which we originally made.

```bash
# git stash apply stash@{0}
On branch master
Your branch is up to date with 'origin/master'.

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
    modified:   test.py
    modified:   testV2.py

no changes added to commit (use "git add" and/or "git commit -a")
```

```bash
git diff
```

```bash
diff --git a/test.py b/test.py
index 0b0d180..4804925 100644
--- a/test.py
+++ b/test.py
@@ -20,6 +20,11 @@ def hello():
     return 'Hello, World!'


+@app.route('/test')
+def test():
+    return 'Hello, World : Testing'
+
+
 @app.route("/api/v1/getData", methods=['GET'])
 def get_data():
     if request.method == "GET":
diff --git a/testV2.py b/testV2.py
index 7f32cb5..4ac580e 100644
--- a/testV2.py
+++ b/testV2.py
@@ -30,6 +30,15 @@ def test():
     return response


+@app.route('/testV3', methods=['GET'])
+def testNew():
+    data = {"x": 123, "y": 456}
+    response = jsonify(data)
+    response.headers.add("Access-Control-Allow-Origin", "*")
+    response.headers.add('Access-Control-Allow-Credentials', 'true')
+    response.headers.add('Access-Control-Allow-Methods', '*')
+    return response
+
 if __name__ == "__main__":
     syslog.syslog("starting api...")
     app.run(debug=True, port=9000, host='127.0.0.1')
```