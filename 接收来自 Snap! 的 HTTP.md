- Snap！向运行 MicroBlocks 的 WiFi 板发出 HTTP 请求的权限
## 问题

MicroBlocks 仅支持简单的 HTTP 服务器（并且由于各种技术原因，这不太可能改变）。不幸的是，大多数现代浏览器会阻止从通过 HTTPS 服务的页面发出的纯 HTTP 请求，例如[https://snap.berkeley.edu/snap/snap.html](https://snap.berkeley.edu/snap/snap.html)。

那么我们如何允许 Snap! 通过 HTTP 与 MicroBlocks 板通信？
## 解决方案

最简单的方法是运行 Snap！使用纯 HTTP：http: [//extensions.snap.berkeley.edu/snap.html](http://extensions.snap.berkeley.edu/snap.html)。

事实证明，还有另一种解决方案只能在 Chrome 中使用：给 Snap！允许使用“混合内容”或“不安全内容”（即从 HTTPS 页面发出 HTTP 请求）。详情在这里：[https://docs.adobe.com/content/help/en/target/using/experiences/vec/troubleshoot-composer/mixed-content.html](https://docs.adobe.com/content/help/en/target/using/experiences/vec/troubleshoot-composer/mixed-content.html)

Firefox 也有类似的解决方法，但是过程比较繁琐，而且由于 Firefox 不记得设置，所以每次打开 Snap 都要重复一遍！详情在这里：[https://support.mozilla.org/en-US/kb/mixed-content-blocking-firefox#w_mixed-content-is-not-blocked-not-secure](https://support.mozilla.org/en-US/kb/mixed-content-blocking-firefox#w_mixed-content-is-not-blocked-not-secure)。请注意，为了获得允许混合内容的选项，您必须首先尝试从 Snap! 发出 HTTP 请求。（起初这让我感到困惑。）
## 建议

对于没有允许混合内容选项的 Safari 等浏览器，您必须运行 Snap！来自[http://extensions.snap.berkeley.edu/snap.html](http://extensions.snap.berkeley.edu/snap.html)。对于其他浏览器，这可能也是最简单的解决方案。

如果您打算做很多涉及 Snap 的工作！对 MicroBlocks 进行 HTTP 调用，但你也使用 Snap！对于其他事情，您可能想要坚持使用 Chrome 并为 Snap 启用不安全的内容！网站。

浏览器在不断变化，因此将来可能会出现针对此问题的其他解决方法。