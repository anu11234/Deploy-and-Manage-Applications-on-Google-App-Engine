# Deploy and Manage Applications on Google App Engine: Challenge Lab || **ARC112**

**Command:**

**Task 1:**
```bash
cd ~
git clone https://github.com/GoogleCloudPlatform/python-docs-samples.git
cd python-docs-samples/appengine/standard_python3/hello_world
```
**Task 2:**
```bash
cat << 'EOF' >> app.yaml

automatic_scaling:
  max_instances: 1
EOF

gcloud auth login
gcloud app create --region=<your-region>
gcloud app deploy --quiet
```

**Task 3:**
```bash
cat << 'EOF' > main.py
from flask import Flask

app = Flask(__name__)

@app.route("/")
def hello():
    return "Goodbye world!"

if __name__ == "__main__":
    app.run(host="127.0.0.1", port=8080, debug=True)
EOF
```
```bash
gcloud app deploy --quiet
