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
Call GET `https://bingomc.net/api/get_stats/?username=<username>` with a Minecraft UUID as the user's stats param.
Stats are the same as the stats that are shown ingame for the current user.
These stats are currently scoped to multiplayer games only.

When this request completes, you'll get back a response like this:
```json
{
  "success": true,
  "stats": {
    "total_games_won": 1,
    "total_games_played": 1,
    "total_items_found": 1,
    "total_hours_played": 1,
    "avg_minutes_played": 1, // average minutes per game
    "avg_items_found": 1, // average items per game
    "first_game": "<iso timeformat string of the first game they played>"
  }
}
```

When this request fails (because of the uuid not existing or never having played), you'll get back a response like this:
```json
{
    "success": false
}
```

An example value (for uuid `117bcad6-abb1-4eb8-8c50-a6c15a713c8c` in this case) would be:
```json
{
  "success": true,
  "stats": {
    "total_games_won": 96,
    "total_games_played": 271,
    "total_items_found": 915,
    "total_hours_played": 32,
    "avg_minutes_played": 7,
    "avg_items_found": 3,
    "first_game": "2022-01-07T10:48:29"
  }
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
- Playing players is the total amount of players playing a multiplayer game
- Waiting servers is the amount of servers that are ready to accept players for a new multiplayer game
- Running servers is the amount of multiplayer servers/games that are currently running (playing)
