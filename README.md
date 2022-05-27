# Documentation

This is the technical documentation for Onboard ID. Refer to `api.md` to view REST API endpoints details and refer to `architecture.md` to view overall system architecture of our solution and how it integrates with stakeholders such as issuers and verifiers. 

## Getting Started

Prerequisite: 
1. Node version >= 16.13.1 (to check, run `node -v`)
2. Npm version >= 7.19.1 (to check, run `npm -v`)

Install required dependencies by running `npm install`. 

To run the server on localhost, run `npm start`. 

### Setup MongoDB

To test the functionalities of REST API endpoints, setup MongoDB locally to store the database.

1. Install MongoDB from https://www.mongodb.com/docs/manual/installation/. 
2. Add the path of the /bin directory to the PATH in your desktop.
3. Install MongoDB by running `npm install mongodb --save`.
4. Create a folder <folder name> in the same directory of your implementation code to store the database.

For everytime when you want to run REST API endpoints, run `mongod --dbpath=<folder name> --bind_ip 127.0.0.1` before running `npm start`.
