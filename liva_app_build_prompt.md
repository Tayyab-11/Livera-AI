# Master Build Prompt: Liva AI Mobile App (React Native / Expo)

Use this complete, step-by-step specification to build the **Liva AI** liver health intelligence app using **React Native / Expo**. This app features a multi-step onboarding wizard, full email & social authentication, a data-rich home dashboard, a medical report uploader, progress timelines, and an AI-driven health insights profile tab.

---

## Table of Contents
1. **Architecture & Project Setup**
2. **Global Design System (Themes & Colors)**
3. **Reusable Layouts & Components**
4. **Phase 1: Onboarding Flow (3 Steps + Success)**
5. **Phase 2: Authentication Screens (Sign Up & Sign In)**
6. **Phase 3: Main App - Dashboard Screen (Home)**
7. **Phase 4: Main App - Upload Medical Report Screen**
8. **Phase 5: Main App - AI Report Analysis Detail Screen**
9. **Phase 6: Main App - Progress Timeline Screen (Analytics)**
10. **Phase 7: Main App - Profile, Insights & Settings Screen**
11. **State Management & Data Flow Guidelines**
12. **UX, A11y & Form Validation Requirements**

---

## 1. Architecture & Project Setup

- **Framework**: React Native with **Expo (TypeScript)**.
- **Routing**: **Expo Router (v3+)** using file-based routing.
- **Navigation Structure**:
  - `(auth)` group: Stack navigator for `sign-up` and `sign-in`.
  - `(onboarding)` group: Stack navigator for wizard screens (`personal-info`, `medical-history`, `lifestyle`, `success`).
  - `(main)` group: Tabs navigator layout containing:
    1. `home` (Dashboard Tab)
    2. `reports` (Upload & Reports list Tab)
    3. `analytics` (Progress Timeline Tab)
    4. `profile` (Insights & Settings Tab)
    - Plus a central elevated floating "+" button that triggers the `(main)/upload-report` screen as a modal or navigation push.
- **Styling**: Standard `StyleSheet.create` for high performance, consistency, and clean style structures.
- **Icons**: `@expo/vector-icons` (Ionicons, MaterialCommunityIcons, FontAwesome5) combined with system emojis for badge representations.
- **Dependencies**:
  - `expo-linear-gradient` for premium background gradients.
  - `react-native-safe-area-context` to safely handle notches, home indicators, and status bars.
  - `react-hook-form` or custom React hooks for form validations.
  - `react-native-svg` for custom illustrations and charts.

---

## 2. Global Design System (Themes & Colors)

### Color Palette
- **Primary Gradients**: Diagonal/linear blend from Deep Teal (`#1F5C56`) → Mid Teal-Green (`#2E9E82`) → Light Sky Blue/Cyan (`#6FC3D9`).
- **Primary Teal/Green Accent**: `#2E9E82` (Active tab colors, borders, primary CTAs).
- **Secondary Success Green**: `#10B981` (Normal statuses, positive metrics, Wins background/text).
- **Warning/Borderline Yellow**: `#F59E0B` (Warning badges, borderline metrics, risk factor highlights).
- **Danger Red**: `#EF4444` (High-risk indicators, priority labels, Sign Out text).
- **Light Blue Info BG**: `#E0F2FE` (For information blocks and areas to monitor).
- **Text Color (Primary)**: `#111827` (Near-Black for headings).
- **Text Color (Secondary)**: `#6B7280` (Muted Grey for subtitles and descriptions).
- **Divider/Border**: `#E2E5E9` (Light grey borders).
- **App Background**: `#F5F6F8` (Off-white canvas background).
- **Card Background**: `#FFFFFF` (100% white with subtle shadows).

### Border Radii & Dimensions
- **Form Card Radius**: `16px`
- **Main Sheet/Panel Top Corners**: `28px`
- **Buttons / Input Radius**: `16px`
- **Pill Height**: `56px`
- **Base Spacing**: `8px / 12px / 16px / 20px / 24px / 32px`

---

## 3. Reusable Layouts & Components

### A. Shared Onboarding & Auth Header Layout
All Onboarding and Auth screens share a persistent layout structure consisting of a **Fixed Gradient Hero Header** (`~330-350px`) and an overlapping **Scrollable White Content Panel** (`marginTop: -28px`, `borderTopLeftRadius: 28`, `borderTopRightRadius: 28`).

