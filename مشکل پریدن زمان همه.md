مشکل از این قرار است که وقتی پیامک رمز پویا میاد ، وقتی زمان یکی منقضی میشه، زمان بقیه هم میپره...
این مشکل رو من احتمال میدادم به خاطر `CountDownTimer` باشه . 
و همینطور هم بود. از هوش مصنوعی سؤال شد . و پاسخ این چنین بود.

> The issue is in your `OtpCodeCard` composable. When the countdown timer finishes, `onCodeExpired()` is called, which likely removes the item from your StateFlow list. However, the removal happens immediately for all expired codes, causing the entire list to disappear.

Here's the fix:

**Problem:** The `LaunchedEffect` in `OtpCodeCard` runs for each item independently, but when one expires, it affects the entire list state.

**Solution:** Handle expiration logic in your `MainActivity` or ViewModel instead of individual cards.

```kotlin
// In your ViewModel or MainActivity
private fun startExpirationTimer() {
    viewModelScope.launch {
        while (true) {
            delay(1000) // Check every second
            val currentTime = System.currentTimeMillis()
            val updatedCodes = _otpCodes.value.filter { code ->
                val elapsed = currentTime - code.sentTime
                elapsed < appSettings.expireTime
            }
            
            if (updatedCodes.size != _otpCodes.value.size) {
                _otpCodes.value = updatedCodes.toMutableList()
            }
        }
    }
}
```

**Update your `OtpCodeCard`:**

```kotlin
@Composable
internal fun OtpCodeCard(
    context: Context, 
    appSettings: App.Settings, 
    code: OtpCode
) {
    var consumedTime by remember { mutableLongStateOf(System.currentTimeMillis() past code.sentTime) }
    
    LaunchedEffect(code.sentTime) {
        while (consumedTime < appSettings.expireTime) {
            delay(1000)
            consumedTime = System.currentTimeMillis() past code.sentTime
        }
    }
    
    val remainingTime = appSettings.expireTime - consumedTime
    
    if (remainingTime > 0) {
        // Your existing Card UI code
        // Remove onCodeExpired parameter and callback
    }
}
```

**In your LazyColumn:**

```kotlin
LazyColumn {
    items(otpCodes) { code ->
        OtpCodeCard(context, appSettings, code)
    }
}
```

This way, expired codes are filtered out at the data source level rather than triggering callbacks from individual composables.