# Demo Plugin: Server

The server component of this demo plugin is written in Go and [net/rpc](https://golang.org/pkg/net/rpc/). It relies on a configured `ChannelName` and `Username` in [plugin.json](../plugin.json) to implement each of the supported hooks. 

Each of the included files or folders is outlined below.

## [go.mod](go.mod), [go.sum](go.sum)

These are metadata files managed by [vgo](https://github.com/golang/vgo) for dependency management. While vgo is currently in beta, it will launch as part of the standard Go 1.11 tooling and stabilize in subsequent releases. It was preferred for this project over [dep](https://github.com/golang/dep) since it does not require locating your plugin in the `$GOPATH`.

## [main.go](main.go)

This is the entry point of your plugin binary, that in turn invokes [plugin.ClientMain](https://godoc.org/github.com/mattermost/mattermost-server/plugin#ClientMain) to wire up RPC communication between your plugin and the Mattermost Server.

## [plugin\_id.go](plugin_id.go)

This is a file generated by the [build/manifest](../build/manifest) tool that captures the plugin id from [plugin.json](../plugin.json). It simplifies the need to hard-code the plugin id in multiple places by exporting a constant for use instead.

## [plugin.go](plugin.go)

This file defines the `Plugin` struct, embedding [plugin.MattermostPlugin](https://godoc.org/github.com/mattermost/mattermost-server/plugin#MattermostPlugin) to automatically handle the wiring up the [API](https://godoc.org/github.com/mattermost/mattermost-server/plugin#API) when the plugin starts. It contains public fields that are automatically unmarshalled from [plugin.json](../plugin.json) as part of the `OnConfiguration` hook in [configuration.go](configuration.go).

## [activate\_hooks.go](activate_hooks.go)

### OnActivate

This demo implementation logs a message to the demo channel whenever the plugin is activated.

### OnDeactivate

This demo implementation logs a message to the demo channel whenever the plugin is deactivated.

## [configuration.go](configuration.go)

### OnConfigurationChange

This demo implementation ensures the configured demo user and channel are created for use
by the plugin.

## [channel\_hooks.go](channel_hooks.go)

### ChannelHasBeenCreated

This demo implementation logs a message to the demo channel whenever a channel is created.

### UserHasJoinedChannel

This demo implementation logs a message to the demo channel whenever a user joins a channel.

### UserHasLeftChannel

This demo implementation logs a message to the demo channel whenever a user leaves a channel.

## [command\_hooks.go](command_hooks.go)

### ExecuteCommand

This demo implementation responds to a `/demo_plugin` command, allowing the user to enable
or disable the demo plugin's hooks functionality (but leave the command and webapp enabled).

## [http\_hooks.go](http_hooks.go)

### ServeHTTP

This demo implementation sends back whether or not the plugin hooks are currently enabled. It
is used by the web app to recover from a network reconnection and synchronize the state of the
plugin's hooks.

## [message\_hooks.go](message_hooks.go)

### MessageWillBePosted

This demo implementation rejects posts in the demo channel, as well as posts that @-mention
the demo plugin user.

### MessageWillBeUpdated

This demo implementation rejects posts that @-mention the demo plugin user.

### MessageHasBeenPosted

This demo implementation logs a message to the demo channel whenever a message is posted,
unless by the demo plugin user itself.

### MessageHasBeenUpdated

This demo implementation logs a message to the demo channel whenever a message is updated,
unless by the demo plugin user itself.

## [team\_hooks.go](team_hooks.go)

### UserHasJoinedTeam

This demo implementation logs a message to the demo channel in the team whenever a user
joins the team.

### UserHasLeftTeam

This demo implementation logs a message to the demo channel in the team whenever a user
leaves the team.

## [login\_hooks.go](login_hooks.go)

### UserWillLogIn

This demo implementation rejects login attempts by the demo user.

### UserHasLoggedIn

This demo implementation logs a message to the demo channel whenever a user logs in.
