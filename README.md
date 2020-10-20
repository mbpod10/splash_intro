# Splash Intro

- Download Docker and launch browser
- in url, enter `https://duckduckgo.com`

```lua
function main(splash, args)
  url = args.url
  assert(splash:go(url))
  assert(splash:wait(1))
  return {
    image = splash:png(),
    html = splash:html()
  }
end
```

- You should now see HTML markup and png image of duckduckgo

```lua
function main(splash, args)
  url = args.url
  assert(splash:go(url))
  assert(splash:wait(1))

  input_box = assert(splash:select("#search_form_input_homepage"))
  input_box:focus()
  input_box:send_text("my user agent")
  assert(splash:wait(0.5))
  --[[
  btn = assert(splash:select("#search_button_homepage"))
  btn:mouse_click()
  --]]

  input_box:send_keys("<Enter>")
  assert(splash:wait(1))

  return {
    image = splash:png(),
    html = splash:html()
  }
end
```
