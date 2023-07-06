# How to Build a Card-Linked Application With the Web SDK

Fidel makes it simple to add card-linking capabilities to any application. The process involves setting up a program, along with the participating brands and locations. And then registering or helping your users register their cards on the Fidel platform. Once they are live, Fidel receives transactions from participating locations and will pass them along to your application using webhooks. You can register your webhook URLs in the Fidel Dashboard and start receiving the transaction data.

## What Are We Building?

There are two main steps in the process, and this tutorial is going to walk you through building an application with a card-linking feature.

![Card-Linked Application Demo](https://raw.githubusercontent.com/FidelLimited/docs/master/assets/images/tutorial-card-linking.png "Card-Linked Application Demo")

 The first requirement is to register user cards on the Fidel platform. Fidel provides SDKs to use within your applications to help you register cards simply and securely. This tutorial will show you how to use the Fidel Web SDK in a React application to register cards. 
 
 The second part of the card-linking process is the flow of transaction data from participating locations whenever a user makes a purchase with a registered card. For delivering the flow of transactions to your application, the Fidel platform uses webhooks. This tutorial is going to walk you through setting up a Node.js server that listens for transaction data, register it with the Fidel platform using ngrok, and start receiving transactions. You'll also use the server to send the transactions to the React client after receiving them, so you can display them in the UI.

## Prerequisites

Before you begin, make sure you've got a few things:

- A [Fidel Account](https://dashboard.fidel.uk/sign-in). You can create one via the [Fidel Dashboard](https://dashboard.fidel.uk/sign-up) if you don't already have one!
- [Node.js v12.22.1](https://nodejs.org/download/release/latest-v12.x/) or higher installed.
- A [ngrok](https://dashboard.ngrok.com/signup) account.
- [ngrok](https://ngrok.com/download) installed and set up.

## Build the Client Application to Link Cards

You'll first build a React client to use the Fidel Web SDK and give your application the ability to link cards to your Fidel program.

### Create React Application

Let's go ahead and create a new React application using `create-react-app`. After you've generated a new project called `fidel-card-linking-tutorial`, run it using the generated `npm start`.

```sh
npx create-react-app fidel-card-linking-tutorial
cd fidel-card-linking-tutorial
npm start
```

You should have a blank new React application running in your browser on port 3000. The generated files should look similar to this:

```sh
.
├── README.md
├── package.json
├── node_modules
├── public
│   ├── favicon.ico
│   ├── index.html
│   ├── logo192.png
│   ├── logo512.png
│   ├── manifest.json
│   └── robots.txt
├── src
│   ├── App.css
│   ├── App.js
│   ├── App.test.js
│   ├── index.css
│   ├── index.js
│   ├── logo.svg
│   ├── reportWebVitals.js
│   └── setupTests.js
└── yarn.lock

2 directories, 17 files
```

### Add Tailwind CSS

You'll want to create a nice-looking application, and for that, a CSS framework is probably the simplest to use option. Let's go ahead and add [TailwindCSS](https://tailwindcss.com/) to the empty React application. Add a line in the `<head>` section of the `/public/index.html` file:

```html
<link href="https://unpkg.com/tailwindcss@^2/dist/tailwind.min.css" rel="stylesheet">
```

### Add Empty Layout

After you've included Tailwind into the empty project, let's remove the boilerplate code in the `/src/App.js` file and replace it with an empty application shell. It's already set up to have a header and a table to display the transaction data you'll get from Fidel.

```jsx
import { ReactComponent as Logo } from "./assets/logo.svg";

function App() {
  const headers = ["Amount", "Cashback", "Scheme", "Card", "Brand", "Location", "Status", "Date↓"]

  return (
    <div className="App h-full">
      <div className="h-full overflow-x-hidden">
        <nav className="bg-white shadow">
          <div className="flex flex-col container mx-auto md:flex-row md:items-center md:justify-between">
            <div className="flex justify-between items-center">
              <div className="flex justify-between items-center">
                <a href="https://fidel.uk/docs" className="w-full">
                  <Logo style={{ width: "90px", height: "60px" }} />
                </a>
                <button
                  className="ml-10 w-full bg-blue-700 hover:bg-blue-900 text-white py-2 px-4 rounded">
                  Add Card
                  </button>
              </div>
            </div>
            <div className="md:flex flex-col md:flex-row md:-mx-4">
              <a
                href="https://fidel.uk/docs/web-sdk"
                className="my-1 hover:text-gray-800 text-blue-700 md:mx-4 md:my-0"
              >
                Documentation ↗
              </a>
            </div>
          </div>
        </nav>

        <div className="px-6 py-2 py-8">
          <div className="flex justify-between container mx-auto">
            <div className="w-full">
              <div className="flex items-center justify-between">
                <h1 className="text-xl text-gray-700 md:text-2xl">
                  Transactions
                </h1>
              </div>
              <div className="-my-2 py-2 overflow-x-auto sm:-mx-6 sm:px-6 py-2 lg:-mx-8 pr-10 lg:px-8">

                <div className="align-middle">
                  <table className="min-w-full">
                    <thead>
                      <tr>
                        {headers.map(header => (
                          <th className="px-6 py-2 py-3 text-left text-gray-400 font-light text-sm">{header}</th>
                        ))}
                      </tr>
                    </thead>
                    <tbody className="bg-white">
                    </tbody>
                  </table>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div >
  );
}

export default App;
```

### Add Logo Components

You've probably noticed your application is failing to compile now. And that's because you haven't yet added the logo component that's being used in the empty application shell above. To do that, create a new `assets` folder inside the `/src` directory, and create an empty `logo.svg` file. Now 

```sh
mkdir src/assets
touch src/assets/logo.svg
```

Your application is still going to fail to compile, but with a new error. And that's because the empty SVG file should actually be a valid SVG. Replace the contents of `/src/assets/logo.svg` with the Fidel logo:

```html
<?xml version="1.0" encoding="utf-8"?>
<svg version="1.1" id="Layer_1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" x="0px" y="0px"
   width="802px" height="407.6px" viewBox="0 0 802 407.6" style="enable-background:new 0 0 802 407.6;" xml:space="preserve">
<style type="text/css">
</style>
<g>
  <g>
    <path class="st0" d="M101.3,286.7h45v-65.3h30.8l17.9-36.2h-48.7v-27.5H195v-36.2h-93.7V286.7z M231.7,286.7h45.5V121.5h-45.5
      V286.7z M422.7,141.4c-7.8-6.7-17.1-11.8-27.7-15.2c-10.6-3.4-22.1-5.2-34-5.2h-42.9v165.7H361c14.8,0,27.9-2.2,38.9-6.6
      c10.9-4.3,20-10.4,27.1-17.9c7.1-7.6,12.4-16.5,15.9-26.6c3.5-10.3,5.3-21.3,5.3-32.9c0-13.6-2.3-25.7-6.9-35.9
      C436.7,156.5,430.4,148,422.7,141.4z M392.9,236.9c-6.9,7.9-16.9,11.9-29.7,11.9h-3.6v-90h3.6c26.2,0,40,15.6,40,45.1
      C403.2,218,399.7,229.1,392.9,236.9z M482.3,286.7H576v-37.9h-48.7v-27.4H576v-36.2h-48.7v-27.5H576v-36.2h-93.7V286.7z
       M660.9,248.8V121.5h-44.9v165.2h84.8v-37.9H660.9z"/>
  </g>
</g>
</svg>
```

## Add the Fidel Web SDK

Now that your application compiles successfully, you'll see an empty table layout with a button above that prompts you to "Add Card". At this stage, the button doesn't do anything, so you'll need to add that capability to the React application. This is where the Fidel Web SDK comes in handy. We'll add the SDK to the application, so we can start registering cards on the Fidel Platform.

First, at the top of your `/src/App.js` file, import `useEffect` from React. 

```jsx
import { useEffect } from "react";
```

The Fidel Web SDK is a JavaScript file hosted at `https://resources.fidel.uk/sdk/js/v3/fidel.js`. The required attributes for functioning correctly are the Fidel SDK Key, the Program Id and the company name. 

You can find the SDK Key in the ["Account" section of the Fidel Dashboard](https://dashboard.fidel.uk/account/plan). For the purpose of this tutorial, use the Test SDK Key. It should start with `pk_test_`. The Program Id can be found in the ["Program" section of the Dashboard](https://dashboard.fidel.uk/programs). The Demo Program that comes with each new account has a contextual menu that you can use to copy the program ID. For the company name, you can use anything you want. For the purposes of this tutorial, use something generic like "Fidel Card-Linking Application".

To add the SDK to your application, use an effect that runs only once at startup to create a `<script>` tag with the Fidel SDK, add the required attributes to it, and then append it to the document body. In the `App()` function of the `/src/App.js` file, add the effect:

```jsx
function App() {
  const headers = ["Amount", "Cashback", "Scheme", "Card", "Brand", "Location", "Status", "Date↓"]

  useEffect(() => {
    document.getElementById("fidel-form")?.remove();

    const sdkScript = document.createElement("script");
    sdkScript.src = "https://resources.fidel.uk/sdk/js/v3/fidel.js";
    sdkScript.id = "fidel-form";

    const attributes = {
      "data-auto-open": "false",
      "data-key": process.env.REACT_APP_FIDEL_SDK_KEY,
      "data-program-id": process.env.REACT_APP_FIDEL_PROGRAM_ID,
      "data-company-name": "Fidel Card-Linking Application",
    };

    Object.keys(attributes).forEach((key) =>
      sdkScript.setAttribute(key, attributes[key])
    );

    document.body.appendChild(sdkScript);
  }, []);

  return (
    ...
  )
}

export default App;
```

Because you've set `auto-open` to false in the SDK attributes, the SDK overlay will only show if called, with `Fidel?.openForm()`. Add a `onClick` handler to the "Add Card" button to open the SDK overlay when clicked.

```jsx
<button
  onClick={() => window.Fidel?.openForm()}
  className="ml-10 w-full bg-blue-700 hover:bg-blue-900 text-white py-2 px-4 rounded">
  Add Card
</button>
```

### Create Environment File

You might have noticed the example code didn't hardcode the values of the SDK key and program id but rather used environment variables. The generated React application already has support for environment variables. To use them, you need to create an `.env` file.

```sh
touch .env
```

Add variables to it, and fill out the values for your SDK key and program Id in `REACT_APP_FIDEL_SDK_KEY` and `REACT_APP_FIDEL_PROGRAM_ID`.

```sh
PORT=3001
REACT_APP_SERVER=http://127.0.0.1:3000
REACT_APP_FIDEL_SDK_KEY=
REACT_APP_FIDEL_PROGRAM_ID=
```

Because you've added the `PORT` environment variable, your application will now run on port 3001 and leave port 3000 open for the server we'll build in a minute. You'll need to restart your application with` npm start`. Your application should compile successfully and run at "http://localhost:3001/". If you click the "Add Card" button, a model should pop up with a form to link a card.

![Card-Linking Form](https://raw.githubusercontent.com/FidelLimited/docs/master/assets/images/tutorial-card-linking-form.png "Card-Linking Form")

### Test Card Numbers

Because you're using the Test SDK key in your application, you won't be able to link a real card in the SDK. There are a few options for test card numbers available in the [documentation](https://fidel.uk/docs/cards/#test-card-numbers). You can add a test Visa card, for example, by using the card number `4444 0000 0000 4001` and set an expiration date in the future, with a participating country of issue from the dropdown.

Congratulation, you have successfully added the ability to register cards to your React application and used it to link your first card on the Fidel platform!

## Server Listening For Webhook Events

To start receiving transactions from your linked card, you'll need to register webhooks in the Fidel Dashboard. Before you can register them, you'll need to build them. Create a new `server.js` file at the root of your `fidel-card-linking-tutorial` folder.

```sh
touch server.js
```

Let's go ahead and implement a fairly standard Node.js server, using `express`, that listens on port 3000. First, install the dependencies with `$ npm install express cors` and then add some boilerplate code to the `server.js` file.

```jsx
import express from 'express'
import { createServer } from 'http'
import cors from 'cors';

const PORT = 3000

const { json } = express;

const app = express()

app.use(json())
app.use(cors())

const server = createServer(app)

server.listen(PORT, () => {
    console.log(`Server listening at http://localhost:${PORT}`)
})
```

The Fidel platform can register a multitude of webhooks, so let's add a generic catch-all route `/api/webhooks/:type` that deals with webhooks and sends back a `200 OK` response. If your webhook doesn't return a 200 status, the Fidel platform retries sending the webhook until it receives a 200 status.

```jsx
app.post('/api/webhooks/:type', (req, res, next) => {

    res.status(200).end()
})
```

If you try to run the server as is right now, you'll get an error saying you "Cannot use import statement outside a module". And that's because you're using modern import statements in your Node.js code. You'll need to update the `package.json` with a new line to support imports.

```jsx
"type": "module"
```

It would also be helpful if you could run both the React client and the Node.js server with the same command. Update the `start` script inside `package.json` to run the server and the client at the same time. You'll need to run `npm start` again after you're done.

```jsx
"scripts": {
    "start": "node server.js & react-scripts start",
  },
```

### Register Webhooks with Fidel

Now that you have created a server listening for webhooks, it's time to register those webhooks on the Fidel platform. Your webhooks need to be publicly accessible on the Internet for Fidel to be able to access them. Sadly, `localhost` is not publicly accessible, so you'll need to use `ngrok` to make it so. 

There are a few other options here. Usually, with production code, you'll have it deployed somewhere with a URL. Or you'll have a load balancer in front of your code, and that will be publicly accessible. But for exposing local code, there aren't many options that don't involve a deployment. This is where `ngrok` comes in handy. It's a tunnelling software that creates a connection between a public URL they host, like `https://someRandomId.ngrok.io, and a port on your local machine.

Because the server is running on port 3000, you'll start `ngrok` on that port with the `http` option. You'll get back a random-looking URL that is publicly accessible on the Internet, and you can use it as the base of your webhook URLs. For example, `https://a709be896425.ngrok.io`.

![Card-Linking ngrok](https://raw.githubusercontent.com/FidelLimited/docs/master/assets/images/tutorial-card-linking-ngrok.png "Card-Linking ngrok")

After you've got a ngrok URL, you can go ahead and register a couple of webhooks in the [Fidel Dashboard](https://dashboard.fidel.uk/webhooks). For the purposes of this tutorial, register the `transaction.auth` webhook on the "Demo Program" to https://a709be896425.ngrok.io/api/webhooks/transaction.auth. Follow the same URL convention for registering the `transaction.clearing` and `transaction.refund` webhooks as well.

![Card-Linking Webhooks](https://raw.githubusercontent.com/FidelLimited/docs/master/assets/images/tutorial-card-linking-webhooks.png "Card-Linking Webhooks")

Congratulations, you've registered your first webhooks with Fidel. You can start receiving transaction data!

## Socket.io to Glue It All Together

You've probably noticed your server can receive transaction data from Fidel, but it doesn't do anything with that data. That's because what you do with that data depends heavily on your application use case. For the purpose of this tutorial, let's send that transaction data from the server to the React client, so you can display it in the UI.

Because the data comes from a webhook, your server doesn't have a lot of control over when it receives the data. Hence, your client can't keep asking for data that isn't there. Because of the event-driven nature of webhooks, long polling is not a great mechanism to pass data. There are a few options, mainly HTTP/2 Server Push, Node.js streams or Web Sockets. You can use something like `socket.io`, which combines WebSockets with long polling to ensure maximum browser compatibility while you pass the transaction data from the server to the client. And you'll use just that for this tutorial. Let's go ahead and install the dependencies with `$ npm install socket.io socket.io-client`.

### Add Socket.io to the Server

You'll need to add the socket mechanism to the server first. Import the `Server` from `socket.io` at the top of your `server.js` file.

```jsx
import { Server } from 'socket.io'
```

Before the webhook route is defined, instantiate a new socket Server, and log to the console every time a client connects to the socket. Update the webhook route handler to emit on the socket every time there is new transaction data coming from Fidel.

```jsx
const io = new Server(server, {
    cors: {
        origin: "http://localhost:3001",
        methods: ["GET", "POST"]
    }
})

io.on('connection', (socket) => {
    console.log('a client connected')
})

app.post('/api/webhooks/:type', (req, res, next) => {
    io.emit(req.params.type, req.body)

    res.status(200).end()
})
```

Now that the server is completed restart it with `npm start`.

### Add Socket.io to the Client

You'll need to add `socket.io` to the client as well, in order to receive the transaction data and display it. At the top of your `/src/App.js` file, import `socketIOClient` from `socket.io-client` and `useState` from react.

```jsx
import { useState, useEffect } from "react";
import socketIOClient from "socket.io-client";
```

After you've imported the socket in your `/src/App.js`, you need to create a new socketIOClient in an effect. And register a listener that sets the transactions state on any incoming data.

```jsx
function App() {
  const [transactions, setTransactions] = useState([]);

  useEffect(() => {
    const socket = socketIOClient(process.env.REACT_APP_SERVER);

    socket.onAny((type, transaction) => {
      setTransactions([{ type, transaction }, ...transactions]);
    });

    return () => socket.disconnect();
  });
  return (
    ...
  )
}
```

### Create Transaction Component

Now that we've got the data coming into the React client, let's create a `Transaction` component that displays it. First, create a `components` folder inside the `src` folder, and create an empty `Transaction.js` file in it.

```sh
mkdir src/components
touch src/components/Transaction.js
```

A transaction has a type and information about the currency, amount, card scheme, card number, store brand, name and location, and date of creation. Let's create a component that displays all that data to match the empty table we used in the initial empty shell for our application.

```jsx
import React from "react";

import { formatCard } from "../utils";

import { ReactComponent as Amex } from "../assets/amex-icon.svg";
import { ReactComponent as Visa } from "../assets/visa-icon.svg";
import { ReactComponent as Mastercard } from "../assets/mastercard-icon.svg";

import TransactionStatus from "./TransactionStatus";

const Transaction = ({ transaction, type, transactions }) => {
    return (
        <tr>
      <td className="px-6 py-2 whitespace-no-wrap border-b border-gray-200 text-sm">
        <div className="flex items-center justify-between">
          <span className="text-gray-400">{transaction?.currency}</span> 
          <span>{transaction?.amount}</span>
        </div>
      </td>
      <td className="px-6 py-2 whitespace-no-wrap border-b border-gray-200 text-sm">
        <span className="text-gray-400">--</span>
      </td>
      <td className="px-6 py-2 whitespace-no-wrap border-b border-gray-200 text-sm">
        {(transaction?.card?.scheme.toUpperCase() === "AMEX") && (<Amex />)}
        {(transaction?.card?.scheme.toUpperCase() === "VISA") && (<Visa />)}
        {(transaction?.card?.scheme.toUpperCase() === "MASTERCARD") && (<Mastercard />)}
      </td>
      <td className="px-6 py-2 whitespace-no-wrap border-b border-gray-200 text-sm text-gray-400">{formatCard(transaction?.card)}</td>
      <td className="px-6 py-2 whitespace-no-wrap border-b border-gray-200 text-sm">{transaction?.brand?.name}</td>
      <td className="px-6 py-2 whitespace-no-wrap border-b border-gray-200 text-sm">{transaction?.location?.address}</td>
      <td className="px-6 py-2 whitespace-no-wrap border-b border-gray-200 text-sm">
        <TransactionStatus status={type} />
      </td>
      <td className="px-6 py-2 whitespace-no-wrap border-b border-gray-200 text-sm">{new Date(transaction?.created).toLocaleString()}</td>
    </tr>
    );
};

export default Transaction;
```

For the application to compile, you'll need to create a few of the things we already used in the Transaction component. Start with the `TransactionStatus` component in the same folder as the `Transaction` component.

```sh
touch src/components/TransactionStatus.js
```

A transaction can have three statuses: authorized, cleared and refunded. The information is contained in the transaction type being passed from the server in the socket. Let's use that data to have a different coloured pill for the status and make it instantly recognizable in the UI.

```jsx
import React from "react";

const TransactionStatus = ({ status }) => (
  <div>
    {(status === "transaction.auth") && (
      <span className="relative inline-block px-3 py-1 font-semibold text-yellow-500">
        <span aria-hidden className="absolute inset-0 bg-yellow-200 opacity-50 rounded-full"></span>
        <span className="relative text-xs">Auth</span>
      </span>
    )}
    {(status === "transaction.clearing") && (
      <span className="relative inline-block px-3 py-1 font-semibold text-green-500">
        <span aria-hidden className="absolute inset-0 bg-green-200 opacity-50 rounded-full"></span>
        <span className="relative text-xs">Cleared</span>
      </span>
    )}
    {(status === "transaction.refund") && (
      <span className="relative inline-block px-3 py-1 font-semibold text-red-500">
        <span aria-hidden className="absolute inset-0 bg-red-200 opacity-50 rounded-full"></span>
        <span className="relative text-xs">Refund</span>
      </span>
    )}
  </div>
);

export default TransactionStatus;
```

You'll also need to create the icons for the three card networks in the assets folder as SVGs, the same as you did for the Fidel logo. 

First, create the Amex icon.

```sh
touch src/assets/amex-icon.svg
```

```html
<svg width="34" height="8" viewBox="0 0 34 8" fill="none" xmlns="http://www.w3.org/2000/svg">
    <path fill-rule="evenodd" clip-rule="evenodd" d="M30.8291 0L29.0156 2.53729L27.2136 0H25L27.951 3.99979L25.0099 8H27.1602L28.9735 5.42863L30.7864 8H33L30.0379 3.96571L32.9789 0H30.8291Z" fill="#2D6EB6"/>
    <path fill-rule="evenodd" clip-rule="evenodd" d="M19 0V8H25V6.38813H20.8003V4.77733H24.9038V3.17724H20.8003V1.62323H25V0H19Z" fill="#2D6EB6"/>
    <path fill-rule="evenodd" clip-rule="evenodd" d="M14.895 0L13.5001 5.66873L12.0946 0H9V8H10.7101V2.53765L10.6678 0.37783L12.7028 8H14.2976L16.3322 0.423478L16.2905 2.52586V8H18V0H14.895Z" fill="#2D6EB6"/>
    <path fill-rule="evenodd" clip-rule="evenodd" d="M3.29308 0L0 8H1.96474L2.61245 6.28552H6.26715L6.92556 8H9L5.71824 0H3.29308ZM3.87452 2.98299L4.43455 1.48624L4.99396 2.98299L5.6744 4.75448H3.2052L3.87452 2.98299Z" fill="#2D6EB6"/>
</svg>
```

Second, create the Mastercard icon.

```sh
touch src/assets/mastercard-icon.svg
```

```html
<svg width="21" height="13" viewBox="0 0 21 13" fill="none" xmlns="http://www.w3.org/2000/svg">
    <path fill-rule="evenodd" clip-rule="evenodd" d="M7.64941 11.4892H13.32V1.3754H7.64941V11.4892Z" fill="#F75B1B"/>
    <path fill-rule="evenodd" clip-rule="evenodd" d="M8.01018 6.43222C8.01018 4.38046 8.97821 2.5532 10.4854 1.37531C9.38287 0.513856 7.99213 0 6.48032 0C2.90126 0 0 2.87996 0 6.43222C0 9.98447 2.90126 12.8644 6.48032 12.8644C7.99213 12.8644 9.38287 12.3506 10.4854 11.4891C8.97821 10.3113 8.01018 8.48397 8.01018 6.43222Z" fill="#E20025"/>
    <path fill-rule="evenodd" clip-rule="evenodd" d="M20.769 10.4177V10.1683H20.7032L20.6278 10.34L20.5524 10.1683H20.4866V10.4177H20.5327V10.2294L20.6035 10.3919H20.6517L20.7225 10.229V10.4177H20.769ZM20.353 10.4177V10.2106H20.4372V10.1686H20.2228V10.2106H20.3069V10.4177H20.353ZM20.9713 6.43179C20.9713 9.98447 18.07 12.864 14.491 12.864C12.9792 12.864 11.5884 12.3501 10.4863 11.4887C11.9935 10.3113 12.9612 8.48361 12.9612 6.43179C12.9612 4.38003 11.9935 2.55278 10.4863 1.37489C11.5884 0.513856 12.9792 0 14.491 0C18.07 0 20.9713 2.87954 20.9713 6.43179Z" fill="#F0A029"/>
</svg>
```

And third, the Visa icon.

```sh
touch src/assets/visa-icon.svg
```

```html
<svg width="29" height="10" viewBox="0 0 29 10" fill="none" xmlns="http://www.w3.org/2000/svg">
    <path fill-rule="evenodd" clip-rule="evenodd" d="M14.7771 3.18797C14.762 4.57733 15.9235 5.35133 16.7987 5.81318C17.6979 6.28578 17.9996 6.5891 17.9962 7.01173C17.9894 7.65913 17.2786 7.94439 16.614 7.95551C15.4534 7.97511 14.7796 7.61668 14.2427 7.34624L13.8247 9.45988C14.3637 9.72733 15.3597 9.96058 16.3923 9.9723C18.8181 9.9723 20.4043 8.67787 20.412 6.67207C20.4222 4.12684 17.1548 3.98636 17.1772 2.84902C17.1846 2.50327 17.4892 2.13551 18.1565 2.04106C18.4871 1.99479 19.3995 1.95869 20.4328 2.47278L20.8385 0.427536C20.2826 0.209585 19.5682 0 18.6783 0C16.3964 0 14.79 1.31105 14.7771 3.18797ZM24.7395 0.176346C24.296 0.176346 23.9223 0.454791 23.7566 0.883759L20.2919 9.82142H22.716L23.1977 8.38041H26.1596L26.4386 9.82142H28.574L26.71 0.176346H24.7395ZM25.0777 2.78243L25.7772 6.40391H23.8622L25.0777 2.78243ZM11.8397 0.176346L9.92964 9.82142H12.2386L14.148 0.176346H11.8397ZM8.42354 0.176346L6.02029 6.74094L5.04824 1.15945C4.93439 0.536328 4.48377 0.176346 3.98336 0.176346H0.054434L0 0.455986C0.80632 0.645602 1.72283 0.951192 2.2779 1.27686C2.61777 1.47628 2.71458 1.65024 2.82632 2.12404L4.66732 9.82142H7.10774L10.8486 0.176346H8.42354Z" fill="url(#paint0_linear)"/>
    <defs>
        <linearGradient id="paint0_linear" x1="28.574" y1="0.259826" x2="0" y2="0.259826" gradientUnits="userSpaceOnUse">
            <stop stop-color="#21489F"/>
            <stop offset="1" stop-color="#261A5E"/>
        </linearGradient>
    </defs>
</svg>
```

The only piece missing is a little utils function to format the card number in the UI. Create a `utils` folder inside the `src` folder, and then an `index.js` file inside.

```sh
mkdir src/utils
touch src/utils/index.js
```

Fidel uses a tokenization mechanism for the card numbers and doesn't store the entire card number. You'll receive the first six, and last four numbers from the card number, and the six in between are missing. You'll replace those with asterisks and then split the long card number every other 4 characters to display. In the `index.js` file, create a `formatCard` method that contains the logic.

```jsx
export function formatCard(card) {
  return `${card?.firstNumbers}******${card?.lastNumbers}`
    .match(/.{4}/g)
    .join(" ");
}
```

Now that you've created all the missing pieces for the `Transaction` component go ahead and add it to your main application. At the top of the `/src/App.js` file, import the `Transaction` component.

```jsx
import Transaction from "./components/Transaction";
```

Inside the render function, there is currently an empty `<tbody>` tag. Replace it to iterate over the `transactions` data, and map a `Transaction` component for each entry.

```jsx
<tbody className="bg-white">
  {transactions.map(({ transaction, type }, idx) => (
    <Transaction key={idx} transaction={transaction} type={type} transactions={transactions} />
  ))}
</tbody>
```

Congratulations, you can now display incoming transactions from Fidel!

## Simulate Transactions

Because you're using a Test Fidel key, and because you're using test cards, you won't receive live transactions. There are [different ways you can simulate test transactions](https://fidel.uk/docs/transactions/#test-transactions) on the Fidel platform thought to test this tutorial. One of the simplest ones is to use the [API Playground](https://dashboard.fidel.uk/playground) in the Fidel Dashboard. Go in there and use the `transactions /create` endpoint to create a few different transactions. If you create one with a positive amount, followed by one with a similar but negative amount, it will be matched as a refunded transaction. This way, you'll get to experience all three transaction statuses in the UI. Because you've set up the webhook endpoints already, all transactions you create are passed from Fidel to your ngrok URL, to your local server and then to your React client via socket.io. You should see them showing up in the UI, similar to this.

![Card-Linked Application Demo](https://raw.githubusercontent.com/FidelLimited/docs/master/assets/images/tutorial-card-linking.png "Card-Linked Application Demo")

I hope you've had a great time following along this tutorial, and you've learnt the basics of building your first card-linked application on the Fidel platform, using the Fidel Web SDK, Node.js, React and socket.io. If you want to take a look at the code, it's [available on GitHub](https://github.com/FidelLimited/fidel-card-linking-tutorial)!