#### Live Illustration Subcomponents:
1. **Onboarding Graphics (`LiverGraphicIllustration`)**:
   - Custom SVG outline representing a stylized two-lobed liver shape in translucent white lines (`opacity: 0.4`).
   - Four orbiting labeled circular nodes connected by thin lines to labels: **ALT** (Top), **GGT** (Left), **BILI** (Right), and **FIB-4** (Bottom).
   - Small ascending bar-chart glyphs to the bottom-left and bottom-right.
2. **Auth Graphics (`AuthGraphicIllustration`)**:
   - Centered circular badge with a green wave-like cell element inside.
   - An overlay badge in the top-right reading `AI`.
   - Concentric green circular blur fields fading out.
   - Orbiting pill badges: **"• AI Ready"** (Top Right) and **"🛡️ Secure"** or **"🛡️ Low Risk"** (Bottom Left).

### B. Segmented Step Indicator
A horizontal indicator showing wizard progress:
- Consists of **3 steps** and **2 connectors** laid out horizontally:
  - Inactive step: Small grey dot.
  - Active step: Wide, teal pill-shaped bar.
  - Connectors: Thin horizontal grey lines linking the steps.
- **Visual Mapping**:
  - **Step 1 Active**: `[Teal Pill] —— [Grey Dot] —— [Grey Dot]`
  - **Step 2 Active**: `[Grey Dot] —— [Teal Pill] —— [Grey Dot]`
  - **Step 3 Active**: `[Grey Dot] —— [Grey Dot] —— [Teal Pill]`
- Centered indicator label below: `"Step X of 3"` (grey, small font).

### C. Gradient Button
- Gradient pill button (`LinearGradient`, background `#1F5C56` to `#2E9E82`, `height: 56px`, `borderRadius: 16px`).
- Centered white text, font size `16px`, bold. Can contain optional trailing arrow `→` or leading icons.

---

## 4. Phase 1: Onboarding Flow (3 Steps + Success)

### Step 1: Personal Info
- **Welcome Card (Top, scrollable, Step 1 only)**:
  - Square dark-teal icon containing a waving hand (👋).
  - Bold text: `"Welcome to Liva AI"` + Subtitle.
  - Wrapping badge row: `"👥 12K+ Users"`, `"🤖 AI-Powered"`, `"🔒 HIPAA Secure"`.
- **Step Header**: Eyebrow `• PERSONAL INFO`, Heading `"Tell us about yourself"`, Subheading `"Basic details to personalize your liver health profile."`.
- **Form**: Two-column layout:
  - **Age**: Cake icon (🎂), numeric input.
  - **Gender**: Person icon (👤), picker dropdown (Male, Female, Other, Prefer not to say).
  - **Height (cm)**: Ruler icon (📏), numeric input.
  - **Weight (kg)**: Scale icon (⚖️), numeric input.
- **CTAs**: `"Continue →"` Button + link `"Already have an account? Sign In"` + bottom security disclaimer card.

### Step 2: Medical History
- **Step Header**: Eyebrow `• MEDICAL HISTORY`, Heading `"Any medical history?"`, Subheading `"This helps us identify relevant risk factors for you."`.
- **Checkbox Cards**: Stacked selectable cards:
  - **Diabetes**: Droplet icon (🩸, red background badge) | Sub: `Type 1 or Type 2`.
  - **Hypertension**: Stethoscope icon (🩺, blue background badge) | Sub: `High blood pressure`.
  - **Previous Liver Disease**: Clinic icon (🏥, green background badge) | Sub: `Hepatitis, fatty liver, cirrhosis`.
  - **Family History**: DNA icon (🧬, purple background badge) | Sub: `Liver disease in family`.
  - *Selection Animation*: When checked, the border highlights in teal and the right-aligned square fills with a white checkmark.
- **CTA**: `"Continue →"`.

