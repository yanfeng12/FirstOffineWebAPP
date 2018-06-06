# 在网络应用中添加服务工作线程和离线功能
## 在网站上注册服务工作线程
可以将离线支持重新添加到应用中。这个过程由两个步骤组成：

1.创建一个将作为服务工作线程的 JavaScript 文件。
2.指示浏览器将此 JavaScript 文件注册为“服务工作线程”。

注意index.html的底部
'''
<script>
if('serviceWorker' in navigator) {
  navigator.serviceWorker
           .register('/sw.js')
           .then(function() { console.log("Service Worker Registered"); });
}
</script>
'''
脚本会检查浏览器是否支持服务工作线程。如果不支持，它会将我们当前使用的空白文件 sw.js 注册为服务工作线程，然后记录到控制台。

sw.js中,注册服务工作线程后，当用户首次点击页面时，会触发 install 事件。此事件就是您要缓存页面资产的地方。
'''
self.addEventListener('install', e => {
  const timeStamp = Date.now();
  e.waitUntil(
    caches.open(cacheName).then(cache => {
      return cache.addAll([
        `/`,
        `/index.html`,
        `/styles/main.css`,
        `/scripts/main.min.js`,
        `/scripts/comlink.global.js`,
        `/scripts/messagechanneladapter.global.js`,
        `/scripts/pwacompat.min.js`,
        `/sounds/airhorn.mp3`
      ])
          .then(() => self.skipWaiting());
    })
  );
});
'''