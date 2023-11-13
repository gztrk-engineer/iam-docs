<details>
<summary>Example `CustomTabs` Android implementation (Kotlin): </summary>

```kotlin
override fun onActivityResult(requestCode: Int, resultCode: Int, data: Intent?) {
   super.onActivityResult(requestCode, resultCode, data)
   if (requestCode == RESULT_CODE_CHROME_TABS) {
       if (resultCode == Activity.RESULT_CANCELED) {
           //handle user cancellation
       }
   }
}

private fun startCustomTabsActivity() {
   // Building the intent and starting the CustomTabsActivity
   val customTabIntent: Intent

   // Creating the custom tab intent builder and customizing the toolbar
   val customTabIntentBuilder = CustomTabsIntent.Builder()

   customTabIntentBuilder.apply {
       setUrlBarHidingEnabled(true)
   }

   customTabIntent = customTabIntentBuilder.build().intent
   customTabIntent.apply {
       // Note the updated base URL 
       data = Uri.parse("https://api.transmitsecurity.io/verify/app/" + [START_TOKEN])

       flags = Intent.FLAG_ACTIVITY_NO_HISTORY and
               Intent.FLAG_ACTIVITY_NEW_TASK and
               Intent.FLAG_ACTIVITY_SINGLE_TOP
   }

   startActivityForResult(customTabIntent, RESULT_CODE_CHROME_TABS)
```

</details>

<details>
<summary>Example `WebView` Android implementation (Kotlin): </summary>

```kotlin
webView.setWebViewClient(WebViewClient())

// Grant web view video capture permissions
webView.webChromeClient = object : WebChromeClient() {
    override fun onPermissionRequest(request: PermissionRequest) {
        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.LOLLIPOP) {
            for (r in request.resources) {
                if (r == PermissionRequest.RESOURCE_VIDEO_CAPTURE) {
                    request.grant(arrayOf(PermissionRequest.RESOURCE_VIDEO_CAPTURE))
                    break
                }
            }
        }
    }
}

val webSettings: WebSettings = webView.getSettings()
webSettings.javaScriptEnabled = true
webSettings.allowFileAccess = false
webSettings.mediaPlaybackRequiresUserGesture = false
// Note the updated base URL
webView.loadUrl("https://api.transmitsecurity.io/verify/app/" + [START_TOKEN]) 
```

</details>

<details>
<summary>Example `ASWebAuthenticationSession` IOS implementation (Swift): </summary>

```swift
private var authSession: ASWebAuthenticationSession?
    
@IBAction func startVerification() {
    let redirectUrl = "YOUR_REDIRECT_URL"
    let startToken = "START_TOKEN_FROM_CREATE_SESSION"
   	// Note the updated base URL 
    let verifyUrl = URL(string: "https://api.transmitsecurity.io/verify/app/\(startToken)")!
    let callBackScheme = URL(string: redirectUrl)!.scheme
   	 
    self.authSession = ASWebAuthenticationSession(
        url: verifyUrl,
        callbackURLScheme: callBackScheme,
        completionHandler: { [weak self] (callBack: URL?, error: Error?) in
            // obtain the `sessionId` and `state` from callBack
    })
   	 
    self.authSession?.presentationContextProvider = self
    self.authSession?.start()
}
    
// MARK: ASWebAuthenticationPresentationContextProviding
    
func presentationAnchor(for session: ASWebAuthenticationSession) -> ASPresentationAnchor {
    return topMostViewController ?? ASPresentationAnchor()
} 
```

</details>