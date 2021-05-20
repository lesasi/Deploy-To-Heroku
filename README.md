# Deploying MERN Application to Heroku

Small guide to deploy a project using React client and Node.js server. Summary of [this article](https://coursework.vschool.io/deploying-mern-app-to-heroku/)

## Git repo
1. In a server and client folder, take all contents of server folder, and paste them one folder up(in root dir), then delete the empty server folder.
2. Make sure the server.js file (file with app.use, routers etc.) is in the root dir.
3. In the development environment, you've likely been running your React app on port 3000 (by running `npm start` or `yarn start`) and your server on whatever port you chose by running `node server.js`. However, we're going to set up Express to do double duty - it will handle API calls like before, but it will also serve up your React app when someone first visits your main site. This just takes a few additions to the main `server.js` file:
	
	
>
	    // ... other imports  
        const path = require("path")
        
        // ... other app.use middleware  		
        app.use(express.static(path.join(__dirname, "client", "build")))
        
        // ... // Right before your app.listen(), add this:  
        app.get("*", (req, res) => {
            res.sendFile(path.join(__dirname, "client", "build", "index.html")); 
        });
        
        app.listen(...)

4.  Add a proxy to the client's  `package.json` - all API calls from the client will be sent to this proxy.
> "proxy": "http://localhost:8000" 
5. Add scripts, as well as node version in **server** package.json to facilitate heroku building.
>
    "scripts": {
        "start": "node server.js",
        "heroku-postbuild": "cd client && npm install --only=dev && npm install && npm run build"
    },
    "engines": {
	    "node": "12.22.1"
    }

7. Deploy all of this to GitHub
## Heroku 

1. Make Heroku account, login into heroku using 'heroku login'.
2. Create new application.
3. Set environment variables(in Settings).
4. Go to deploy tab, in 'Deployment Method', click 'GitHub'. 
5. Connect your Git Repo to the Heroku app. Put the name of your git repo in the "Search for repository to connect to", then click "Connect".
6. Teach Heroku which branch of your Git Repo to deploy from
7. Click **Deploy Branch** button to deploy
8. To see console logs from application, go to **More > 'View Logs'** (on the top right).