### Step 3: Lifestyle
- **Step Header**: Eyebrow `• LIFESTYLE`, Heading `"Your daily lifestyle"`, Subheading `"We'll use this to generate your first AI health insights."`.
- **Select Dropdowns** (icon-leading, title, chevron-down on right edge):
  - **Activity Level**: Runner icon (🏃) | Options: `Sedentary`, `Lightly Active`, `Moderately Active`, `Very Active`.
  - **Exercise Frequency**: Flexed bicep icon (💪) | Options: `Never`, `1–2x/week`, `3–4x/week`, `5+ times/week`.
  - **Alcohol Consumption**: Wine glass icon (🍷) | Options: `None`, `Occasional`, `Moderate`, `Frequent`.
  - **Smoking Status**: No-smoking icon (🚭) | Options: `Never smoked`, `Former smoker`, `Current smoker`.
- **CTA**: `"Create My Health Profile"` (displays spinner on press).

### Step 4: Success / Confirmation
- **Layout**: Centered white card over a full-bleed teal-to-cyan gradient background.
- **Content**:
  - Shadowed teal badge containing a bold checkmark (✔).
  - Heading: `"Profile Created!"`.
  - Body: `"Your personalized liver health dashboard is ready. Liva AI is now analyzing your profile."`
  - CTA Button: `"View My Dashboard →"` (replaces onboarding route with main tab routing using `router.replace()`).

---

## 5. Phase 2: Authentication Screens (Sign Up & Sign In)

### Screen A: Create Your Account (Sign Up)
- **Header**: Icon badge with shield checkmark icon + `"Liva AI"` + `"Your Personal Liver Health Intelligence"`. Below it, the `AuthGraphicIllustration` containing `"• AI Ready"` and `"🛡️ Secure"` badges.
- **Title**: `"Create Your Account"` + subtitle `"Start monitoring your liver health with AI-powered insights."`.
- **Form Inputs** (White sheet overlay):
  - **Full Name**: leading icon (👤), placeholder `"Sarah Johnson"`.
  - **Email Address**: leading icon (✉️), placeholder `"sarah@example.com"`.
  - **Password**: leading icon (🔒), trailing visibility eye icon (👁️‍🗨️).
  - **Confirm Password**: leading icon (🔒), trailing visibility eye icon (👁️‍🗨️).
  - **Checkbox**: `"I agree to the Terms of Service & Privacy Policy"` (Terms/Privacy colored in bold teal).
- **Feature Checklist Card**: Light green box with thin green border listing benefits:
  - `✔ AI Liver Analysis` | `✔ Health Score`
  - `✔ Progress Tracking` | `✔ Secure Storage`
- **CTAs**:
  - Gradient CTA: `Create My Health Profile` (with a user-plus / plus-in-circle icon on the left).
  - Social Buttons: Horizontal row with `"Google"` (Google color logo) and `"Apple"` (Apple black logo).
  - Bottom Link: `"Already have an account? Sign In"`.

### Screen B: Welcome Back (Sign In)
- **Header**: Same as Sign Up, but the `AuthGraphicIllustration` badges read `"• Score: 76"` and `"🛡️ Low Risk"`.
- **Title**: `"Welcome Back"` + subtitle `"Sign in to continue tracking your liver health journey."`.
- **Form Inputs**:
  - **Email Address**: leading icon (✉️), placeholder `"sarah@example.com"`.
  - **Password**: leading icon (🔒), trailing visibility eye icon, + `"Forgot Password?"` link on the bottom-right.
- **CTAs**:
  - Gradient CTA: `Sign In` (with leading sign-in arrow icon).
  - Social Row: Google and Apple buttons.
  - Bottom Link: `"Don't have an account? Create Account"`.

---

## 6. Phase 3: Main App - Dashboard Screen (Home)

### Top Navigation Bar
- Left: Logo icon + `"Liva AI"` + `"Liver Intelligence Platform"` subtitle.
- Right: Notification Bell icon (with red badge indicator) + circular user avatar image.

### Main Greeting & Liver Health Score Card
- Greeting: `"Good morning, Sarah"` under date label `"Monday, June 9 • Last sync 2h ago"`.
- **Health Score Card** (Premium teal gradient block):
  - Label: `"LIVER HEALTH SCORE / Based on 12 biomarkers - Q2 2026"`.
  - Top Right Badge: `"↑ +4 pts"` (translucent green background).
  - Circular Gauge: Centered circular progress ring showing score **`76`** out of 100. Below it, a status badge `"• Good"` (white text on a transparent green oval).
  - Bottom Stats row: `Fibrosis: F0–F1` | `Inflammation: Mild` (Mild highlighted in yellow) | `Steatosis: None`.

