# MySQL/MariaDB Provider for Discord.js-Commando
Uses a MySQL or MariaDB (recommended) database to store guild settings. It requires the `mysql2` npm package.

This works with discord.js v12 but should be backwards compatible. That remains untested as I have no interest in supporting older versions. Feel free to fork it and do what you will!

**This is completely unsupported.** It works for me, it may not work for you. I'm unable to provide support for this provider.

# Installation
```
npm install commando-provider-mysql
```

# Usage (Example)
```js
const { CommandoClient } = require('discord.js-commando')
const mysql = require('mysql2/promise')
const mysqlProvider = require('commando-provider-mysql')
const path = require('path')

const client = new CommandoClient({
	commandPrefix: '?',
	owner: 'your-owner-id',
})

mysql.createConnection({
	host: 'mysql-host',
	user: 'mysql-user',
	password: 'mysql-password',
	database: 'mysql-db'
}).then((db) => {
	client.setProvider(new mysqlProvider(db))
})

client.registry
	.registerDefaultTypes()
	.registerGroups([
		['first', 'Your First Command Group'],
	])
	.registerDefaultGroups()
	.registerDefaultCommands()
	.registerCommandsIn(path.join(__dirname, 'commands'))

client.once('ready', () => {
	console.log(`Logged in as ${client.user.tag}! (${client.user.id})`)
	client.user.setActivity('with Commando');
})

client.on('error', console.error)

client.login('your-token-goes-here')
```