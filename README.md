# proxy-integrations
Bright Data proxy integrations guide
If you're like most web developers and scraping experts, you've probably used Selenium or an equivalent browser automation tool for one of your projects. These are 
powerful testing and web page interaction tools but in most cases they require a proper <b>proxy IP integration</b> to successfully complete certain job types without 
getting blocked.
There are 2 options for proxy integrations — the first is integrating directly with <b>Bright Data super proxies</b> and the second is through the Bright Data <b>[proxy 
  manager](https://github.com/luminati-io/luminati-proxy)</b>.

<h2>Selenium proxy integration</h2>

[Selenium](https://github.com/SeleniumHQ/selenium) web driver is a popular browser automation tool among Python coders that may be used to create realistic
browsing situations for the most precise website testing as well as web scraping that emulates a real-user's interaction with the web page.
<h3>To integrate Selenium with Bright Data super proxies, follow these steps:</h3>

* First, access your Bright Data [control panel](https://brightdata.com/cp/zones) and click ‘<b>add zone</b>’.
* Select your preferred network type - Datacenter, ISP, Residential, Mobile, etc.  and click '<b>add zone</b>' again.
* In Selenium web driver, fill in the ‘Proxy IP:Port’ in the ‘setProxy’ function for examplezproxy.lum-superproxy.io:22225 of both HTTP and HTTPS.
* Under ‘sendKeys’ input your Bright Data account ID and proxy zone name:```sh lum-customer-CUSTOMER-zone-YOURZONE``` and your zone password found in the proxy zone settings.
* Here is an example how your code should look like:
```sh
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
* Click the ‘add new proxy’ option and choose the zone and settings you require for it, then click ‘save’.
* Go to Selenium web driver.  Under the setProxy type in your local IP and proxy manager port (i.e. 127.0.0.1:24000).
* The local host IP is 127.0.0.1.
* The port that is created in the Proxy Manager is in the format 24XXX, for example 24000.
* Don't type username and password into the fields — the Bright Data Proxy Manager is already authenticated with the Super Proxy server.
* Here is an example how your Selenium code should look like:
```sh
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
