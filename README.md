# Splash Intro

- Download Docker and launch browser
- in url, enter `https://duckduckgo.com`

- You should now see HTML markup and png image of duckduckgo

## Lua Code In Splash

```lua
function main(splash, args)
  url = args.url
  assert(splash:go(url))
  assert(splash:wait(1))

  -- User Agent

  splash:on_request(function(request)
  	request:set_header('User-Agent', 'Blue')
  end)
  -- User Agent

   headers = {
    ['User-Agent'] = "Here Again!"∏
  }

	splash:set_custom_headers(headers)

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

# Scrape Javascript Heavy Websites Using Splash
(www.livecoin.net/en)
```
scrapy startproject livecoin    
cd livecoin
scrapy genspider coin www.livecoin.net/en
```
### Install And Configure ScrapySplash
https://github.com/scrapy-plugins/scrapy-splash
- Install
```
pip install scrapy-splash
```
- Configure
1. In `settings.py` of project: `SPLASH_URL = 'http://192.168.59.103:8050'` or `SPLASH_URL = 'http://localhost:8050'` if you have docker desktop
2. Add middleware to `settings.py`
```python
DOWNLOADER_MIDDLEWARES = {
  'scrapy_splash.SplashCookiesMiddleware': 723,
  'scrapy_splash.SplashMiddleware': 725,
  'scrapy.downloadermiddlewares.httpcompression.HttpCompressionMiddleware': 810,
}
```
3. Spider Middleware to `settings.py` 
```py
SPIDER_MIDDLEWARES = {
    'scrapy_splash.SplashDeduplicateArgsMiddleware': 100,
}
```
4. Set a custom DUPEFILTER_CLASS:
```py
DUPEFILTER_CLASS = 'scrapy_splash.SplashAwareDupeFilter'
```
5. If you use Scrapy HTTP cache then a custom cache storage backend is required. scrapy-splash provides a subclass of scrapy.contrib.httpcache.FilesystemCacheStorage:
```py
HTTPCACHE_STORAGE = 'scrapy_splash.SplashAwareFSCacheStorage'
```