### Biomarkers Panel
- Header: `"BIOMARKERS"` + right-aligned link `"View all >"`.
- Horizontal scroll row of metric cards:
  - **ALT Card**: Flask icon (green bg) | Value: `38 U/L` | Change: `↘ ~0%` | Badge: `Normal` (green text, light green bg).
  - **AST Card**: Pipette icon (yellow bg) | Value: `52 U/L` | Change: `— 0%` | Badge: `Borderline` (yellow text, light yellow bg) with amber indicator line.
  - **Bilirubin Card**: Droplet icon (blue bg) | Value: `0.8 mg/dL` | Change: `↘ ~0%` | Badge: `Normal`.

### Quick Actions Grid
- Row 1:
  - **Upload Lab Report**: File/health icon, subtitle `"PDF, Image, or CSV"`, bottom-left arrow link.
  - **AI Clinical Insights**: Brain icon, subtitle `"3 new findings"`, bottom-left arrow link.
- Row 2:
  - **Scan & Imaging**: Folder icon, subtitle `"Ultrasound, MRI"`, bottom-left arrow link.
  - **Medication Log**: Pill icon, subtitle `"2 due today"`, bottom-left arrow link.

### AI Insights Feed
- Header: `"AI INSIGHTS"` + `"All findings >"`.
- Vertical Stack of cards:
  - **ALT Normalizing**: Green check badge, heading, badge `"Positive"`, description text, footer `"Updated 2 hours ago"`.
  - **GGT Elevation**: Yellow triangle warning badge, heading, badge `"Monitor"`, description text, footer `"Recommend physician review"`.
  - **FIB-4 Score Update**: Blue document badge, heading, badge `"Info"`, description text, footer link `"View trend"`.

### Upcoming Section
- Header: `"UPCOMING"`.
- Events:
  - **Hepatology Clinic Review**: Left date indicator block (`14 Jun`), title, doctor, badges (`"In 5 days"`, `"Lab prerequisite"`), trailing chevron.
  - **Fibroscan Imaging**: Left date indicator block (`21 Jun`), title, department, badge (`"In 12 days"`), trailing chevron.

### Tab Navigation Bar (Sticky Bottom)
- Icons: `Home` (Active, teal text & icon), `Reports` (Inactive), floating center plus button (`+` white icon inside dark teal elevated circle), `Trends` (Inactive), `Profile` (Inactive).

---

## 7. Phase 4: Main App - Upload Medical Report Screen

- **Header**: Standard Home page top nav bar (Liva AI logo, notifications, user avatar).
- **Title Block**: `"Upload Medical Report"` + subtitle `"Upload liver-related reports for AI-powered health insights."`.
- **Upload Dropzone Card**:
  - Dotted teal border card with light green gradient background.
  - Centered: Upload cloud button + `"Drag & drop or tap to upload"` + `"Secure, encrypted • HIPAA compliant"`.
  - Camera & File Actions Row:
    - **Scan with Camera**: Teal button with camera icon.
    - **Upload PDF**: Blue button with document icon.
    - **Upload Image**: Purple button with image icon.
  - Supported Badges Row: `"✔ Liver Function Tests"`, `"Blood Reports"`, `"Ultrasound"`.
- **AI Report Analysis Status Container**:
  - Deep teal background panel.
  - Heading: `"AI Report Analysis"` + status `"• Ready to analyze"` (green text).
  - Graphic representation: A hub-and-spoke system linking a center node to 4 outer nodes (Biomarkers, Trends, Health Score, Insights).
  - Description body: `"Receive liver health scores, biomarker analysis, trend tracking, and personalized health insights after upload."`
  - Badges row: `Health Score`, `Biomarker Analysis`, `Trend Tracking`, `AI Insights`.
- **Recent Reports Feed**:
  - Header: `"RECENT REPORTS"` + `"View all >"`.
  - **Report Item 1**: Green report badge | `"Liver Function Test"`, date, status badge `"✔ Analyzed"`, metadata `"Score: 76/100"`, trailing chevron.
  - **Report Item 2**: Blue report badge | `"Ultrasound Report"`, date, status badge `"✔ Completed"`, metadata `"F0-F1 Stage"`, trailing chevron.
  - **Report Item 3**: Orange report badge | `"Blood Panel Report"`, date, status badge `"🕒 Processing"`, trailing chevron.
