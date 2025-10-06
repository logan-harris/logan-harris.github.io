Establishing Internet Presence
  1. Created Oracle Cloud VM
    -Oracle Linux 9, Always Free tier, public subnet enabled, shape configuration VM.Standard.E2.1.Micro
    -Generated SSH key pair
  2. Connected to VM via SSH
    -ssh -i [PATH_TO_YOUR_PRIVATE_KEY] opc@[YOUR_VM_PUBLIC_IP]
    -verified connection and fixed key permissions if needed.
  3. Updated system and increased swap
    -sudo dnf update -y
    -sudo fallocate -l 8G /swapfile
    -sudo chmod 600 /swapfile
    -sudo mkswap /swapfile
    -sudo swapon /swapfile
    -swapon --show
    -free -h
  4. Installed Node.js and npm
    -curl -fsSL https://rpm.nodesource.com/setup_18.x | sudo bash -
    -sudo dnf install -y nodejs --setopt=tsflags=nodocs
    -node -v
    -npm -v
  5. Initialized Node.js project
    -npm init -y
    -npm install express
  6. Configured Node.js Express server
    -index.js:
      const express = require('express');
      const path = require('path');
      const app = express();
      const PORT = process.env.PORT || 3000;
      
      app.use(express.json());
      app.use(express.urlencoded({ extended: true }));
      
      app.get('/', (req, res) => {
        res.sendFile(path.join(__dirname, 'index.html'));
      });
      
      app.all('/echo', (req, res) => {
        res.json({
          method: req.method,
          headers: req.headers,
          query: req.query,
          body: req.body
        });
      });
      
      app.listen(PORT, '0.0.0.0', () => {
        console.log(`Server is running on http://0.0.0.0:${PORT}`);
      });
    -index.html:
      <!DOCTYPE html>
        <html lang="en">
          <head>
            <meta charset="UTF-8">
            <title>My Home Page</title>
          </head>
          <body>
            <h1>Welcome to My Home Page</h1>
            <p>This page is under construction.</p>
          </body>
      </html>
   7. Opened firewall on port 3000
    -Navigate to Oracle website and enter into your instance
    -Security List Ingress rule under default instance VCN:
      -Source Type: CIDR
      -Source CIDR: 0.0.0.0/0
      -IP Protocol: TCP
      -Destination Port Range: 3000
      -Description: Allow Node.js HTTP traffic
   9. Tested server in VM instance
    -curl http://localhost:3000/
    -curl http://localhost:3000/echo
   10. GitHub Pages Redirect
    -Created new Repository: [YOUR_GITHUB_USERNAME].github.io
    -Created new index.html with meta refresh:
      -<meta http-equiv="refresh" content="0; url=http://[YOUR_VM_PUBLIC_IP]:3000">
  11. Testing the Webpage and Server
    -Node.js server: http://[YOUR_VM_PUBLIC_IP]:3000 in the browser
    -GitHub Pages redirect: https://[YOUR_GITHUB_USERNAME].github.io in the browser
    

