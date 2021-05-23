# JDA-Kotlin-Extensions

JDA-Ktx is a package for Discord.JDA which uses Kotlin-Only features like Type-Safe Builders (for creating roles, text channels, embeds) and it has a built in SlashCommandHandler, Music Manager + Event Manager (with coroutines).
This package is mainly for me but you can also contribute.
+ The Slash Commands will likely change because jda's slash commands are not done

# Installation

Currently you can only install this dependency through jitpack: https://jitpack.io/#jan-tennert/JDA-Ktx/

# Example:

### Slash Commands

```kotlin
val jda = JDABuilder.createDefault("token").build()
val commandHandler = CommandHandler(jda)

jda.awaitReady()

//You can create a command through a class:

commandHandler.registerCommands(testCommand())

class testCommand : Command("hi", "Say hi to the bot") {
      override fun run(
            channel: TextChannel?,
            member: Member?,
            user: User,
            privateChannel: PrivateChannel?,
            hook: CommandHook,
            options: MutableList<SlashCommandEvent.OptionData>,
            event: SlashCommandEvent
        ) {
            event.reply("Hi, ${event.user.name}!").queue()

}


//Or through a type safe builder:
commandHandler.registerCommands(createSlashCommand {
     name = "hi"
     description = "Say hi to the bot"
     guildID = 631131922424135716

     action {
        val embed = messageEmbed {
            title = "Title!!"
            footer = "footer"
            field {
                name = "field"
                value = "value"
                inline = false
            }
        }
        it.reply(embed).queue()
     }
})
```

### Create roles & guild channels in custom event manager

```kotlin

val jda = JDABuilder.createDefault("token")
      .build()

jda.on<GuildMessageReceivedEvent>() {
     it.channel.sendMessage("Received Message!").queue()
        
     //Create channel:
        
     it.guild.createTextChannel("test") {
            
        slowMode = 20
        topic = "Very cool text channel!"
            
        addMemberPermission(523405005402) {
             + Permission.MESSAGE_READ //allow permission
             - Permission.MESSAGE_WRITE //deny permission
        }
            
    }
        
     //Create role:
        
     it.guild.createRole { 
            
         name = "role"
         color = Color.CYAN
         mentionable = false
            
         permissions {  //Add permissions to the role
             + Permission.KICK_MEMBERS
             + Permission.BAN_MEMBERS
         }
            
     }
}

```

### Custom events on channels, guilds

```kotlin

val guild = jda.getGuildById("guild")
val voiceChannel = guild.getVoiceChannelById("id")

voiceChannel.onMemberJoin {
      println("Member join")
}

guild.onMemberJoin {
      println("new member joined")
}

```

#### There are much more custom events!
