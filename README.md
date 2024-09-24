# Proxy Integrations


If you're like most web developers and scraping experts, you've probably used <b>Selenium</B> or an equivalent browser automation tool for one of your projects. These 
are powerful testing and web page interaction tools but in most cases they require a proper <b>proxy IP integration</b> to successfully complete certain job types 
without getting blocked.
There are 2 options for proxy integrations — the first is integrating directly with [Bright Data super proxies](https://brightdata.com/proxy-types/proxy-servers) 
and the second is through the Bright Data [proxy manager](https://github.com/luminati-io/luminati-proxy).

![Bright Data Proxy Manager](https://github.com/luminati-io/proxy-integrations/blob/main/Proxy%20Manager.png)

<h2>Selenium proxy integration</h2>

[Selenium](https://github.com/SeleniumHQ/selenium) web driver is a popular browser automation tool among Python coders that may be used to create realistic
browsing situations for the most precise website testing as well as web scraping that emulates a real-user's interaction with the web page.
<h3>To integrate Selenium with Bright Data super proxies, follow these steps:</h3>

* First, access your Bright Data [control panel](https://brightdata.com/cp/zones) and click ‘<b>add zone</b>’.
* Select your preferred network type - Datacenter, ISP, Residential, Mobile, etc.  and click '<b>add zone</b>' again.
* In Selenium web driver, fill in the ```Proxy IP:Port``` in the ```setProxy``` function for example ```zproxy.lum-superproxy.io:22225``` of both HTTP and HTTPS.
* Under ```sendKeys``` input your Bright Data account ID and proxy zone name:```lum-customer-CUSTOMER-zone-YOURZONE``` and your zone password found in the proxy zone 
* settings.
* Here is an example of what your code should look like:

```s
const {Builder, By, Key, until} = require('selenium-webdriver');
const proxy = require('selenium-webdriver/proxy');

(async function example(){
  let driver = await new Builder().forBrowser('firefox').setProxy(proxy.manual({
    http: 'zproxy.lum-superproxy.io:22225',
    https: 'zproxy.lum-superproxy.io:22225'
  })).build()

  try {
    await driver.get('http://lumtest.com/myip.json');
    driver.switchTo().alert()
      .sendKeys('lum-customer-USERNAME-zone-YOURZONE'+Key.TAB+'PASSWORD');
    driver.switchTo().alert().accept();
  } finally {
      await driver.quit();
  }
})();
```
<h3>To integrate Selenium with Bright Data proxy manager, follow these steps:</h3>

* Create a proxy zone with the network, IP type, and number of IPs you require.
* [Install](https://brightdata.com/products/proxy-manager) the Bright Data Proxy Manager on your machine or access it via cloud on your Bright Data control panel.
* Click the <b>‘add new proxy’</b> option and choose the zone and settings you require for it, then click <b>‘save’</b>.
* Go to Selenium web driver.  Under the setProxy type in your local IP and proxy manager port (i.e. 127.0.0.1:24000).
* The local host IP is 127.0.0.1.
* The port that is created in the Proxy Manager is in the format 24XXX, for example 24000.
* Don't type username and password into the fields — the Bright Data Proxy Manager is already authenticated with the Super Proxy server.
* Here is an example how your Selenium code should look like:

```s
const {Builder, By, Key, until} = require('selenium-webdriver');
const proxy = require('selenium-webdriver/proxy');

(async function example(){
    let driver = await new Builder().forBrowser('firefox').setProxy(proxy.manual({
        http: '127.0.0.1:24000',
        https: '127.0.0.1:24000'
    })).build()

    try {
        await driver.get('http://lumtest.com/myip.json');
        driver.switchTo().alert().accept();
    } finally {
        await driver.quit();
    }
})();
```

Start using the <b>[Selenium proxy integration here](https://brightdata.com/integration/selenium)</b>.


<h2>Puppeteer proxy integration</h2>

Puppeteer is a Node library created to control and automate headless and non-headless Chrome and Chromium browsers with its high-level API. Though it wasn't
originally designed to be used as a testing platform, it has become a very popular alternative to Selenium among JavaScript users and features some additional
[stealth extra plug-ins](https://github.com/berstend/puppeteer-extra).

<h3>To integrate Puppeteer with Bright Data super proxies, follow these steps:</h3>

* First, access your Bright Data control panel and click <b>‘add zone’</b>.
* Select your preferred proxy network type - Datacenter, ISP, Residential, Mobile, etc. and click <b>'add zone'</b> again.
* Go to Puppeteer, add the ```Proxy IP:Port``` in the ```proxy-server``` value, for instance ```zproxy.lum-superproxy.io:22225``` .
* Under the ```page.authenticate``` insert your Bright Data account ID and proxy zone name in the ```username``` value, this way: ```lum-customer-CUSTOMER-zone-YOURZONE``` and your  proxy zone password found in the proxy zone settings.
* Here is an example how what your Puppeteer code should look like:

```javascript
const puppeteer = require('puppeteer');

(async () => {
  const browser = await puppeteer.launch({
    headless: false,
    args: ['--proxy-server=zproxy.lum-superproxy.io:22225']
  });
  const page = await browser.newPage();
    await page.authenticate({
        username: 'lum-customer-USERNAME-zone-YOURZONE',
        password: 'PASSWORD'
    });
    await page.goto('http://lumtest.com/myip.json');
    await page.screenshot({path: 'example.png'});
    await browser.close();
})();
```

<h3>To integrate Puppeteer with Bright Data proxy manager, follow these steps:</h3>

* Access your Bright Data control panel and create a zone with the proxy network type, IP type, and number of IPs you require.
* Install the Proxy Manager on your device or access it via the cloud on your Bright Data control panel.
* Click <b>‘add new proxy’</b> and choose the zone and settings you require, click <b>‘save’</b>.
* In Puppeteer, under the ```proxy-server```, insert your local IP and Bright Data Proxy Manager port (i.e. 127.0.0.1:24000).
* The local host IP is 127.0.0.1
* The port created by default in the Bright Data Proxy Manager is in the format 24XXX, e.g., 24000.
* Don't type username and password into the fields — the Bright Data Proxy Manager is already authenticated with the Super Proxy server.
* Here is an example how what your Puppeteer code should look like:

```javascript
const puppeteer = require('puppeteer');

(async () => {
    const browser = await puppeteer.launch({
        headless: false,
        args: ['--proxy-server=127.0.0.1:24000']
    });
    const page = await browser.newPage();
    await page.authenticate();
    await page.goto('http://lumtest.com/myip.json');
    await page.screenshot({path: 'example.png'});
    await browser.close();
})();
```

Start using the Bright Data <b>[Puppeteer proxy integration here](https://brightdata.com/integration/puppeteer)</b>.


<h2>Playwright proxy integration</h2>

Playwright is a Node.js library that allows Chromium, Firefox, and WebKit automation using a single API. Here are the steps for a quick Playwright proxy Integration 
with Bright Data.

<h3>To integrate Playwright with Bright Data super proxies, follow these steps:</h3>

* First, access your Bright Data control panel and click <b>‘add a zone’</b>.
* Select your preferred proxy network type - Datacenter, ISP, Residential, Mobile, etc. and click <b>'add zone'</b> again.
* Go to Playwright and insert the ```Proxy IP:Port``` in the ```server``` value, for example ```http://zproxy.lum-superproxy.io:22225``` .
* Under ```username``` type in your Bright Data account ID and proxy zone name, for example:```lum-customer-CUSTOMER-zone-YOURZONE``` and under ```password``` type in 
your zone password found in the Bright data proxy zone settings.
* Here is an example how what your Playwright code should look like:

```javascript
const playwright = require('playwright');

(async () => {
    for (const browserType of ['chromium', 'firefox', 'webkit']) {
        const browser = await playwright[browserType].launch({
            headless: false,
            proxy: {
                server: 'http://zproxy.lum-superproxy.io:22225',
                username: 'lum-customer-USERNAME-zone-YOURZONE',
                password: 'PASSWORD'
            },
        });
        const context = await browser.newContext();
        const page = await context.newPage();
        await page.goto('http://lumtest.com/myip.json');
        await page.screenshot({ path: 'example.png' });
        await browser.close();
    }
})();
```

<h3>To integrate Playwright with Bright Data proxy manager, follow these steps:</h3>

* Access your Bright Data control panel and create a zone with the proxy network type, IP type, and number of IPs you require.
* Install the Proxy Manager on your device or access it via the cloud on your Bright Data control panel.
* Click <b>‘add new proxy’</b> and choose the zone and settings you require, click <b>‘save’</b>.
* In Playwright, under the ```server```, insert your local IP and Bright Data Proxy Manager port (i.e. 127.0.0.1:24000).
* The local host IP is 127.0.0.1.
* The port created by default in the Bright Data Proxy Manager is in the format 24XXX, e.g., 24000.
* Don't type username and password into the fields — the Bright Data Proxy Manager is already authenticated with the Super Proxy server.
* Here is an example of what your Playwright code should look like:

```javascript
const playwright = require('playwright');

(async () => {
    for (const browserType of ['chromium', 'firefox', 'webkit']) {
        const browser = await playwright[browserType].launch({
            headless: false,
            proxy: {
                server: '127.0.0.1:24000',
                username: '',
                password: ''
            },
        });
        const context = await browser.newContext();
        const page = await context.newPage();
        await page.goto('http://lumtest.com/myip.json');
        await page.screenshot({ path: 'example.png' });
        await browser.close();
    }
})();
```

Start using the Bright Data <b>[Playwright proxy integration here](https://brightdata.com/integration/playwright)</b>.


<h2>Other useful Bright Data proxy integrations</h2>

* PhantomBuster - watch the proxy integration tutorial video on [YouTube](https://youtu.be/Tw68CHXs_jE).
* Apify
* SessionBox
* VMLogin
* AdsPower

Go to Bright Data [proxy integrations](https://brightdata.com/integration) center to learn what's new.
