# chromium-headless

A simple image containing headless Chromium built for testing. This container contains all the dependencies that are required to run Chromium and it spins up in remote debugging mode. Chromium will also echo the CDP websocket address to `stdout`.

## Running

`docker run --init -d -p 9222:9222 --name chromium-test yashdalfthegray/chromium-headless`

Once the container is up, you should be able to `curl http://localhost:9222/json/version` from the Docker host or just navigate to the [Remote debugging webpage](http://localhost:9222/) to see the container running.

## Puppeteer

This container is really meant to work seamlessly with Puppeteer. You can use something like `request-promise` to query for the websocket URL from the container and use the code below to connect/disconnect to the container.

**Note**: If you call `browser.close()` while using this container, the browser inside the container will close and the container will stop. Instead use `browser.disconnect()`.

```js
const puppeteer = require("puppeteer-core");
const request = require("request-promise");

(async () => {
  const response = await request("http://localhost:9222/json/version");
  const { webSocketDebuggerUrl: browserWSEndpoint } = JSON.parse(response);

  const browser = await puppeteer.connect({ browserWSEndpoint });

  const page = await browser.newPage();

  await page.setViewport({ height: 768, width: 1200 });
  await page.goto("https://www.github.com/yashdalfthegray");
  await page.waitForSelector("img.avatar");

  await page.close();
  browser.disconnect();
})();
```

## Other tags

You shouldn't really need to use anything other than `latest` but listed below are the other tags in the repository.

### Using Docker version < 1.13

This container uses the `--init` flag to make sure that Chromium isn't misbehaving while running as the root process. Since the `--init` flag was introduced in Docker v1.13, the `tini` tag pulls in [`tini`](https://github.com/krallin/tini) to initialize the container properly for Docker versions older than 1.13.

### Running somewhere other than `localhost`

**Use with extreme care and perhaps not at all.**

The tag called `remote` comes built in with an `nginx` proxy that works with not only the rest endpoint but also the websocket that Puppeteer uses to control Chromium. This is an interesting experiment because you are then able to run this container on a separate host than your Puppeteer automation scripts. It does come with a serious warning though -- do not expose this to the internet because it could go horribly wrong! You're basically giving control of a Chromium instance to anyone with access to the URL.

_Note_: the `remote` tag uses a different port (8080) than the port (9222) that the other tags use.
