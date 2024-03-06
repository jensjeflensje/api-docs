---
title: "BingoMC"
date: 2023-09-22T20:45:00+02:00
draft: false
---
BingoMC is my Minecraft server. It features singleplayer and multiplayer bingo games with various game settings.

Website: [https://bingomc.net](https://bingomc.net)

## First things first
This is a public API, which means you won't be able to actually change stuff. It's read only.
The API also has a rate limit in place.
The current limit is 20 requests per second, which will be plenty for most real-world use cases.

## Get UUID
Call GET `https://bingomc.net/api/get_uuid/?username=<username>` with a Minecraft username as the username param.
This endpoint can be used to fetch someone's UUID,
and as a side note, also returns whether a player has ever been online on the server.

When this request completes, you'll get back a response like this:
```json
{
    "uuid": "<the username's uuid>",
    "success": true
}
```

When this request fails (because of the player never having joined), you'll get back a response like this:
```json
{
    "success": false
}
```

## Get stats
Call GET `https://bingomc.net/api/get_stats/?uuid=<uuid>` with a Minecraft UUID as the uuid stats param.
Stats are the same as the stats that are shown ingame for the current user.

When this request fails (because of the uuid not existing or never having played), you'll get back a response like this:
```json
{
  "avg_items_per_game": null,
  "avg_time_per_game": null,
  "avg_singleplayer_time": null,
  "first_multiplayer_game": null,
  "first_singleplayer_game": null,
  "total_games_multiplayer": null,
  "total_games_singleplayer": 0,
  "total_games_won_multiplayer": null,
  "total_items_found_multiplayer": null,
  "total_items_found_singleplayer": null,
  "total_balloons": null
}
```

An example value would be:
```json
{
  "avg_items_per_game": 3,
  "avg_time_per_game": 722, // seconds
  "avg_singleplayer_time": 32, // seconds
  "first_multiplayer_game": "2022-01-07T10:48:29", // iso 8601
  "first_singleplayer_game": "2022-01-07T10:48:29", // iso 8601
  "total_games_multiplayer": 314,
  "total_games_singleplayer": 1,
  "total_games_won_multiplayer": 115,
  "total_items_found_multiplayer": 1052,
  "total_items_found_singleplayer": 16,
  "total_balloons": 63
}
```

## Get widget data
Call GET `https://bingomc.net/api/widget_data/` to get realtime stats on the entire network.
These stats were originally meant for a widget,
but can be used as a JSON representation of the network's stats.

When this request completes, you'll get back a response like this:
```json
{
  "success": true,
  "data": {
    "total_players": 4,
    "playing_players": 2,
    "waiting_servers": 3,
    "running_servers": 1
  }
}
```

To elaborate on these stats:
- Total players is the total amount of players on the network
- Playing players is the total amount of players that are ingame
- Waiting servers is the amount of servers that are ready to accept players for new games
- Running servers is the amount of servers/games that are currently running (playing)