- **Sticky CTA**: Bottom fixed-position gradient button `"Upload Report"` with upload icon.

---

## 8. Phase 5: Main App - AI Report Analysis Detail Screen

- **Top Navigation Bar**:
  - Left: Back arrow button.
  - Center: `"Liva AI"` brand logo + Date pill badge `"June 28, 2026"` (dark green text on light green bg).
  - Right: Notification Bell icon.
- **Title Block**: `"AI Report Analysis"` + subtitle `"Understand your liver health with AI-powered insights."`.
- **Liver Health Score Card**:
  - Teal-to-green gradient card.
  - Left: Large circular score gauge showing `"82 / 100"`.
  - Right: Status badge `"✔ Healthy"` + paragraph: `"Your liver health is stable based on your latest report. Keep maintaining healthy habits. ↑ 5 pts from last report"`.
- **Risk Level Indicator Slider**:
  - Horizontal bar spanning Green (Low) → Orange (Moderate) → Red (High).
  - Pointer/Slider thumb sitting in the green ("Low") zone.
  - Text labels below: `• Low`, `• Moderate`, `• High`.
- **Key Biomarkers List**:
  - Header: `"KEY BIOMARKERS"` + `"All >"`.
  - Biomarker items with detailed values:
    - **ALT**: Flask icon | value `52 U/L` | Normal limit label | yellow arrow indicator `"↑ 14%"` | green status `"Normal"`.
    - **AST**: Pipette icon | value `31 U/L` | Normal limit label | green arrow indicator `"↓ 8%"` | green status `"Normal"`.
    - **Bilirubin**: Droplet icon | value `0.8 mg/dL` | Normal limit label | `"Stable"` | green status `"Normal"`.
    - **Albumin**: Microscope icon | value `4.5 g/dL` | Normal limit label | green arrow indicator `"↑ 2%"` | green status `"Normal"`.
    - **GGT**: Beaker icon | value `28 U/L` | Normal limit label | `"Stable"` | green status `"Normal"`.
- **Interpretive Insights Panel ("What Does This Mean?")**:
  - Light blue informational container block.
  - Bullet 1: `ALT: 52 U/L` — "Your ALT level is within the normal range..." (Inside light green highlight card).
  - Bullet 2: `Albumin: 4.5 g/dL` — "Your albumin is healthy..." (Inside light blue highlight card).
- **AI Summary Card**:
  - Dark teal background card.
  - Header: `"AI Summary"` + status `"• Liva AI Analysis"`.
  - Body: `"Overall liver health appears stable. No significant abnormalities detected. ALT improved by 12% since your last test..."`.
  - Badges: `"🛡️ No Abnormalities"`, `"📈 Improving"`.
- **Biomarker Trends Grid ("What Changed Since Last Report?")**:
  - 2x2 grid displaying badges with delta percentages:
    - `ALT ↑ 14%` (yellow badge) | `AST ↓ 8%` (green badge)
    - `Bilirubin Stable` (grey badge) | `Albumin ↑ 2%` (green badge)

---

## 9. Phase 6: Main App - Progress Timeline Screen (Analytics Tab)

- **Header**: Standard header row (Back arrow, App Logo, Notification Bell, Avatar picture).
- **Title Block**: `"Progress Timeline"` + subtitle `"Track your liver health journey over time."`.
- **Time Range Selector Segment**:
  - A horizontal selector row: `Weekly`, `Monthly`, `Quarterly`, `Yearly`.
  - `Weekly` is active (white text on solid teal block). The other options are inactive (grey text on white background pill).
- **Health Score Trend Card**:
  - Teal-to-green gradient card.
  - Title: `"HEALTH SCORE TREND / Last 4 reports"` + top-right green trend badge `"↑ 18%"`.
  - Metric: **`85`** `/ 100 Best Score`.
  - Bottom Row: Chronological label axis (`Mar`, `Apr`, `May`, `Jun`) supporting a vector-based trend line graph tracking historical scores (e.g. `72 -> 78 -> 82 -> 85`).
