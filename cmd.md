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
gcloud app create --region=<region>
gcloud app deploy --quiet
```

**Task 3:**
```bash
sed -i 's/Hello World!/Welcome to this world!/g' main.py
gcloud app deploy --quiet
