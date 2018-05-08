# GraphQL Node Server

### Start Project

Install devDependencies.

```sh
$ mkdir graphql-project && cd graphql-project
$ mkdir node-server && cd node-server
$ npm init -y
$ yarn add --dev nodemon babel-cli babel-preset-env babel-preset-stage-3
$ touch index.js
$ touch .babelrc 
```

Configuration babel presets and nodemon .

```sh
$ nano .babelrc
{
    "presets": ["env", "stage-3"]
}
$ nano package.json
...
"scripts": {
	"start": "nodemon --exec babel-node index.js"
}
...
```
Setup Eslint Airbnb .

```sh
$ eslint --init
? would you like to configure ESLint?  Use a popular style guide
? Which style guide do you want to follow?  Airbnb
? Do you use React?  No
? What format do you want your config file to be in?  JavaScript
# Mac or Linux
(
 export PKG=eslint-config-airbnb;
 npm info "$PKG@latest" peerDependencies --json |  command sed 's/[\{\},]//g ; s/: /@/g'  | xargs npm install --save-dev "$PKG@latest"
)
```
Install the dependencies.

```sh
$ yarn add express body-parser apollo-server-express graphql graphql-tools
```
Create GraphQL Schema

```sh
// The GraphQL schema in string form
const typeDefs = `
  type Query { books: [Book] }
  type Book { title: String, author: String }
`;
export default typeDefs;
```
Create GraphQL Resolvers

```sh
// Some fake data
const books = [
  {
    title: "Harry Potter and the Sorcerer's stone",
    author: 'J.K. Rowling',
  },
  {
    title: 'Jurassic Park',
    author: 'Michael Crichton',
  },
];

// The resolvers
const resolvers = {
  Query: { 
    books: (parent, args, context, info) => books
  },
};
export default resolvers;
```
Create Server

```sh
$ nano index.js
import express from 'express';
import bodyParser from 'body-parser';
import { graphqlExpress, graphiqlExpress } from 'apollo-server-express';
import { makeExecutableSchema } from 'graphql-tools';

import typeDefs from './schema';
import resolvers from './resolvers';

// Put together a schema
const schema = makeExecutableSchema({
  typeDefs,
  resolvers,
});
// Initialize the app
const app = express();

// The GraphQL endpoint
app.use('/graphql', bodyParser.json(), graphqlExpress({ schema }));

// GraphiQL, a visual editor for queries
app.use('/graphiql', graphiqlExpress({ endpointURL: '/graphql' }));

// Start the server
app.listen(3000, () => {
    console.log('Go to http://localhost:3000/graphiql to run queries!');
});
```
Run server

```sh
$ yarn start
```
