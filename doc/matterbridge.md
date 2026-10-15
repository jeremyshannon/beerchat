# Matterbridge relay

This manual describes how to set up a local matterbridge chat relay.

The `beerchat` mod communicates via http to the matterbridge api, for details see [matterbridge api reference](https://github.com/matterbridge-org/matterbridge/blob/master/docs/api/README.md)

## Install matterbridge

Get matterbridge from [matterbridge-org/matterbridge](https://github.com/matterbridge-org/matterbridge/blob/master/doc/compiling.md)

A working example with `docker-compose` is located in the `/dev` folder of this repository

## Configure the matterbridge

This `matterbridge.toml` example connects the ingame `main` channel to libera `#beerchat` and a personal discord server:

```toml
[irc]
[irc.Libera]
Server="irc.libera.chat:6667"
Nick="BeerchatTestBot"
RemoteNickFormat="<{NICK}> "
ColorNicks=true
ShowJoinPart=true

[discord]
[discord.Discord]
Token="<omitted>"
Server="839951944515715084"
RemoteNickFormat="<{NICK}> "
ShowJoinPart=true
UseUserName=true

[api.luanti]
BindAddress="127.0.0.1:4242"
Token="mytoken"
Buffer=1000
RemoteNickFormat="{NICK}"
ShowJoinPart=true

[[gateway]]
name="main"
enable=true
[[gateway.inout]]
account="irc.Libera"
channel="#beerchat"
[[gateway.inout]]
account = "discord.Discord"
channel="matterbridge"
[[gateway.inout]]
account="api.luanti"
channel="api"
```

(tokens and passwords are omitted in this example)

For all settings see [matterbridge settings docs](https://github.com/matterbridge-org/matterbridge/blob/master/docs/settings.md).

## Configure Luanti

Add the `beerchat` mod to the http-settings and configure the url and token:

```text
secure.http_mods = beerchat
# your local matterbridge setup
beerchat.matterbridge_url = http://127.0.0.1:4242
# The token you set up in the previous `matterbridge.toml` file (under `[api.myapi]`/Token)
beerchat.matterbridge_token = mytoken
```

Optionally, you may add a trigger to address this server specifically, in the event that you have more than one server connected:
```
beerchat.trigger = BeerchatTestBot
```
The trigger can be anything, but must not contain spaces, and you will probably want it to match the server's Nick. You can then send it commands like so:
```
!BeerchatTestBot players
```
The above will run the !players command, but only on BeerchatTestBot.

## Start the matterbridge and Luanti

Start the matterbridge server and Luanti, there might be some errors in the Luanti console if the matterbridge isn't available