- **Timeline Summary Banner**:
  - Light green block layout with graph icon on left.
  - Text: `"Liver health score improved by 10 points in the last 3 months. Biomarkers remain stable and within normal range."`.
- **Biomarker Trends Grid**:
  - Header: `"BIOMARKER TRENDS"` + `"All >"`.
  - Stacked cards representing trends:
    - **ALT**: Beaker/flask icon | value `52 U/L` | indicator `↑ 14%` (yellow text) | Sub-label `Prev: 45 U/L`.
    - **AST**: Pipette icon | value `31 U/L` | indicator `↓ 8%` (green text) | Sub-label `Prev: 34 U/L`.
    - **Bilirubin**: Droplet icon | value `0.8 mg/dL` | indicator `Stable` (grey text) | Sub-label `Prev: 0.8 mg/dL`.
    - **Albumin**: Microscope icon | value `4.5 g/dL` | indicator `↑ 2%` (green text) | Sub-label `Prev: 4.4 g/dL`.
    - **GGT (Horizontal Card)**: Beaker icon on left, value `28 U/L` & `Stable` on right. Sub-label: `Prev: 28 U/L`.
- **Health History Timeline Section**:
  - Header: `"HEALTH HISTORY"`.
  - Timeline items connected by a vertical line:
    - **JUNE 2026**:
      - Item 1: Star icon badge | `"Health Score: 82 → 85"`, `"Score improved by 3 points after latest report."` + green badge `"1 Best Score"`.
      - Item 2: Report file badge | `"Report Uploaded"`, `"Liver Function Test • June 28, 2026"` + status `"Analyzed ✔"`.
      - Item 3: Alert badge | `"Biomarker Changes"` + badge list: `ALT ↑ 14%`, `AST ↓ 8%`, `Bilirubin Stable`.
    - **MAY 2026**:
      - Item 1: Graph icon badge | `"Health Score: 78 → 82"`, `"Consistent improvement after lifestyle changes."`.
      - Item 2: Report file badge | `"Report Uploaded"`, `"Ultrasound Report • May 15, 2026"` + status `"Completed ✔"`.
- **Achievements Carousel / Grid**:
  - Header: `"ACHIEVEMENTS / 4 Earned"`.
  - 2x2 circular badges:
    - **First Report Uploaded** (medal icon, light green badge, `"Mar 2026"`).
    - **30 Days Tracked** (calendar icon, light cyan badge, `"Apr 2026"`).
    - **Score Improved** (upward arrow graph, light blue badge, `"May 2026"`).
    - **Consistent Monitor** (shield check icon, light green badge, `"Jun 2026"`).
- **Sticky CTA**: Gradient button `"View Latest Analysis"` with a leading brain icon.

---

## 10. Phase 7: Main App - Profile, Insights & Settings Screen (Profile Tab)

- **Header**: Standard header row (App Logo, Notification Bell, Avatar picture).
- **Title Block**: `"Your Health Insights"` + subtitle `"AI-powered recommendations based on your latest health trends."`.
- **Overall Health Score Card**:
  - Gradient container card.
  - Left: status badge `"• Good Progress"`, metric **`82`** `/ 100`, description `"Liver & metabolic indicators improving vs. previous assessments"`.
  - Right: Circular progress bar showing `"82%"`.
- **Positive Changes Section ("3 Wins")**:
  - Light green panel card with upward chart line icon.
  - Header: `"Positive Changes / 3 Wins"`.
  - Horizontal list of green progress lines:
    - `✔ Improved hydration levels` (with filled green progress bar)
    - `✔ Better sleep consistency` (with filled green progress bar)
    - `✔ Reduced inflammation markers` (with filled green progress bar)
- **Risk Factors Section ("2 Flagged")**:
  - Soft yellow panel card with warning icon.
  - Header: `"Risk Factors / 2 Flagged"`.
  - Rows:
    - `Elevated sugar intake` | Badge `"High"` (orange).
    - `Irregular physical activity` | Badge `"Moderate"` (yellow).
- **Areas to Monitor Section**:
  - Soft blue panel card with wave chart icon.
  - Header: `"Areas to Monitor / Track closely this week"`.
  - Rows:
    - `Sleep Duration` | `6.2h avg` (blue text)
    - `Weekly Exercise` | `2 / 4 sessions` (blue text)
    - `Liver Enzyme Trends` | `Stable` (green text)
