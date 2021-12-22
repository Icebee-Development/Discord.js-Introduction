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
  if(message.content.startsWith("ping")) {
    message.channel.send("pong!");
  }
});

// Logs in your bot with discord token
client.login("yourToken");
```

## Introducing events
Before we dive into any further coding, we need to first understand what an *Event* is.

This is an event:
```js
client.on("messageCreate", (message) => {
  // This code runs when the event is triggered
});
```

This is, specifically, an event in *discord.js* but it's similar to how other APIs handle events. This event triggers *every time the bot sees a message.* This includes every channel the bot has access to as well as any direct or private message it receives. If someone sends 5 messages on a channel, this event fires 5 times.

Why is this important? Well, if you intend to use your bot on a large server, or if you want it to be on multiple servers, this becomes a large number of events triggering at every moment. I don't want to go into too much optimization talk, but for a single point: **use a single event function for each event**.

Discord.js contains a large number of events that can trigger under certain situations. For instance, the `ready` event triggers when the bot comes online. The `guildMemberAdd` event triggers when a new user joins a server shared with the bot. For a full list of events, see [Events in the documentation](https://discord.js.org/#/docs/main/stable/class/Client?scrollTo=e-applicationCommandCreate). We will come back to some of those later in this chapter.

## Adding a second command
One of the first useful things you might want to learn is how to add a second command to your bot. While there are *better* ways than what I'm about to show you, for the time being this will be enough.

{% hint style="info" %} From now on I will omit the code that requires and initiates the discord.js and concentrate on specific parts of the code. {% endhint %}

```js
client.on("messageCreate", (message) => {
  if (message.content.startsWith("ping")) {
    message.channel.send("pong!");
  } else

  if (message.content.startsWith("foo")) {
    message.channel.send("bar!");
  }
});
```

Save your code and restart your bot. To do so, use `CTRL+C` in the command line, and re-run `node index.js`. Yes, there are better ways to reload the code, as you will see later in this book.

You can test your new command by saying `foo` in a channel you share with the bot. You can also confirm that `ping` still returns `pong`!