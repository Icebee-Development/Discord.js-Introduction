# Discord.js introduction
This chapter assumes you already have basic knowledge of javascript. If not then go to [Codecademy](https://www.codecademy.com/learn/introduction-to-javascript) and take a javascript course.

In this chapter We'll guide you through the development of a simple bot with some useful commands. We'll start with the basic example:
```js
const { Client, Intents } = require("discord.js");
const client = new Client({
  intents: [Intents.FLAGS.GUILDS, Intents.FLAGS.GUILD_MESSAGES]
});

// Displays text message "I am ready" when bot has started
client.once("ready", () => {
  console.log("I am ready");
});

// Checks if message starts with "hi" if true then replys with "hello"
client.on("messageCreate", (message) => {
  if(message.content.startsWith("hi")) {
    message.channel.send("hello");
  }
});

// Logs in your bot with discord token
client.login("yourToken");
```