- **AI Recommendations Section**:
  - Header: `"AI Recommendations / 4 Tips"`.
  - Interactive rows:
    - Tip 1: Runner icon | **Increase physical activity** (Badge `"Priority"` in red) | `"Aim for 30-min moderate exercise..."`.
    - Tip 2: Coffee icon | **Reduce sugary drinks** | `"Switch to water or herbal teas..."`.
    - Tip 3: Moon icon | **Improve sleep quality** | `"Target 7-8 hours per night..."`.
    - Tip 4: Scale icon | **Maintain healthy weight** | `"Reducing BMI by 1-2 points..."`.
  - Bottom Buttons Row:
    - Action 1: Gradient button `"View Plan"`.
    - Action 2: White button with teal border `"Learn More"`.
- **Account & Settings Panel**:
  - Header: `"ACCOUNT & PROFILE"`.
  - **User Bio Card**: Gradient card showing avatar, username `"Sarah Johnson"`, details `"Age 34 • Female"`, `"🎯 Goal: Improve Liver Health"`, and white outline edit pencil button.
  - **Settings Navigation List** (Each row has leading icon and trailing chevron `>`):
    1. `Personal Information` (person icon)
    2. `Health Goals` (target icon)
    3. `Connected Devices` (phone icon)
    4. `Notifications` (bell icon)
    5. `Privacy & Security` (shield icon)
    6. `Help & Support` (question icon)
  - **Logout CTA**: Bottom full-width white button with red border and text `"Sign Out"` (with red sign-out door icon).

---

## 11. State Management & Data Flow Guidelines

Accumulate user inputs and medical data using a global state solution (Zustand or React Context + `useReducer` combined with `AsyncStorage` for local cache persistence).

### Context Profiles Structure
```typescript
interface UserProfile {
  personalInfo: { age: number; gender: string; heightCm: number; weightKg: number };
  medicalHistory: { diabetes: boolean; hypertension: boolean; previousLiverDisease: boolean; familyHistory: boolean };
  lifestyle: { activityLevel: string; exerciseFrequency: string; alcoholConsumption: string; smokingStatus: string };
  accountInfo?: { fullName: string; email: string };
}

interface MedicalReport {
  id: string;
  reportType: 'Liver Function Test' | 'Ultrasound Report' | 'Blood Panel Report';
  date: string;
  status: 'Analyzed' | 'Completed' | 'Processing';
  score?: number;
  metadata?: string;
  biomarkers?: {
    alt: { value: number; change: string; status: 'Normal' | 'Borderline' | 'High' };
    ast: { value: number; change: string; status: 'Normal' | 'Borderline' | 'High' };
    bilirubin: { value: number; change: string; status: 'Normal' };
    albumin?: { value: number; change: string; status: 'Normal' };
    ggt?: { value: number; change: string; status: 'Normal' };
  };
}
```

### Mock Actions & API Flow
1. **Submit Onboarding**: Cache results, then navigate user to the Sign Up screen.
2. **Complete Sign Up**: Append `accountInfo` to onboarding profile, mock a user account creation API call (with 1.5s timeout and loading indicator), then redirect to Dashboard Success.
3. **Upload File Action**: Mock file selection from camera/picker, show progress loading spinner, add a new element to the `MedicalReport` array with status `Processing`, and automatically update it to `Analyzed` after 5 seconds to simulate backend AI ingestion.

---

## 12. UX, A11y & Form Validation Requirements

- **Smooth Keyboard Handling**: Implement `KeyboardAvoidingView` on form views. Wrap content panels in `ScrollView` with `keyboardShouldPersistTaps="handled"` so inputs remain responsive and buttons aren't obscured.
- **Form Validation**:
  - Sign Up / Sign In: Validate correct email format, matching password and confirm password fields, and force terms selection check before allowing button submission.
  - Show error labels in red `#EF4444` under validation failures.
- **Accessibility (A11y)**:
  - Set `accessibilityRole="button"` and `accessibilityLabel` on all interactive touchables.
  - Interactive selection cards must use `accessibilityRole="checkbox"` and pass the boolean checking to `accessibilityState={{ checked }}`.
  - Set `importantForAccessibility="no"` on purely visual background graphics to prevent screen-reader clutter.
