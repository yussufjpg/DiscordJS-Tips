# Logging User Based Activities

Every event is being logged in a channel which is named as value of logChannel variable. 

**Logging Voice Channel Activities**

To log voice channel activites we need to make a function and run it every time we get a signal by using ['voiceStateUpdate'](https://discord.js.org/#/docs/main/stable/class/Client?scrollTo=e-voiceStateUpdate) event.

```js
//First we have to write a function to detect users voice activity.

function getUsersVoiceActivity(oldState,newState){
	if(newState.voiceChannel == undefined){
		//If voiceChannel of newState equals to undefined this means user left the channel.
		return var restofLog = `left a voice channel ${oldState.voiceChannel}`
	}else if(newState.voiceChannel != undefined && oldState.voiceChannel == undefined){
		//If users old voiceChannel equals to undefined and new voiceChannel doesn't, this means user joins a voice channel.
		return var restofLog = `joined a voice channel (${newState.voiceChannel}).`
	}else if(newState.deaf == true && oldState.deaf == false){
		//If deaf property of oldState equals to false and newState doesn't, this means user deafens himself.
		return var restofLog = 'deafed himself.'
	}else if(newState.deaf == false && oldState.deaf == true){
		//Opposite of detecting deafening.
		return var restofLog = 'stopped deafening himself.'
	}else if(newState.mute == true && oldState.mute == false){
		//If mute property of oldState equals to false and newState doesn't, this means user mutes himself.
		return var restofLog = 'muted himself.'
	}else if(newState.mute == false && oldState.mute == true){
		//Opposite of detecting self mute.
		return var restofLog = 'stopped mutening himself.'
	}else if(newState.voiceChannel != oldState.voiceChannel && oldState.voiceChannel != undefined){
		//If users old and new voiceChannel doesn't equals to undefined this means user switched from old voiceChannel to new voiceChannel.
		return var restofLog = `switched voice channel(${oldState.voiceChannel} to ${newState.voiceChannel}).`
	}
}

//Running the function when we get a signal.
client.on('voiceStateUpdate', (oldState,newState) => {
	let user = client.users.get(newState.id)
	newState.guild.channels.find(channel => channel.name === logChannel).send(`${user.username}#${user.discriminator} ${getUsersVoiceActivity(oldState,newState)}`)
});
```

**Logging New Messages**

['message'](https://discord.js.org/#/docs/main/stable/class/Client?scrollTo=e-message) Event works when a new message is sent.

```js
client.on('message', message => {
	message.guild.channels.find(channel => channel.name === logChannel).send(`message.author just sent a new message ${message.content} to #${message.channel.name} channel.`)
})
```

**Logging Message Updates**

['messageUpdate'](https://discord.js.org/#/docs/main/stable/class/Client?scrollTo=e-messageUpdate) Event works when a message is updated.

```js
client.on('messageUpdate', (oldMessage,newMessage) => {
	oldMessage.guild.channels.find(channel => channel.name === logChannel).send(`${message.author} changed his ${oldMessage.content} message with ${newMessage.content} in ${newMessage.channel.name}.`)
});
```

**Logging Deleted Messages**

['mesageDelete'](https://discord.js.org/#/docs/main/stable/class/Client?scrollTo=e-messageDelete) Event works when a message is deleted.

```js
client.on(`messageDelete`, message => {
	message.guild.channels.find(channel => channel.name === logChannel).send(`${message.author} deleted his ${message.content} message in ${message.channel.name}.`)
})
```

You can make this look much better with embeds, you can go to 'Generating Embeds' section if you want to make.

p.s. You can log emotes but logging them is basically useless so i didn't add it.
