# HRMS – HR Mobile (React Native)

A cross-platform mobile app (Android & iOS) for **[Frappe HRMS](https://frappe.io/hr)**, built with **Expo SDK 54** and **React Native 0.81**. Employees sign in to their company's Frappe site and handle day-to-day HR tasks — check-in/out, leaves, payslips, expense claims and approvals — from their phone.

---

## Features

| Area | What you can do |
| --- | --- |
| **Login** | Sign in with your Frappe site domain (e.g. `yourcompany.frappe.cloud`), email/username and password. The session is remembered between launches. |
| **Home / Check-in** | Punch **IN / OUT** with GPS location (creates an `Employee Checkin` with latitude/longitude), see your last log and current location. |
| **Shift** | View shift assignments and the shift roster. |
| **Attendance** | Browse attendance records on a calendar and raise **Attendance Requests**. |
| **Leave** | View leave balances (allocations) and apply for / track **Leave Applications**. |
| **Salary** | List **Salary Slips** and open/download the PDF slip. |
| **Expense Claim** | Create and track **Expense Claims** with expense line items. |
| **Announcement** | Read company announcements. |
| **Approvals** | Managers can review and approve/reject pending leave applications and expense claims. |
| **Profile** | View employee details from the `Employee` record. |
| **More** | Holidays list, notifications, settings and logout. |
| **Theme** | Light and dark mode (follows the system by default; switch it from the header menu). |

Forms are rendered dynamically from Frappe DocType metadata (`GenericField`, `LinkField`, `ChildTable`), so forms follow the DocType fields configured on the server.

---

## Tech Stack

- **Expo SDK 54**, React 19, React Native 0.81
- **React Navigation 7** – bottom tabs + stack navigators
- **axios / fetch** with cookie-based Frappe session auth
- **AsyncStorage** – stores site URL, user email, theme and a small API cache
- **expo-location**, **react-native-maps** – geo check-in
- **react-native-calendars**, **@react-native-community/datetimepicker** – dates
- **react-native-webview**, **expo-file-system**, **expo-sharing** – payslip PDF viewing/sharing
- **Formik** – forms, **lucide-react-native** – icons, **react-native-toast-message** – toasts

---

## Project Structure

```
.
├── App.js                     # App entry: providers, error boundary, toast
├── index.js                   # Expo root registration
├── app.json                   # Expo config (name, bundle IDs, permissions, EAS project)
├── eas.json                   # EAS build profiles
└── src/
    ├── RootNavigator.js
    ├── navigation/
    │   ├── AppContainer.js     # Restores session, switches Auth ⇄ App
    │   ├── AuthNavigator.js    # Login flow
    │   ├── AppNavigator.js     # Bottom tabs + stacks
    │   └── CreateSharedOptions.js
    ├── screens/                # Home, Attendance, Leaves, SalarySlip, ExpenseClaim,
    │                           # Approval, Announcement, Holidays, Notification,
    │                           # Profile, Settings, ShiftDetails, Login, Auth
    ├── Components/             # Loader, ErrorBoundary, ProfileAvatar, doctype modals
    │   └── FormComponents/     # GenericField, LinkField, ChildTable (metadata-driven forms)
    ├── context/ThemeContext.js # Light/dark theme
    ├── utils/
    │   ├── frappeApi.js        # Frappe REST helpers (login, getResourceList, saveDoc, …)
    │   ├── axiosInstance.js
    │   └── CacheManager.js     # AsyncStorage cache with expiry
    └── assests/                # Logos and login backgrounds
```

---

## Prerequisites

- **Node.js 18+** and npm
- **Expo Go** app on your phone (for quick testing), or Android Studio / Xcode for emulators
- **EAS CLI** for builds: `npm install -g eas-cli`
- A reachable **Frappe HRMS** site over **HTTPS**, and a user linked to an `Employee` record

---

## Getting Started

```bash
# 1. Install dependencies
npm install

# 2. Start the dev server
npx expo start

# 3. Run it
#    - Scan the QR code with Expo Go (Android) or the Camera app (iOS)
#    - or press  a  for Android emulator /  i  for iOS simulator
```

Other scripts:

```bash
npm run android   # expo start --android
npm run ios       # expo start --ios
npm run web       # expo start --web
```

> **Note:** `react-native-maps` and some native modules may need a development build instead of Expo Go: `npx expo run:android` / `npx expo run:ios`.

### Signing in

1. Enter your site domain **without** `https://` (e.g. `hr.yourcompany.com`) — the app adds `https://` automatically.
2. Enter your Frappe email/username and password.
3. Allow location access when prompted — it is required for check-in/out.

---

## Building with EAS

The EAS project is configured in `app.json → extra.eas`. Log in first with `eas login`.

| Profile | Command | Output |
| --- | --- | --- |
| `preview` | `eas build -p android --profile preview` | Android APK (internal) |
| `preview` | `eas build -p ios --profile preview` | iOS simulator build |
| `preview-device` | `eas build -p ios --profile preview-device` | iOS Ad Hoc build for registered devices |
| `production` | `eas build -p android --profile production` | Android APK |
| `production` | `eas build -p ios --profile production` | iOS App Store build |


---

## Permissions

| Permission | Why |
| --- | --- |
| Location (when in use) | Attach GPS coordinates to attendance check-in/out |

---

## Troubleshooting

- **Login fails** – check that the site is reachable over HTTPS in a browser and that the user is enabled and linked to an Employee.
- **"Does not have doctype access"** – the user's Frappe role lacks read permission for that DocType (e.g. Announcement, Expense Claim). Fix it in Role Permissions on the server.
- **Location timeout on check-in** – turn on GPS / precise location and try again outdoors or near a window.
- **Stale data** – pull down to refresh; cached data expires automatically.
- See `CRASH_ANALYSIS_REPORT.md` for notes on past crash fixes.

---

## License

0BSD
