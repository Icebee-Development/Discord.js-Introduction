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

From now on I will omit the code that requires and initiates the discord.js and concentrate on specific parts of the code.

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

## Using a Prefix
You might have noticed that a lot of bots respond to commands that have a prefix. This might be an exclamation mark (!), a dot (.), a question mark(?), or another character but with the introduction of slash commands it is heavily advised against using `/`. But this is useful for two things.

First, if you don't use a unique prefix and have more than one bot on a server, both will respond to the same commands. On developer servers, typing `!help` leads to a flood of replies and private messages which is something to avoid.

Second, in the example above we respond when the message *starts with* the 3 characters, `foo`. In its current state, this means the following sentence will trigger the bot's response: **fool, you have not heard the last of me!**. Yes, that's an odd example, but it's still valid - say this on your bot's channel and he will respond.

To work around this, we'll be using prefix, which we will store in a variable. This way we get the prefix as well as the ability to change it for all commands in one place. Here's an example code that does this:

```js
// Set the prefix
const prefix = "!";
client.on("messageCreate", (message) => {
  // Exit and stop if it's not there
  if (!message.content.startsWith(prefix)) return;

  // The back ticks are Template Literals introduced in Javascript in ES6 or ES2015, as an replacement for String Concatenation Read them up here https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals
  if (message.content.startsWith(`${prefix}ping`)) {
    message.channel.send("pong!");
  } else

  if (message.content.startsWith(`${prefix}foo`)) {
    message.channel.send("bar!");
  }
});
```

The changes to the code are still simple. Let's go through them:
- `const prefix = "!";` defines the prefix as the exclamation mark. You can change it to something else, of course.
- The line `if (!msg.content.startsWith(prefix)) return;` is a small bit of optimization which reads: "If the message does not start with my prefix, stop what you're doing". This prevents the rest of the function from running, making your bot faster and more responsive.
- The commands have changed so use this prefix, where startsWith(\`\${prefix}ping\`\) would only be triggered when the message starts with `!ping`.

The second point is just as important as having a single `message` event handler. Let's say the bot receives a hundred messages every minute (not much of an exaggeration on popular bots). If the function does not break off at the beginning, you are processing these hundred messages in each of your command conditions. If, on the other hand, you break off when the prefix is not present, you are saving all these processor cycles for better things. If commands are 1% of your messages, you are saving 99% processing power...