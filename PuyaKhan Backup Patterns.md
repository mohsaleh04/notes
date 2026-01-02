```kotlin
import java.util.regex.Pattern

/**
 * This function extracts a 4-to-8-digit OTP (One-Time Password) from a given message string.
 * It uses a comprehensive and flexible regex that is dynamically built from keyword lists,
 * making it highly accurate and easy to maintain. It intelligently handles complex messages
 * with multiple codes by finding all valid matches and returning the last one.
 *
 * @param message The SMS or message content to parse.
 * @return The extracted OTP as a String, or null if no OTP is found.
 */
fun extractOtp(message: String): String? {
    // --- Structured and Final Expanded Keyword Datasets for maximum accuracy ---

    val persianPositive = listOf(
        "رمز", "گذرواژه", "تأیید", "تایید", "فعال سازی", "فعالسازی", "ورود",
        "موقت", "دسترسی", "احراز هویت", "یکبار مصرف", "یک بار مصرف", "شناسایی", "امنیتی",
        "اعتبارسنجی", "احراز", "کلید", "کد فعال‌سازی", "رمز یک‌بار مصرف", "کد امنیتی"
    )
    val englishPositive = listOf(
        "password", "otp", "verification", "passcode", "pin", "auth", "login",
        "access", "token", "security code", "authentication", "one-time", "2fa",
        "key", "secret", "credential", "2-step", "mfa", "validation", "one time password",
        "2-factor", "multi-factor", "confirmation code"
    )
    
    val persianNegative = listOf(
        "سفارش", "پیگیری", "مرسوله", "محصول", "کالا", "فاکتور", "شبا", "بارکد",
        "تخفیف", "ملی", "اقتصادی", "پشتیبانی", "قرعه کشی", "شناسه", "پستی",
        "مشتری", "پرواز", "رزرو", "بلیت", "صورتحساب", "اعتبار", "بسته", "حواله",
        "بارنامه", "قرارداد", "رهگیری", "پرداخت", "شماره مشتری"
    )
    val englishNegative = listOf(
        "order", "tracking", "promo", "discount", "invoice", "support", "raffle",
        "ticket", "booking", "reference", "confirmation", "customer id", "flight", "shipment",
        "bill", "package", "credit", "contract", "id", "receipt", "order number", "tracking number",
        "zip code", "payment id", "reference number"
    )

    // Find all potential OTP codes (4-8 digits)
    val digitPattern = Pattern.compile("\\b(\\d{4,8})\\b")
    val digitMatcher = digitPattern.matcher(message)
    val allCodes = mutableListOf<CodeCandidate>()
    
    while (digitMatcher.find()) {
        val code = digitMatcher.group(1)
        val start = digitMatcher.start()
        val end = digitMatcher.end()
        allCodes.add(CodeCandidate(code, start, end))
    }
    
    if (allCodes.isEmpty()) {
        return null
    }
    
    // Filter codes based on context
    val validCodes = allCodes.filter { candidate ->
        isValidOtpContext(message, candidate, persianPositive, englishPositive, persianNegative, englishNegative)
    }
    
    // If no valid codes found, return null
    if (validCodes.isEmpty()) {
        return null
    }
    
    // If multiple valid codes, return the one with highest score
    return validCodes.maxByOrNull { candidate ->
        calculateContextScore(message, candidate, persianPositive, englishPositive, persianNegative, englishNegative)
    }?.code
}

/**
 * Check if a code candidate is in valid OTP context
 */
private fun isValidOtpContext(
    message: String,
    candidate: CodeCandidate,
    persianPositive: List<String>,
    englishPositive: List<String>,
    persianNegative: List<String>,
    englishNegative: List<String>
): Boolean {
    val contextWindow = 30
    val start = maxOf(0, candidate.start - contextWindow)
    val end = minOf(message.length, candidate.end + contextWindow)
    val context = message.substring(start, end)
    
    // Check for explicit negative keywords first
    val allNegativeKeywords = persianNegative + englishNegative
    for (negativeKeyword in allNegativeKeywords) {
        if (context.contains(negativeKeyword, ignoreCase = true)) {
            // A simple contains check can be wrong (e.g., "booking id").
            // We need a more robust check to see if the keyword is a whole word.
            val pattern = Pattern.compile("\\b${Pattern.quote(negativeKeyword)}\\b", Pattern.CASE_INSENSITIVE)
            if (pattern.matcher(context).find()) {
                 return false
            }
        }
    }
    
    // Check for positive keywords
    val allPositiveKeywords = persianPositive + englishPositive
    for (positiveKeyword in allPositiveKeywords) {
        if (context.contains(positiveKeyword, ignoreCase = true)) {
            return true
        }
    }
    
    // Check for ambiguous keywords "کد" and "code"
    val hasKod = context.contains("کد")
    val hasCode = Regex("\\bcode\\b", RegexOption.IGNORE_CASE).containsMatchIn(context)
    
    if (hasKod || hasCode) {
        // For ambiguous keywords, ensure they're not followed/preceded by negative terms
        
        // Check Persian "کد" patterns
        if (hasKod) {
            val kodPattern = Pattern.compile("کد\\s*(\\p{L}+)", Pattern.CASE_INSENSITIVE)
            val kodMatcher = kodPattern.matcher(context)
            while (kodMatcher.find()) {
                val followingWord = kodMatcher.group(1)
                if (persianNegative.any { it.contains(followingWord, ignoreCase = true) || followingWord.contains(it, ignoreCase = true) }) {
                    return false
                }
            }
        }
        
        // Check English "code" patterns
        if (hasCode) {
            val codePattern = Pattern.compile("(\\w+)\\s+code\\b", Pattern.CASE_INSENSITIVE)
            val codeMatcher = codePattern.matcher(context)
            while (codeMatcher.find()) {
                val precedingWord = codeMatcher.group(1)
                if (englishNegative.any { it.contains(precedingWord, ignoreCase = true) || precedingWord.contains(it, ignoreCase = true) }) {
                    return false
                }
            }
        }
        
        return true
    }
    
    return false
}

/**
 * Calculate context score for ranking multiple valid codes
 */
private fun calculateContextScore(
    message: String,
    candidate: CodeCandidate,
    persianPositive: List<String>,
    englishPositive: List<String>,
    persianNegative: List<String>,
    englishNegative: List<String>
): Int {
    val contextWindow = 50
    val start = maxOf(0, candidate.start - contextWindow)
    val end = minOf(message.length, candidate.end + contextWindow)
    val context = message.substring(start, end).lowercase()
    
    var score = 0
    
    // Add points for positive keywords
    for (keyword in persianPositive) {
        if (context.contains(keyword)) score += 3
    }
    
    for (keyword in englishPositive) {
        if (context.contains(keyword.lowercase())) score += 3
    }
    
    // Specific high-priority keywords
    val highPriorityKeywords = listOf("login", "verification", "تایید", "ورود", "رمز", "auth", "pin", "otp")
    for (keyword in highPriorityKeywords) {
        if (context.contains(keyword.lowercase())) score += 5
    }
    
    // Subtract heavily for negative keywords (this shouldn't happen if filtering works correctly)
    for (keyword in persianNegative + englishNegative) {
        if (context.contains(keyword.lowercase())) score -= 10
    }
    
    return score
}

/**
 * Data class to hold code candidate information
 */
private data class CodeCandidate(val code: String, val start: Int, val end: Int)

// --- Example Usage ---
fun main() {
    val messages = listOf(
        // --- Standard OTPs (should be found) ---
        "کد تایید شما: 123456",
        "رمز یکبار مصرف شما 9876 می باشد.",
        "Your access code is 778899",
        "556677 کد ورود شما به اپلیکیشن است.",
        "Your security code is 454545",
        "کد شناسایی شما 887766 است",
        "کد اعتبارسنجی: 112233",
        "Your 2FA code is 998877",

        // --- Non-OTPs (should be ignored) ---
        "کد سفارش شما : 231122 از خریدتان متشکریم.",
        "کد پیگیری مرسوله: 565656",
        "Your tracking code is T12345B.",
        "کد پشتیبانی شما 102030 است.",
        "شناسه رزرو شما 998877 است.",
        "Your booking reference is 112233.",
        "شماره قرارداد شما 456789 است.",
        
        // --- The tricky composite message (should now work correctly) ---
        "The order code: 12812211\nthe login code is : 818221",
        
        // --- Multiple کد words (should return null or the correct one) ---
        "کد مرسوله: 123456 کد تخفیف: 789012",
        "کد سفارش: 111111 کد پیگیری: 222222",
        "کد تخفیف: 132811\nکد تایید: 819911"
    )

    println("--- Running Final & Most Comprehensive Logic ---")
    for (sms in messages) {
        val otp = extractOtp(sms)
        println("Message: \"$sms\" -> OTP: ${otp ?: "Not Found"}")
    }
}

```


