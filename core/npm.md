# NPM 

NPM is
 - official online registry package manager for Node module/package
 - runs through the command line and manages dependencies for an application with package.json

### NPM command
 npm init //create new package.json
 npm ls -g//list npm installed globally -g
 npm search loadash
 npm install loadash --save//-g for globally, --save-dev for devDependencies
 npm uninstall loadash
 npm prune //remove unused npm package
 npm update loadash//update based on package.json change
 npm adduser
 npm publish //publish package to npm
 
### package.json
 - name: project name
 - version: 0.0.1
 - main: entry point app.js
 - dependencies: "lodash": "^2.4.1"
 - scripts: auto run on start/test run w npm test
