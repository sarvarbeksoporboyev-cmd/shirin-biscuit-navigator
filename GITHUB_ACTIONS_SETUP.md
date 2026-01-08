# GitHub Actions APK Build Setup - Navigator App

This repository includes a GitHub Actions workflow that builds Android APK files for the Shirin Biscuit Navigator (Driver) app.

## Quick Start

1. **Add required secrets to GitHub**
2. **Trigger the workflow manually** from GitHub Actions tab
3. **Download your APK** from the workflow artifacts

## Setting Up GitHub Secrets

Go to your repository on GitHub: **Settings → Secrets and variables → Actions → New repository secret**

Direct link: [https://github.com/sarvarbeksoporboyev-cmd/shirin-biscuit-navigator/settings/secrets/actions](https://github.com/sarvarbeksoporboyev-cmd/shirin-biscuit-navigator/settings/secrets/actions)

### Required Secrets

#### 1. `FLEETBASE_HOST`
Your Fleetbase API host URL.
```
https://api.fleetbase.io
```

#### 2. `FLEETBASE_KEY`
**IMPORTANT**: This is the **Developers API Key** (NOT the Storefront key).

**Where to find it:**
- Go to Fleetbase Console → **Developers** tab → **API Keys**
- Copy the **Secret Key** (starts with `$2y$10$...`)

**Your key:**
```
$2y$10$tBRZALDA294JWgmMBlh6Y.fVkLat0yIWn3d/AI/7VxQSkE5c3UNhS
```

**Note:** Do NOT use the Storefront API key (`store_...`) for the Navigator app. That's only for the customer app.

#### 3. `GOOGLE_MAPS_API_KEY`
Google Maps API key with Maps SDK for Android enabled (same as storefront app).
```
AIzaSyDE1Tqnjnl5Dk4nj9Inv_La3tEOSSvZKJQ
```

#### 4. `GOOGLE_SERVICES_JSON_NAVIGATOR`
Your Firebase/Google Services configuration file for the Navigator app.

**Important:** This should be a **different** google-services.json than the storefront app, with package name `com.shirinbiscuit.navigator`.

**How to create:**
1. Go to [Firebase Console](https://console.firebase.google.com/)
2. Select your project (or create a new one)
3. Add a new Android app with package name: `com.shirinbiscuit.navigator`
4. Download the `google-services.json` file
5. Copy the entire content
6. Create a new secret named `GOOGLE_SERVICES_JSON_NAVIGATOR`
7. Paste the content as the value

### Optional Secrets

- `FACEBOOK_APP_ID` - Facebook App ID (defaults to '0')
- `FACEBOOK_CLIENT_TOKEN` - Facebook Client Token (defaults to '0')
- `GOOGLE_CLIENT_ID` - Google OAuth Client ID
- `GOOGLE_IOS_CLIENT_ID` - Google iOS Client ID

## Running the Workflow

### Manual Trigger

1. Go to [https://github.com/sarvarbeksoporboyev-cmd/shirin-biscuit-navigator/actions/workflows/build-apk.yml](https://github.com/sarvarbeksoporboyev-cmd/shirin-biscuit-navigator/actions/workflows/build-apk.yml)
2. Click **Run workflow** button (top right)
3. Choose build type:
   - **debug** - For testing (faster build, larger file)
   - **release** - For production (optimized, smaller file)
4. Click **Run workflow**

## Downloading Your APK

After the workflow completes (~25-30 minutes):

1. Go to the workflow run page
2. Scroll down to **Artifacts** section
3. Download the APK file:
   - Debug builds: `shirin-navigator-debug-{run_number}.zip`
   - Release builds: `shirin-navigator-release-{run_number}.zip`
4. Extract the ZIP file to get your APK

## About the Navigator App

The Navigator app is the **driver/delivery app** that works with Fleetbase. It's used by:
- Delivery drivers
- Fleet operators
- Logistics personnel

This is different from the Storefront app which is for customers.

## Key Differences from Storefront App

| Feature | Storefront App | Navigator App |
|---------|---------------|---------------|
| **Purpose** | Customer ordering | Driver/delivery management |
| **Package** | `com.shirinbiscuit.storefront` | `com.shirinbiscuit.navigator` |
| **Users** | Customers | Drivers/Fleet operators |
| **Firebase Project** | Can be same or different | Needs separate app registration |

## Troubleshooting

### "API credentials provided were not valid" error when logging in

This means the `FLEETBASE_KEY` is incorrect or not set properly.

**Solution:**
1. Go to Fleetbase Console → **Developers** tab → **API Keys**
2. Copy the **Secret Key** (NOT the Storefront key)
3. Add it to GitHub secrets as `FLEETBASE_KEY`
4. Rebuild the APK

**Common mistakes:**
- ❌ Using the Storefront API key (`store_...`) - This won't work for Navigator app
- ❌ Using a test/sandbox key instead of production key
- ❌ Key is disabled or expired in Fleetbase Console
- ✅ Use the Developers → API Keys → Secret Key

### Build fails with "google-services.json not found"
- Make sure you've added the `GOOGLE_SERVICES_JSON_NAVIGATOR` secret
- Verify it's a different file than the storefront app
- Check that the package name is `com.shirinbiscuit.navigator`

### Build fails with "Invalid Fleetbase key"
- Ensure `FLEETBASE_HOST` and `FLEETBASE_KEY` secrets are set
- The key must be from **Developers → API Keys** (not Storefront)

### APK won't install on device
- **Debug builds**: Enable "Install from unknown sources" on your Android device
- **Release builds**: You need to sign the APK with a keystore

## Build Times

- **Debug builds**: ~25-30 minutes
- **Release builds**: ~30-35 minutes

## Support

For issues with the build process:
1. Check workflow logs in GitHub Actions
2. Ensure all required secrets are set correctly
3. Verify your Firebase configuration is correct
