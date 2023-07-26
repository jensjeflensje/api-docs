---
title: "Skin Renderer"
date: 2023-07-26T12:32:13+02:00
draft: false
---
Skin Renderer is a project with which you can generate profile photo-like images of a Minecraft skin. There are various rendering options, but not all are recommended for automation, you'll see.

Website: [https://skin-render.jensderuiter.dev](https://skin-render.jensderuiter.dev)

## First things first
To call the api, use `https://skin-render.jensderuiter.dev/api` as a base url and set the `Content-Type` header to `multipart/form-data` (request body must match, of course).

Images over this API are transported as a base64 string representation of the image.

Responses are always in JSON format.

## Generation
Call POST `https://skin-render.jensderuiter.dev/api/generate/` with the following parameters to request a skin render.

```properties
skin=<base64 representation of skin file>
offset=<0 or 1 depending on if you want to offset the top layer>
top_layer=<0 or 1 depending on if you want to render the top layer>
skin_style=<Steve or Alex>
blur=<0 or 1 depending on if you want background blur (makes the request slower)>
background_color=<hex color with #, e.g. #00FF00>
```

When this request completes, you'll get back a response like this:
```json
{
    "bytes": "<base64 representation of rendered image>",
    "success": true
}
```

You can use now this new base64 string to show on a website:
```html
<img src="data:image/png;base64,<what you got back>" />
```

Or save it using Python:
```py
import base64
img_b64 = '...' # what you got back
with open('./skin_render.png', 'wb') as new_file:
    new_file.write(base64.decodebytes(bytes(img_b64, "utf-8"))))
```

Or do whatever else you want with it!


## Fetching the skin
If you don't already have the skin file you need to make the generation call, you can request it from the API.

Call GET `https://skin-render.jensderuiter.dev/api/get_skin/<username>/` to get their skin.

When this request completes, you'll get back a response like this:
```json
{
    "bytes": "<base64 representation of the username's skin>",
    "success": true
}
```

Then use this base64 string to call the generation endpoint.