# Official Links
 * [Moei Twitter](https://twitter.com/0xMoei)
 * [AO discord](https://discord.gg/aAnB5SJJaa)


# Preparations

## Installing aos
```shell
npm i -g https://get_ao.g8way.io
```

## Run aos
```shell
aos
```
- This Create a main and unique aos Proccess ID for you
- Press Ctrl+C twice to exit aos chat


# CRED and Quests
https://cookbook_ao.g8way.io/welcome/testnet-info/cred-and-quests.html

## Join Dev chat
```shell
.load-blueprint chat
```

## List Rooms
```shell
List()
```

## Join Quests room
```shell
Join("Quests")
```

## Create a nickname
- you can replace nickname
```shell
Join("Getting-Started", "nickname")
```

## Check Quests and CRED lists
```shell
Say("/Quests", "Quests")
```

## Check Details of a Quest
```shell
Say("/Quests:1", "Quests")
```
- Press Ctrl+C twice to exit aos chat

---------------------------------
# "Quest 1 : Begin" Guide
---------------------------------

# Messaging
## Run aos
```shell
aos
```

## Send a Message
```shell
Send({ Target = "process ID", Data = "Hello World!" })
```

## Store Morpheus's Process ID
```shell
Morpheus = "wu_tAUDUveetQZpcN8UxHt51d9dyUkI4Z-MfQV8LnUU"
```

## Check the Morpheus Variable
```shell
Morpheus
```

## Send a Message to Morpheus
```shell
Send({ Target = Morpheus, Data = "Morpheus?" })
```

## Check your Inbox
- when you send a message to the bots, you get a message in your inbox
```shell
Inbox[#Inbox].Data
```

## Send a Message to Morpheus with tags
```shell
Send({ Target = Morpheus, Data = "Code: rabbithole", Action = "Unlock" })
```

# Create lua Chatroom
- We have to create a file so better you not exit the aos and open a new terminal tab with +
## Create a file for lua Chatroom
```shell
nano chatroom.lua
```

## Paste these codes to the files
```shell
Members = Members or {}
Handlers.add(
    "Register",
    Handlers.utils.hasMatchingTag("Action", "Register"),
    function (msg)
      table.insert(Members, msg.From)
      Handlers.utils.reply("registered")(msg)
    end
  )
  Handlers.add(
    "Broadcast",
    Handlers.utils.hasMatchingTag("Action", "Broadcast"),
    function (msg)
      for _, recipient in ipairs(Members) do
        ao.send({Target = recipient, Data = msg.Data})
      end
      Handlers.utils.reply("Broadcasted.")(msg)
    end
  )
```
Ctrl + X / Y / Enter

## Reload Lua Chatroom
- Go back to aos terminal
```shell
.load chatroom.lua
```

## Check your lua Chatroom
```shell
 Handlers.list
```

## Register our proccess ID to lua Chatroom
```shell
 Send({ Target = ao.id, Action = "Register" })
```

## Check lua Chatroom members
```shell
 Members
```
- You see your proccess ID joined the chat


## Broadcast your first massage to the chatroom
```shell
  Send({Target = ao.id, Action = "Broadcast", Data = "Broadcasting My 1st Message" })
```

## Invite Morpheus to the Chatroom
```shell
Send({ Target = Morpheus, Action = "Join" })
```

## Check lua Chatroom members (Morpheus Added)
```shell
 Members
```

## Check your Inbox
- when you invited Morpheus to the Chatroom he sent you Trinity ID in your Inbox
```shell
Inbox[#Inbox].Data
```
- Copy Trinity Proccess ID

## Store Trinity's Process ID
- Paste Trinity Process ID in the command below first then store it
```shell
Trinity = "Paste_your_Proccess_ID"
```

## Invite Trinity to the Chatroom
```shell
Send({ Target = Trinity, Action = "Join" })
```

## Check lua Chatroom members (Trinity Added)
```shell
 Members
```
- Now you should have 3 Proccess IDs in the members


# Build a Token
## Join Token Chatroom
```shell
.load-blueprint token
```

## Verify the Chat is loaded
```shell
Handlers.list
```
- You should see a new list of handlers that have been loaded into your aos process


## Testing the Token
```shell
Send({ Target = ao.id, Action = "Info" })
```

## Sending 1000 Tokens to Trinity
```shell
Send({ Target = ao.id, Action = "Transfer", Recipient = Trinity, Quantity = "1000"})
```

## Edit your Broadcast handler
- Open a new terminal without aos
```shell
nano chatroom.lua
```
- Delete all the lines and paste the code below into it
```shell
Members = Members or {}
Handlers.add(
    "Register",
    Handlers.utils.hasMatchingTag("Action", "Register"),
    function (msg)
      table.insert(Members, msg.From)
      Handlers.utils.reply("registered")(msg)
    end
  )
Handlers.add(
    "Broadcast",
    Handlers.utils.hasMatchingTag("Action", "Broadcast"),
    function(m)
        if Balances[m.From] == nil or tonumber(Balances[m.From]) < 1 then
            print("UNAUTH REQ: " .. m.From)
            return
        end
        local type = m.Type or "Normal"
        print("Broadcasting message from " .. m.From .. ". Content: " .. m.Data)
        for i = 1, #Members, 1 do
            ao.send({
                Target = Members[i],
                Action = "Broadcasted",
                Broadcaster = m.From,
                Data = m.Data
            })
        end
    end
)
```
Ctrl+X / Y / Enter

## Reload the chatroom.lua file
- Go back to aos terminal
```shell
.load chatroom.lua
```

## Test the Tokengate
```shell
Send({ Target = ao.id , Action = "Broadcast", Data = "Hello" })
```
Expected Results:
```shell
message added to outbox
New Message From [Your Process ID]: Action = Broadcasted
Broadcasting message from [Your Process ID]. Content: Hello.
```

## Testing from another Process ID
- Create a new Terminal to make a new proccess ID
```shell
aos chatroom-no-token
```
```shell
.load chatroom.lua
```
```shell
Send({ Target = ao.id, Action = "Register" })
```
```shell
Send({ Target = ao.id , Action = "Broadcast", Data = "Hello?" })
```

## Tell Trinity "It is done"
- Go back to your main aos terminal
```shell
Send({ Target = ao.id , Action = "Broadcast", Data = "It is done" })
```

- Wait 30 seconds and Trinity will send you a messege saying "I guess Morpheus was right. You are the one. Consider me impressed. You are now ready to join The Construct, an exclusive chatroom available to only those that have completed this tutorial. Now, go join the others by using the same tag you used Register, with this process ID: jg2Duezl68c8lHU5RiV8kHZrZ-7MJSVyyfQDhz5nJqQ Good luck. -Trinity". Additionally, a footer will follow the message.

You Are done now you have to
1) Join Construct Chat
2) Send your discord-ID to Trinity
3) Post your main aos Proccess ID in #q1-claims discord channel

## 1) Join Construct Chat
```shell
Send({Target = "jg2Duezl68c8lHU5RiV8kHZrZ-7MJSVyyfQDhz5nJqQ", Action = "Register"})
```

## 2) Send your discord-ID to Trinity
```shell
Send({ Target = Trinity, Action = "Claim", Data = "Your-Discord-Username" })
```

## 3) Post your main aos Proccess ID in #q1-claims discord channel
 * [AO discord](https://discord.gg/aAnB5SJJaa)
