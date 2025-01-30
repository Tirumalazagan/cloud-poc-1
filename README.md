#!/bin/bash

# Set your GitHub details
GITHUB_USER="YOUR_GITHUB_USERNAME"
REPO_NAME="YOUR_REPO_NAME"
ACCESS_TOKEN="YOUR_PERSONAL_ACCESS_TOKEN"

# Create a new GitHub repository using API
curl -H "Authorization: token $ACCESS_TOKEN" \
     -H "Accept: application/vnd.github.v3+json" \
     https://api.github.com/user/repos \
     -d "{\"name\":\"$REPO_NAME\"}"

# Clone the repository locally
git clone https://github.com/$GITHUB_USER/$REPO_NAME.git
cd $REPO_NAME

# Initialize a basic README.md
echo "# $REPO_NAME" > README.md
echo "This repository contains instructions to set up a local server." >> README.md

# Create a simple Express.js server
mkdir server
cd server
echo "const express = require('express');" > server.js
echo "const app = express();" >> server.js
echo "const PORT = 3000;" >> server.js
echo "app.get('/', (req, res) => res.send('Local server is running!'));" >> server.js
echo "app.listen(PORT, () => console.log(\Server running at http://localhost:\${PORT}\));" >> server.js

# Create a package.json file
echo '{
  "name": "local-server",
  "version": "1.0.0",
  "description": "A simple Node.js local server setup",
  "main": "server.js",
  "dependencies": {
    "express": "^4.17.1"
  },
  "scripts": {
    "start": "node server.js"
  },
  "author": "YOUR_GITHUB_USERNAME",
  "license": "MIT"
}' > package.json

# Go back to the main repo folder
cd ..

# Initialize Git and push
git init
git add .
git commit -m "Initial commit: Local server setup"
git branch -M main
git remote add origin https://github.com/$GITHUB_USER/$REPO_NAME.git
git push -u origin main

echo "Repository setup complete! Check it at: https://github.com/$GITHUB_USER/$REPO_NAME"
