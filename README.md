# Gametime.js-X
This is a publish-subscribe API that uses a public socket server or PubNub to function. It is based on the previous projects [Gametime.js](https://github.com/Parking-Master/Gametime.js) and [Gametime.js-2.0](https://github.com/Parking-Master/Gametime.js-2.0). This project relies only on pure JavaScript to work.

This API comes with a public socket server hosted by us. As an alternative, you can also use [PubNub](https://www.pubnub.com/), but that is not completely free and is a bit slower. If the script fails to connect to our socket server, it will try to connect to PubNub instead (only if you provide your Publish/Subscribe keys). There is also an option to use PubNub completely instead of our server.

## Documentation
### └ Installation
Gametime.js-X should be embedded in your web page in order to function properly.

```javascript
<script src="https://cdn.jsdelivr.net/gh/Parking-Master/Gametime.js-X@latest/gametime.js"></script>
```

For the minified version:

```
<script src="https://cdn.jsdelivr.net/gh/Parking-Master/Gametime.js-X@latest/gametime.min.js"></script>
```

<img src="icons/lightbulb.svg"> Tip: always keep this library at the latest version for bug fixes and critical updates.

### └ Usage
Setting up and using this API is simple and easy. Here, we'll show you how to connect to the Gametime.js-X socket server (or use PubNub instead), then we'll show you how to send a JavaScript function over the internet. We'll also show you additional methods like setting the max players on one channel, disconnecting player(s), and generating UUIDs for a user.

#### Setting the channel
Channels (in the context of a socket server) are like chat rooms. Only people inside them will be able to send and recieve messages to and from other users connected to the channel. It is required to create a unique indentifier to seperate channels in a game or chat room.

In other words, a group of users connected to a certain channel will be able to interact with each other over the internet using Gametime.js-X.

To set the channel, use the `set` method to specify a certain channel. Here is an example:

```javascript
await gametime.set("channel", "example-channel123");
```

Specify the channel in the second field.

#### Connecting to a server
To use the API, you need to connect to a socket server first.

To use our public socket server, use the `setCustomServer` method and specify our server's URL.

```javascript
await gametime.setCustomServer("https://caribou-needed-implicitly.ngrok-free.app");
```

To use PubNub instead, [log in](https://admin.pubnub.com/#/login) to your PubNub account first, and then go to your Dashboard and take note of your Publish/Subscribe keys. Then, specify them using the `set` method below:

```
await gametime.set("key", "pub-c-xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx", "sub-c-xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx");
```

<img src="icons/notebook.svg"> Note: Fill in the fields with your PubNub Publish/Subscribe keys.

Using either of these methods will most likely connect you to a server. If it doesn't, make sure you PubNub API plan is active, and that our public server is currently up. Our server may go down briefly from time to time, so if it's down for you, please wait for it to come back up.

#### Sustaining a connection
Next, you will use an asynchronous function to wait until Gametime.js-X has successfully connected you to a channel. This step is critical, so if you don't do it, the API will not successfully connect you to the channel.

Here is an example of this, using the `sustain` method:

```javascript
await gametime.sustain();
```

<img src="icons/notebook.svg"> Note: Make sure to seperate this function from the channel/server setup, and the actual API usage.

#### API usage & methods
Now that you've set up Gametime.js-X, you are ready to use it. First, you will define a function by default on the page, so every user on the page will have the functions to be run by other users (e.g. define a function that will show a message on the page when another user posts it). Then, you will run that using another function, and it will be run on the same page if other users are also connected to it.

- Method: `on(eventName: String, callback: Function(arguments))`
  - This method defines a function to run later.
- Method: `run(eventName: String, arguments: Array[])`
  - This method runs a function what was previously defined with `on()`.

Here is an example of defining a function:

```javascript
gametime.on("postMessage", function(user, message) {
  if (user == gametime.player.position) return;

  // `message` is a custom argument that contains an example message posted by another user.
  document.querySelector(".exampleChatWrapper").textContent += message;
});
```

<img src="icons/notebook.svg"> Note: Functions run using Gametime.js-X will be run for every user in the channel. To specify every user except for the one who posted the message, we've included the line `if (user == gametime.player.position) return;` in the example.

Now, this is how you can run that function:

```javascript
gametime.run("postMessage", [gametime.player.position, "This is an example chat message!"]);
```

You can call the function whatever you want (in this example, "postMessage"), and you can specify as many arguments as you want in your function. This is a basic example showing how to define a chat room post-message function, and then run it when a user wants to send a message. Feel free to customize this however you'd like and create whatever you want with it.

### └ Methods
- Method: `on(eventName: String, callback: Function(arguments))`
  - This method defines a function to run later.
- Method: `run(eventName: String, arguments: Array[])`
  - This method runs a function what was previously defined with `on()`.
- Method: `sustain()`
  - Waits for Gametime.js-X to sustain a connecting to a server and channel.
- Method: `set(globalSetting: String("key" | "channel"), channel | publishKey: String, subscribeKey: String)`
  - Sets a global setting such as "key" or "channel".
- Method: `setCustomServer(socketServerURL: String)`
  - When run, will attempt to connect to the specified socket server if possible. If it is not available, it will then attempt to use PubNub instead.
- Method: `uuidv4()`
  - Generates a UUIDv4. Automatically used by the API when a user connects.
- Method: `onconnect()`
  - A custom function that will run when you connect to a channel.
- Method: `ondisconnect()`
  - A custom function that will run when you disconnect from a channel.
- Method: `logger.info(message: any)`
  - This is a built in `console.log` method with colored output and unicodes. This specific function is the _info_ method.
- Method: `logger.error(message: any)`
  - This is a built in `console.log` method with colored output and unicodes. This specific function is the _error_ method.
- Method: `logger.warn(message: any)`
  - This is a built in `console.log` method with colored output and unicodes. This specific function is the _warn_ method.
- Method: `logger.success(message: any)`
  - This is a built in `console.log` method with colored output and unicodes. This specific function is the _info_ method.
- Method: `disconnect()`
  - Disconnect the current user from the channel.

<img src="icons/notebook.svg"> Note: These are all methods apart of the global `gametime` variable when the API is installed (e.g. `gametime.on()`, `gametime.run()`, etc.)

### └ Examples
Example on sending arguments through a function:

```javascript
gametime.on("myCustomFunction", function(arg1, arg2, arg3) {
  console.log(arg1, arg2, arg3);
  // Output: "foo bar baz"
});

let arg1 = "foo";
let arg2 = "bar";
let arg3 = "baz";

gametime.run("myCustomFunction", [arg1, arg2, arg3]);
```

Example of setting the max amount of players/users:

```javascript
// The default is 16
gametime.maxPlayers = 3;
```
Example of setting the socket server:


```javascript
// Replace SERVER_URL with the URL to your socket server
gametime.setCustomServer(SERVER_URL);

// For example, this is the public socket server for Gametime.js-X
gametime.setCustomServer("https://caribou-needed-implicitly.ngrok-free.app");
```

Example chat room page:

<details>
  <summary>Click to expand</summary>

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Gametime.js-X chat room</title>
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/gh/Parking-Master/Gametime.js-X@latest/example_chat.css">
    <script src="https://cdn.jsdelivr.net/gh/Parking-Master/Gametime.js-X@latest/gametime.js"></script>
  </head>
  <body>
    <div id="loading">Loading chat...</div>
    <ul id="messages"></ul>
    <form id="form">
      <input id="input" placeholder="Type your message here...">
      <button class="send">Send</button>
      <button class="disconnect">Disconnect</button>
    </form>
    <script>
      (async () => {
        // We'll try to connect to a Gametime.js-X socket server, but will use PubNub instead if it is not available
        await gametime.setCustomServer("https://caribou-needed-implicitly.ngrok-free.app");
        await gametime.set("channel", "my-chat-room1234");
        // You don't need to set your PubNub keys, though it is an alternative in case the public server doesn't work
        await gametime.set("key", "pub-c-xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx", "sub-c-xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx");
        await gametime.sustain();

        document.querySelector("#loading").remove();

        gametime.on("postChatMessage", function(message) {
          let item = document.createElement("li");
          item.textContent = message;
          messages.appendChild(item);
          window.scrollTo(0, document.body.scrollHeight);
          let chatHistory = localStorage["history"];
          let noBug = chatHistory === "" ? "" : ",";
          localStorage.setItem("history", (chatHistory + noBug + encodeURIComponent(message)).split(",").toString());
        });

        let messages = document.querySelector("#messages");
        let form = document.querySelector("#form");
        let input = document.querySelector("#input");

        if (!localStorage["history"]) {
          localStorage.setItem("history", "");
        } else {
          for (var i = 0; i < localStorage["history"].split(",").length; i++) {
            var item = document.createElement("li");
            item.textContent = decodeURIComponent(localStorage["history"].split(",")[i]);
            messages.appendChild(item);
            window.scrollTo(0, document.body.scrollHeight);
          }
        }

        form.onsubmit = function(event) {
          event.preventDefault();
          if (input.value) {
            gametime.run("postChatMessage", [input.value]);
            input.value = "";
          }
        };

        document.querySelector(".disconnect").onclick = function(event) {
          event.preventDefault();
          gametime.disconnect();
        };
      })();
    </script>
  </body>
</html>
```

</details>

[Live example of this page](https://parking-master.github.io/Gametime.js-X/example_chat.html)

<br>
<br>

For other open-source examples, see [FPS-X](https://github.com/Parking-Master/FPS-X) which is an open source FPS game that you can build yourself with Gametime.js-X and pure JavaScript.

## License
This project is licensed under the __MIT license__, and is free to use, remake, and distribute however you like.

###### Copyright &copy; 2021-2024 Parking Master
