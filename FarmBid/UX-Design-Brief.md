# FarmBid – UI/UX Design Brief

**Project Name:** FarmBid **Package Name:** `com.buildstack.farmbid` **Platform:** Android Mobile Application **Design Tool:** Google Stitch **Document Version:** 1.0 **Status:** MVP **Last Updated:** August 30, 2026

---

# 1. Purpose

This document defines the UI/UX requirements for the FarmBid Android application.

FarmBid is an agricultural marketplace connecting:

- **Farmers** who list harvested agricultural products.
- **Buyers** who purchase products in bulk through a bidding system.

The main UI/UX objective is to make the application simple, clear, and easy to use for users who may not be highly familiar with complex digital marketplaces.

---

# 2. Mandatory Design Tool

## Strict Requirement

**All FarmBid UI/UX design work must be created using Google Stitch.**

Google Stitch will be used for:

- UI screen generation.
- Screen layouts.
- User flows.
- Component design.
- Visual hierarchy.
- Mobile application interface concepts.
- Design iteration.
- UI improvement.

### Do not use alternative AI design tools as the primary design tool.

The design workflow is:

```text id="gyyw3h"
PRODUCT REQUIREMENT
        ↓
UI/UX BRIEF
        ↓
GOOGLE STITCH
        ↓
GENERATE UI DESIGN
        ↓
REVIEW DESIGN
        ↓
REFINE DESIGN
        ↓
APPROVE SCREEN
        ↓
IMPLEMENT IN KOTLIN
```

---

# 3. Design Goal

FarmBid should feel like:

> **A modern, trustworthy, simple agricultural marketplace with a strong focus on product bidding.**

The design must prioritize:

1. Simplicity.
2. Readability.
3. Clear navigation.
4. Large touch targets.
5. Easy product browsing.
6. Clear bid comparison.
7. Minimal unnecessary features.

The application should not look like a complicated enterprise agriculture dashboard.

---

# 4. Target Users

## Farmer

Primary activities:

- Add products.
- Upload product images.
- Manage inventory.
- View bids.
- Compare buyer offers.
- Accept or reject bids.
- Manage deals.
- Communicate with buyers.

---

## Buyer

Primary activities:

- Browse products.
- Search products.
- View product information.
- View quantity.
- Check base price.
- Submit a bid.
- Track bid status.
- Manage deals.
- Communicate with farmers.

---

# 5. Design Principles

## 5.1 Simple First

Every screen should focus on one primary task.

Avoid:

- Too many buttons.
- Excessive information.
- Deep navigation.
- Complex forms.
- Small controls.

---

## 5.2 Mobile First

FarmBid is an Android mobile application.

The design must prioritize:

- One-handed usage.
- Thumb-friendly controls.
- Bottom navigation.
- Large buttons.
- Vertical scrolling.
- Clear action placement.

---

## 5.3 Clear Status Communication

Users must immediately understand the status of:

- Products.
- Bids.
- Deals.

Examples:

```text id="7x83f9"
AVAILABLE
PENDING
ACCEPTED
REJECTED
ACTIVE DEAL
COMPLETED
```

Status should be displayed using clear text and visual indicators.

Do not rely only on color.

---

## 5.4 Minimal User Interface

The application should not feel overloaded.

Avoid unnecessary:

- Charts.
- Dashboards.
- Statistics.
- Advanced analytics.
- Complex menus.

---

# 6. Visual Direction

## Overall Style

FarmBid should use:

- Clean.
- Modern.
- Minimal.
- Friendly.
- Agricultural.
- Professional.

The design should combine:

```text id="dtrbzw"
AGRICULTURE
+
MARKETPLACE
+
BIDDING
```

---

# 7. Color System

The primary visual direction should use agriculture-inspired colors.

## Primary Color

A natural green should be used as the primary brand color.

Primary green is used for:

- Primary buttons.
- Active navigation.
- Important actions.
- Selected states.
- Positive status indicators.

---

## Secondary Color

A warm earth or yellow tone may be used for:

- Highlights.
- Agricultural accents.
- Secondary actions.

---

## Background

Use a clean neutral background:

```text id="rj2n6e"
Light background
White cards
Subtle borders
```

The interface should maintain high readability.

---

# 8. Typography

Typography must prioritize readability.

Use:

- Large headings.
- Clear product names.
- Readable prices.
- High-contrast body text.
- Adequate line spacing.

Important information should follow:

```text id="0i3n3n"
PRODUCT NAME
        ↓
PRICE
        ↓
QUANTITY
        ↓
LOCATION
        ↓
ACTION
```

---

# 9. Main Navigation

The application should use bottom navigation.

## Farmer Navigation

```text id="g3wq2f"
Home
Products
Bids
Deals
Profile
```

---

## Buyer Navigation

```text id="l9nxly"
Home
Search
My Bids
Deals
Profile
```

The active role determines which navigation options are displayed.

---

# 10. Common UI Components

Google Stitch designs should establish reusable components.

Required components:

- Primary button.
- Secondary button.
- Outline button.
- Text button.
- Product card.
- Bid card.
- Deal card.
- Status badge.
- Search bar.
- Text input.
- Dropdown/select.
- Image uploader.
- Quantity selector.
- Empty state.
- Loading state.
- Error state.
- Confirmation dialog.
- Bottom sheet.

---

# 11. Screen Requirements

---

# 11.1 Splash Screen

### Purpose

Introduce FarmBid while checking authentication state.

### Content

- FarmBid logo.
- App name.
- Short loading indicator.

### UX Rule

The splash screen should be simple and short.

---

# 11.2 Onboarding

The onboarding should explain the core concept.

## Screen 1

```text id="e1gix7"
LIST YOUR HARVEST

Sell your agricultural products
directly to buyers.
```

---

## Screen 2

```text id="il97lt"
PLACE YOUR BID

Buyers can place competitive
offers on available products.
```

---

## Screen 3

```text id="9sxhjr"
MAKE A DEAL

Farmers select the best offer
and connect with buyers.
```

Actions:

- Skip.
- Next.
- Get Started.

---

# 11.3 Login Screen

### Components

- FarmBid logo.
- Welcome heading.
- Email field.
- Password field.
- Login button.
- Google Sign-In.
- Forgot password.
- Sign-up navigation.

### Primary Action

```text id="inry8c"
LOG IN
```

---

# 11.4 Sign-Up Screen

### Components

- Full name.
- Email.
- Password.
- Confirm password.
- Create account button.
- Google Sign-Up.
- Login navigation.

---

# 11.5 Role Selection

After authentication, the user chooses one role.

### Options

```text id="7enflq"
I AM A FARMER
```

```text id="8npqv2"
I AM A BUYER
```

Each role should use a large selectable card.

The selected card must have a strong visual selection state.

---

# 11.6 Profile Setup

### Required Information

- Profile image.
- Full name.
- Phone number.
- Location.
- Language.

### Location

The user should be able to:

```text id="x9kr2w"
Use Current Location
OR
Enter Location Manually
```

---

# 12. Farmer Home Screen

The farmer home should prioritize:

```text id="en4hmx"
PRODUCTS
BIDS
DEALS
```

## Layout

```text id="m0o2vx"
Header
│
├── Greeting
├── Profile Avatar
│
├── Quick Actions
│     ├── Add Product
│     └── View Bids
│
├── Active Products
│
├── Recent Bids
│
└── Active Deals
```

### Primary Action

```text id="7zw4yr"
+ ADD PRODUCT
```

This should be highly visible.

---

# 13. Buyer Home Screen

The buyer home should prioritize product discovery.

## Layout

```text id="hqctm3"
Header
│
├── Greeting
│
├── Search Bar
│
├── Product Categories
│
├── Latest Products
│
└── Nearby Products
```

### Primary Action

```text id="1iglk9"
SEARCH PRODUCTS
```

---

# 14. Product Card

The product card is one of the most important components.

Each card should display:

```text id="5sc7gm"
┌──────────────────────────┐
│                          │
│      PRODUCT IMAGE       │
│                          │
├──────────────────────────┤
│ Wheat                    │
│ 20 KG                    │
│ Base Price: ₹30 / KG     │
│ 📍 Jamshedpur            │
│                          │
│ [ VIEW PRODUCT ]         │
└──────────────────────────┘
```

### Required Information

- Main image.
- Product name.
- Available quantity.
- Base price.
- Location.
- Listing status.

---

# 15. Product Details Screen

The product details screen must clearly communicate all product information.

## Layout