-----

```kotlin
import java.util.regex.Pattern
import kotlin.math.abs

// Data classes to hold found items and their positions
private data class FoundItem(val text: String, val index: Int)
private data class ScoredCode(val code: String, val score: Int)

/**
 * This function extracts a 4-to-8-digit OTP (One-Time Password) from a given message string.
 * It uses an advanced contextual analysis algorithm. Instead of a simple regex, it finds all
 * potential codes and all keywords, then intelligently associates each code with its nearest
 * keyword to determine its validity. This approach accurately handles complex messages
 * containing multiple different types of codes.
 *
 * @param message The SMS or message content to parse.
 * @return The extracted OTP as a String, or null if no valid OTP is found.
 */
fun extractOtp(message: String): String? {
    // --- Structured and Final Expanded Keyword Datasets for maximum accuracy ---
    val persianPositive = listOf(
        "رمز", "گذرواژه", "تأیید", "تایید", "فعال سازی", "فعالسازی", "ورود",
        "موقت", "دسترسی", "احراز هویت", "یکبار مصرف", "یک بار مصرف", "شناسایی", "امنیتی",
        "اعتبارسنجی", "احراز", "کلید", "کد فعال‌سازی", "رمز یک‌بار مصرف", "کد امنیتی"
    )
    val englishPositive = listOf(
        "password", "otp", "verification", "passcode", "pin", "auth", "login",
        "access", "token", "security code", "authentication", "one-time", "2fa",
        "key", "secret", "credential", "2-step", "mfa", "validation", "one time password",
        "2-factor", "multi-factor", "confirmation code"
    )
    
    val persianNegative = listOf(
        "سفارش", "پیگیری", "مرسوله", "محصول", "کالا", "فاکتور", "شبا", "بارکد",
        "تخفیف", "ملی", "اقتصادی", "پشتیبانی", "قرعه کشی", "شناسه", "پستی",
        "مشتری", "پرواز", "رزرو", "بلیت", "صورتحساب", "اعتبار", "بسته", "حواله",
        "بارنامه", "قرارداد", "رهگیری", "پرداخت", "شماره مشتری"
    )
    val englishNegative = listOf(
        "order", "tracking", "promo", "discount", "invoice", "support", "raffle",
        "ticket", "booking", "reference", "confirmation", "customer id", "flight", "shipment",
        "bill", "package", "credit", "contract", "id", "receipt", "order number", "tracking number",
        "zip code", "payment id", "reference number"
    )

    // 1. Find all potential numeric codes in the message
    val codeCandidates = mutableListOf<FoundItem>()
    Pattern.compile("\\b(\\d{4,8})\\b").matcher(message).results().forEach {
        codeCandidates.add(FoundItem(it.group(1), it.start(1)))
    }

    if (codeCandidates.isEmpty()) {
        return null
    }

    // 2. Find all positive and negative keywords
    val positiveKeywords = findKeywords(message, persianPositive + englishPositive)
    val negativeKeywords = findKeywords(message, persianNegative + englishNegative)

    // Handle ambiguous "کد" and "code"
    val ambiguousKeywords = findKeywords(message, listOf("کد", "code"))
    val allNegativeContextKeywords = negativeKeywords + findAmbiguousNegative(message, ambiguousKeywords, persianNegative, englishNegative)
    val allPositiveContextKeywords = positiveKeywords + findAmbiguousPositive(message, ambiguousKeywords, persianNegative, englishNegative)

    if (allPositiveContextKeywords.isEmpty()) {
        return null // No positive signal in the message
    }

    // 3. Score each code candidate based on its nearest keyword
    val scoredCandidates = codeCandidates.map { code ->
        val closestPositive = allPositiveContextKeywords.minByOrNull { abs(it.index - code.index) }
        val closestNegative = allNegativeContextKeywords.minByOrNull { abs(it.index - code.index) }

        val distToPositive = closestPositive?.let { abs(it.index - code.index) } ?: Int.MAX_VALUE
        val distToNegative = closestNegative?.let { abs(it.index - code.index) } ?: Int.MAX_VALUE

        // The code is valid only if its closest keyword is positive
        if (distToPositive <= distToNegative) {
            ScoredCode(code.text, distToPositive) // Lower distance (score) is better
        } else {
            ScoredCode(code.text, -1) // Invalid
        }
    }

    // 4. Return the valid code with the best (lowest distance) score
    return scoredCandidates.filter { it.score != -1 }.minByOrNull { it.score }?.code
}

// Helper function to find all occurrences of a list of keywords
private fun findKeywords(message: String, keywords: List<String>): List<FoundItem> {
    val found = mutableListOf<FoundItem>()
    keywords.forEach { keyword ->
        // Use regex for whole-word matching for English keywords
        val pattern = if (keyword.first().isLetter() && keyword.first() <= 'z') "\\b${Pattern.quote(keyword)}\\b" else Pattern.quote(keyword)
        Pattern.compile(pattern, Pattern.CASE_INSENSITIVE).matcher(message).results().forEach {
            found.add(FoundItem(keyword, it.start()))
        }
    }
    return found
}

// Helper to find ambiguous keywords that are likely negative
private fun findAmbiguousNegative(message: String, ambiguous: List<FoundItem>, pNeg: List<String>, eNeg: List<String>): List<FoundItem> {
    return ambiguous.filter {
        val contextAfter = message.substring(it.index, minOf(message.length, it.index + 25)).lowercase()
        val contextBefore = message.substring(maxOf(0, it.index - 25), it.index).lowercase()
        pNeg.any { neg -> contextAfter.contains(neg) } || eNeg.any { neg -> contextBefore.contains(neg) }
    }
}

// Helper to find ambiguous keywords that are likely positive
private fun findAmbiguousPositive(message: String, ambiguous: List<FoundItem>, pNeg: List<String>, eNeg: List<String>): List<FoundItem> {
    return ambiguous.filterNot {
        val contextAfter = message.substring(it.index, minOf(message.length, it.index + 25)).lowercase()
        val contextBefore = message.substring(maxOf(0, it.index - 25), it.index).lowercase()
        pNeg.any { neg -> contextAfter.contains(neg) } || eNeg.any { neg -> contextBefore.contains(neg) }
    }
}


// --- Example Usage ---
fun main() {
    val messages = listOf(
        // --- Standard OTPs (should be found) ---
        "کد تایید شما: 123456",
        "رمز یکبار مصرف شما 9876 می باشد.",
        "Your access code is 778899",
        "556677 کد ورود شما به اپلیکیشن است.",
        "Your security code is 454545",
        "کد شناسایی شما 887766 است",
        "کد اعتبارسنجی: 112233",
        "Your 2FA code is 998877",

        // --- Non-OTPs (should be ignored) ---
        "کد سفارش شما : 231122 از خریدتان متشکریم.",
        "کد پیگیری مرسوله: 565656",
        "Your tracking code is T12345B.",
        "کد پشتیبانی شما 102030 است.",
        "شناسه رزرو شما 998877 است.",
        "Your booking reference is 112233.",
        "شماره قرارداد شما 456789 است.",
        
        // --- The tricky composite message (should now work correctly) ---
        "The order code: 12812211 the login code is : 818221",
        
        // --- Multiple کد words (should now work correctly) ---
        "کد مرسوله: 123456 کد تخفیف: 789012",
        "کد سفارش: 111111 کد پیگیری: 222222",
        "کد تخفیف: 132811\nکد تأیید: 819911"
    )

    println("--- Running Rewritten Advanced Logic ---")
    for (sms in messages) {
        val otp = extractOtp(sms)
        println("Message: \"$sms\" -> OTP: ${otp ?: "Not Found"}")
    }
}

```

