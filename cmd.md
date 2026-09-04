# Deploy and Manage Applications on Google App Engine: Challenge Lab || **ARC112**

**Command:**

```bash
cat << 'EOF' > solution.sh
#!/bin/bash
set -e

echo "=== Step 1: Identifying & Cloning Repository ==="
if [ ! -d "$HOME/python-docs-samples" ] && [ ! -d "$HOME/php-docs-samples" ] && [ ! -d "$HOME/golang-samples" ]; then
    git clone https://github.com/GoogleCloudPlatform/python-docs-samples.git ~/python-docs-samples 2>/dev/null || true
    git clone https://github.com/GoogleCloudPlatform/php-docs-samples.git ~/php-docs-samples 2>/dev/null || true
    git clone https://github.com/GoogleCloudPlatform/golang-samples.git ~/golang-samples 2>/dev/null || true
fi

TARGET_DIR=$(find ~ -type d \( -path "*/appengine/standard_python3/hello_world" -o -path "*/appengine/standard/helloworld" -o -path "*/appengine/go11x/helloworld" \) | head -n 1)

if [ -z "$TARGET_DIR" ]; then
    echo "Error: Target directory could not be found."
    exit 1
fi

echo "Navigating to: $TARGET_DIR"
cd "$TARGET_DIR"

echo "=== Step 2: Configuring app.yaml & Initial Deployment ==="
if ! grep -q "max_instances" app.yaml 2>/dev/null; then
cat << 'YML' >> app.yaml

automatic_scaling:
  max_instances: 1
YML
fi

if [ -f "app.yaml" ]; then
    sed -i 's/php81/php83/g' app.yaml 2>/dev/null || true
fi

# Ensure account/project authorization inside the VM
gcloud config set project $(gcloud config get-value project 2>/dev/null) 2>/dev/null || true
gcloud app deploy --quiet

echo "=== Step 3: Updating Greeting Message & Redeploying ==="
MAIN_FILE=$(ls main.py index.php helloworld.go 2>/dev/null | head -n 1)

if [ -n "$MAIN_FILE" ]; then
    echo "Updating greeting inside $MAIN_FILE..."
    sed -i -E 's/(Hello, World!|Hello World!|Hello World)/Welcome to this world!/g' "$MAIN_FILE"
fi

gcloud app deploy --quiet

echo "=== Deployment Complete! Click 'Check my progress' on all tasks. ==="
EOF

bash solution.sh