```text id="g5ahxu"
Image Gallery
│
├── Product Name
├── Available Quantity
├── Base Price
├── Quality
├── Harvest Date
├── Location
│
├── Farmer Information
│
└── [ PLACE BID ]
```

The primary buyer action is:

```text id="p2a6bj"
PLACE BID
```

---

# 16. Add Product Screen

Only available to farmers.

The screen should be divided into clear sections.

## Section 1: Product Information

- Product name.
- Quantity.
- Unit.
- Base price.

---

## Section 2: Product Images

```text id="5c32a1"
UPLOAD 2 TO 4 IMAGES
```

The interface should clearly display:

```text id="53qfd6"
2 / 4 Images Uploaded
```

---

## Section 3: Product Details

- Harvest date.
- Quality.
- Location.

---

## Action

```text id="qzeyfr"
PUBLISH PRODUCT
```

---

# 17. My Products Screen

The farmer can view all products.

Tabs:

```text id="6spcce"
AVAILABLE
IN DEAL
SOLD
```

Each product card should provide:

- Product image.
- Product name.
- Quantity.
- Base price.
- Status.
- Number of bids.

---

# 18. Place Bid Screen

This screen must make the bidding process clear.

## Display

```text id="rmhxkr"
Product: Wheat
Available: 20 KG
Base Price: ₹30 / KG
```

## Input

- Offer price.
- Requested quantity.

## Bid Summary

```text id="kl66a1"
Your Offer

Price: ₹35 / KG
Quantity: 20 KG
Total Offer: ₹700
```

### Primary Action

```text id="sowd6j"
SUBMIT BID
```

---

# 19. Bid Confirmation

After placing a bid:

```text id="1y0p7n"
BID SUBMITTED SUCCESSFULLY

The farmer will review your offer.
```

Actions:

```text id="mghw6x"
VIEW MY BIDS
BACK TO MARKETPLACE
```

---

# 20. Farmer Bid Management

This is the most important screen in FarmBid.

The farmer needs to compare bids easily.

## Bid Card

```text id="vgne8g"
Buyer Name

Offer Price
₹35 / KG

Requested Quantity
20 KG

Total Offer
₹700

[ ACCEPT BID ]
[ REJECT ]
```

---

## Bid Sorting

Recommended sorting:

```text id="kvq4fd"
Highest Price
Lowest Price
Newest
Oldest
```

The highest price can be visually highlighted.

However, the farmer is free to choose any bid.

---

# 21. Bid Comparison Screen

For products with multiple bids.

## Layout

```text id="z3k2e9"
PRODUCT: WHEAT
AVAILABLE: 20 KG

BID 1
₹35 / KG
20 KG
₹700

BID 2
₹38 / KG
15 KG
₹570

BID 3
₹32 / KG
20 KG
₹640
```

Each bid should have:

```text id="p45ghj"
VIEW BUYER
ACCEPT BID
REJECT BID
```

The UI should make price and quantity comparison extremely clear.

---

# 22. Accept Bid Confirmation

Before accepting:

```text id="nznf49"
ACCEPT THIS BID?

Price: ₹35 / KG
Quantity: 20 KG
Buyer: Rahul Kumar
```

Actions:

```text id="k2pkj2"
CANCEL
ACCEPT BID
```

A confirmation is required because accepting a bid creates a deal and closes other bids.

---

# 23. My Bids Screen

Buyer view.

Tabs:

```text id="njpewf"
PENDING
ACCEPTED
REJECTED
```

Each bid card should show:

- Product name.
- Product image.
- Offered price.
- Quantity.
- Bid status.

---

# 24. Deal Details Screen

The deal screen is the connection point between farmer and buyer.

## Information

```text id="vlpuyh"
PRODUCT
BUYER / FARMER
FINAL PRICE
QUANTITY
DEAL STATUS
```

## Actions

```text id="46s2rf"
OPEN CHAT
VIEW CONTACT
MARK AS COMPLETED
```

---

# 25. Chat Screen

The MVP uses normal polling-based messaging.

## Layout

```text id="kv80z9"
Header
│
├── User Name
├── Deal/Product Name
│
├── Message List
│
└── Message Input
       [ Send ]
```

### UX Requirements

- Latest messages appear at the bottom.
- Manual refresh is supported.
- Pull-to-refresh can be used.
- Polling may refresh messages automatically.
- Sent and received messages must have clearly different alignment.

