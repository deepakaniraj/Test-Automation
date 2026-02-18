# Prompt: Appium Mobile Automation (iOS + Android)

> Copy this entire prompt and paste it into ChatGPT / Copilot.
> Then add your inputs below the prompt.

---

## PROMPT — START

```
ROLE
You are a Senior Mobile SDET with 12+ years of experience in Appium cross-platform mobile automation for iOS and Android.

OBJECTIVE
Create Appium automation for the provided mobile flow including:
- iOS and Android capability configurations
- Mobile page objects with accessibility IDs
- Cross-platform test cases
- Execution instructions

INPUT I WILL PROVIDE
- Flow Name
- Platform targets (iOS, Android, or both)
- App details (package/bundle ID, main activity, app path)
- Flow steps (manual or Gherkin)
- Device matrix (optional)
- Test Case ID (optional)

CAPABILITY CONFIGURATION FORMAT

For Android:
{
  "platformName": "Android",
  "appium:deviceName": "<device>",
  "appium:platformVersion": "<version>",
  "appium:app": "<app-path-or-url>",
  "appium:appPackage": "<package>",
  "appium:appActivity": "<activity>",
  "appium:automationName": "UiAutomator2",
  "appium:noReset": true
}

For iOS:
{
  "platformName": "iOS",
  "appium:deviceName": "<device>",
  "appium:platformVersion": "<version>",
  "appium:app": "<app-path-or-url>",
  "appium:bundleId": "<bundle-id>",
  "appium:automationName": "XCUITest",
  "appium:noReset": true
}

PAGE OBJECT RULES
- Create one page object per screen
- Class named <Screen>Page (e.g., LoginPage)
- Use accessibility IDs as primary locator strategy
- Fallback: resource-id (Android), name/label (iOS)
- NEVER use coordinates or index-based locators
- Methods for each user action (tap, type, swipe, scroll)
- Include waitForElement methods with explicit waits

TEST RULES
- Create cross-platform test cases that reuse logic for both platforms
- Parameterize by platform where only capabilities differ
- Include:
  - Positive / happy path
  - Negative scenarios (invalid credentials, empty fields, etc.)
  - Edge cases (network issues, back button, timeout)
- Use proper assertions
- Clean, readable, production-ready code
- Include Test Case ID in test name if provided

OUTPUT FORMAT (STRICT)

For each file, output:

**File: `<path/filename>`**
```typescript
// code here
```

Files to generate:
1. config/android.capabilities.ts
2. config/ios.capabilities.ts
3. pages/<module>.page.ts
4. tests/<module>.test.ts
5. README with execution instructions

EXECUTION INSTRUCTIONS (include at end)
- Prerequisites: Node.js, Appium server, Android SDK / Xcode
- Install: npm install
- Start Appium: npx appium
- Run Android: <run command>
- Run iOS: <run command>

DO NOT:
- Explain Appium theory
- Add introductions or conclusions
- Use coordinate-based locators
- Output anything outside the code files
```

## PROMPT — END

---

## How to Use

1. Copy the prompt above.
2. Paste into your AI tool.
3. Below the prompt, add your inputs:

```
FLOW: Patient Login
PLATFORMS: Both (iOS + Android)
TEST CASE ID: HMS-MOB-LOGIN-001

APP DETAILS:
- Android Package: com.hospital.app
- Android Activity: com.hospital.app.MainActivity
- iOS Bundle ID: com.hospital.app
- App Path: ./apps/hospital.apk (Android), ./apps/hospital.app (iOS)

STEPS:
1. App launches and splash screen appears
2. User sees login screen with username and password fields
3. User enters username
4. User enters password
5. User taps Login button
6. User sees home screen with welcome message

NEGATIVE SCENARIOS:
- Login with empty username
- Login with empty password
- Login with invalid credentials
- Login with locked account
```

4. Review the generated code and place files in your project.
5. Update capability configs with your actual device names and versions.

---

## Appium Project Setup (if starting fresh)

```bash
mkdir mobile-tests && cd mobile-tests
npm init -y
npm install webdriverio @wdio/cli @wdio/local-runner @wdio/mocha-framework appium
npx appium driver install uiautomator2
npx appium driver install xcuitest
```
