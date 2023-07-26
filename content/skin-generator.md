---
title: "Skin Generator"
date: 2023-07-26T13:22:01+02:00
draft: false
---
Skin Generator is a project with which you can generate Minecraft skins based on premade components.

Website: [https://jens.skin](https://jens.skin)

## First things first
To call the api, use `https://jens.skin/api` as a base url and set the `Content-Type` header to `application/json` (request body must match, of course).

Responses (except for the generate endpoint) are always in JSON format.

## List categories
Categories are component groups.
For example: skin color, hair, eyes...

Call GET `https://jens.skin/api/get_categories/` and you'll get a response like this:
```json
[
    {
        "id": 2,
        "name": "Skin",
        "description": "Choose the skin color",
        "icon": "/static/categories/skin_W6Y9oZO.png",
        // some categories don't have any color profiles per component
        "color_profiles": []
    },
    {
        "id": 4,
        "name": "Hair",
        "description": "Choose hair style",
        "icon": "/static/categories/hair_3DRiwtK.png",
        // some categories have multiple color profiles, serving as pallettes
        // the user will be prompted to select one profile
        "color_profiles": [
            {
                "name": "Hair -> blond",
                "colors": [
                    {
                        "name": "Hair blond 1",
                        "color": "#F5C168"
                    },
                    {
                        "name": "Hair blond 2",
                        "color": "#E0A85B"
                    },
                    {
                        "name": "Hair blond 3",
                        "color": "#C68848"
                    },
                    {
                        "name": "Hair blond 4",
                        "color": "#A46938"
                    }
                ]
            },
            {
                "name": "Hair -> ginger",
                "colors": [
                    {
                        "name": "Hair ginger 1",
                        "color": "#FF7A43"
                    },
                    {
                        "name": "Hair ginger 2",
                        "color": "#E25A41"
                    },
                    {
                        "name": "Hair ginger 3",
                        "color": "#BC3E30"
                    },
                    {
                        "name": "Hair ginger 4",
                        "color": "#9A2A2A"
                    }
                ]
            },
        ]
    },
    {
        "id": 1,
        "name": "Eyes",
        "description": "Choose the eye style for your character",
        "icon": "/static/categories/eyes_gIbZKgn.png",
        // some categories have a single color profile
        // the user will be prompted to select one color from the profile
        "color_profiles": [
            {
                "name": "Eyes",
                "colors": [
                    {
                        "name": "Eye color 1",
                        "color": "#E500DD"
                    },
                    {
                        "name": "Eye color 2",
                        "color": "#5B1D51"
                    },
                ]
            }
        ]
    },
]
```


## List components
Components are inside categories.
You can only select one component per category.

Call GET `https://jens.skin/api/get_components/<category id>/` and you'll get a response like this:
```json
[
    {
        "id": 18,
        "category_id": 4,
        "name": "Hair 1",
        "thumbnail": null,
        "texture": "/static/textures/hair_1.png"
    },
    {
        "id": 19,
        "category_id": 4,
        "name": "Hair 2",
        "thumbnail": null,
        "texture": "/static/textures/hair_2.png"
    },
    {
        "id": 20,
        "category_id": 4,
        "name": "Hair 3",
        "thumbnail": null,
        "texture": "/static/textures/hair_3.png"
    },
    {
        "id": 21,
        "category_id": 4,
        "name": "Hair 4",
        "thumbnail": null,
        "texture": "/static/textures/hair_4.png"
    },
]
```

## Generate skin
For skin generation, send a JSON object containing category ids as keys, and objects (with component id and colors) as values.
You don't have to send every possible category.
Every category is optional.

Call POST `https://jens.skin/api/generate/` with a request body like this:
```json
{
    "1": {
        "value": 1,
        "color": [ // a single color (when the category has one profile)
            "#86BC00"
        ]
    },
    "2": {
        "value": 16
    },
    "4": {
        "value": 18,
        "color": [ // a color profile (when the category has multiple profiles)
            {
                "name": "Hair dark gray 1",
                "color": "#4D565F"
            },
            {
                "name": "Hair dark gray 2",
                "color": "#3B4048"
            },
            {
                "name": "Hair dark gray 3",
                "color": "#2A2D34"
            },
            {
                "name": "Hair dark gray 4",
                "color": "#1E2127"
            }
        ]
    },
    "9": {
        "value": 40
    }
}
```

And you'll get a response body will be a 64x64 PNG image containing the skin.
The above payload will result in the following image:

![Generated skin](/content/skin_example.png)