WebView myWebView = (WebView) findViewById(R.id.webview);
myWebView.getSettings().setJavaScriptEnabled(true);
myWebView.loadUrl("file:///android_asset/index.html"); // 将 HTML 放入 asset 文件夹