No advanced real-time indicators are required.

---

# 26. Deals Screen

Tabs:

```text id="z90cui"
ACTIVE
COMPLETED
CANCELLED
```

Each deal card displays:

- Product.
- Other user.
- Final price.
- Quantity.
- Status.

---

# 27. Profile Screen

The profile should display:

- Profile image.
- Full name.
- Role.
- Location.

Actions:

```text id="31uh7u"
EDIT PROFILE
CHANGE LANGUAGE
LOG OUT
```

---

# 28. Settings Screen

Required settings:

```text id="u57t9j"
Language
Location
Logout
```

Language options:

```text id="hftzxq"
English
हिन्दी
```

---

# 29. Loading States

Every major screen must have a loading state.

Avoid empty white screens.

Examples:

- Product loading.
- Bid loading.
- Chat loading.
- Profile loading.

Use skeleton loading where appropriate.

---

# 30. Empty States

Required empty states include:

```text id="dkdbn5"
NO PRODUCTS YET

You have not listed any products.
```

```text id="w67wpb"
NO BIDS YET

You have not received any bids.
```

```text id="ysjil9"
NO DEALS

Your active deals will appear here.
```

Empty states should provide a clear next action.

---

# 31. Error States

The application should clearly handle:

- No internet.
- Server error.
- Image upload failure.
- Authentication error.
- Invalid form data.
- Product not available.
- Bid no longer available.

Every error should provide:

```text id="txg6g0"
CLEAR MESSAGE
+
RETRY ACTION
```

---

# 32. Accessibility Requirements

The design must support:

- Readable font sizes.
- Large touch targets.
- High contrast.
- Text labels for icons.
- Status labels in addition to color.
- Clear error messages.

Minimum touch target should follow Android accessibility guidelines.

---

# 33. Google Stitch Screen Generation Workflow

For every FarmBid screen:

```text id="e22zj4"
DEFINE SCREEN PURPOSE
        ↓
CREATE SCREEN PROMPT
        ↓
GENERATE USING GOOGLE STITCH
        ↓
REVIEW RESULT
        ↓
CHECK AGAINST UI/UX BRIEF
        ↓
REFINE DESIGN
        ↓
APPROVE DESIGN
        ↓
IMPLEMENT IN JETPACK COMPOSE
```

---

# 34. Mandatory UI/UX Review Checklist

Before approving any screen:

```text id="ft17tq"
□ Screen has one clear primary purpose
□ Primary action is obvious
□ Text is readable
□ Buttons are large enough
□ Status is clear
□ Error states are considered
□ Loading state is considered
□ Empty state is considered
□ English/Hindi text can fit
□ Design matches FarmBid branding
□ Design was generated/refined using Google Stitch
```

---

# 35. Required Google Stitch Screen Order

Design screens in the following order:

```text id="f9ljiz"
PHASE 1
Splash
Onboarding
Login
Sign Up
Role Selection

PHASE 2
Profile Setup
Profile
Settings

PHASE 3
Farmer Home
Buyer Home
Bottom Navigation

PHASE 4
Marketplace
Product Details
Add Product
My Products

PHASE 5
Place Bid
My Bids
Received Bids
Bid Comparison

PHASE 6
Deal Details
Deals

PHASE 7
Chat
```

---

# 36. Final UX Principles

The FarmBid experience should be:

```text id="d2i0uy"
SIMPLE
CLEAR
MOBILE-FIRST
TRUSTWORTHY
BID-FOCUSED
```

The user should always understand:

```text id="8z8hct"
What product is this?
How much is available?
What is the price?
What is my current bid?
What action can I take?
What is the current status?
```

---

# 37. Final Design Requirement

Every major UI screen must be created and refined through **Google Stitch** before final implementation.

The implementation workflow is strictly:

```text id="2sjth9"
PRD
+
TRD
+
UI/UX BRIEF
        ↓
GOOGLE STITCH DESIGN
        ↓
DESIGN REVIEW
        ↓
JETPACK COMPOSE IMPLEMENTATION
        ↓
TEST
        ↓
FIX
        ↓
GRADLE BUILD
        ↓
GIT COMMIT
```

**Google Stitch is the mandatory UI/UX design tool for FarmBid.**
