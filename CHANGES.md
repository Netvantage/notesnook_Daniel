# Pro/Premium Validation Bypass Changes

This document describes all the changes made to bypass pro/premium validation checks in the Notesnook desktop client (and related components) to make all premium features freely available.

## Summary

All pro/premium feature validation checks have been removed or bypassed. The application now treats all users as having the highest tier subscription (BELIEVER) with unlimited access to all features.

## Files Modified

### 1. packages/common/src/utils/is-feature-available.ts

This is the **core file** that controls feature availability across all Notesnook applications.

#### Changes Made:

**`isFeatureAvailable()` function (lines ~475-490)**
- Changed to always return `isAllowed: true` for any feature check
- Always uses BELIEVER tier limits (highest tier) instead of checking user's actual subscription
- Returns empty error string since features are always allowed

**`getFeatureLimit()` function (lines ~492-497)**
- Changed to always return BELIEVER tier limits regardless of user's subscription

**`areFeaturesAvailable()` function (lines ~499-526)**
- Changed to always return `isAllowed: true` for all requested features
- Always uses BELIEVER tier limits
- Removed subscription plan checking logic

**`getUserPlan()` function (lines ~528-531)**
- Changed to always return `SubscriptionPlan.BELIEVER` regardless of actual user subscription

### 2. apps/web/src/hooks/use-is-user-premium.ts

This hook is used throughout the web application to check if a user has a premium subscription.

#### Changes Made:

**`isActiveSubscription()` function (lines ~23-26)**
- Changed to always return `true`
- Removed all subscription status checking logic

**`isUserSubscribed()` function (lines ~27-30)**
- Changed to always return `true`
- Removed all subscription plan and expiry checking logic

### 3. apps/mobile/app/services/premium.ts

This service handles premium status for the mobile application.

#### Changes Made:

**`get()` function (lines ~88-91)**
- Changed to always return `true`
- Removed subscription plan checking logic

### 4. extensions/web-clipper/src/stores/app-store.tsx

This store manages the web clipper extension's state, including user premium status.

#### Changes Made:

**`login()` async function (lines ~69-84)**
- After fetching user data, now forcibly sets `user.pro = true`
- Original user object is modified to always have premium status enabled

## Features Now Unlocked (Previously Pro-Only)

With these changes, all the following features are now freely available:

### Storage & Files
- **Unlimited storage** (was: 50MB/mo for free, 10GB/mo for Pro)
- **Maximum file size**: 5GB (was: 10MB for free, 1GB for Pro)
- **Full quality images** (was: Pro-only)

### Editor Features
- **Block-level note links** (was: Essential+)
- **Task lists** (was: Essential+)
- **Outline lists** (was: Essential+)
- **Callouts** (was: Essential+)
- **Markdown shortcuts** (was: Essential+)
- **Font ligatures** (was: Pro-only)
- **Custom toolbar preset** (was: Pro-only)

### Organization
- **Unlimited colors** (was: 7 for free)
- **Unlimited tags** (was: 50 for free)
- **Unlimited notebooks** (was: 50 for free)
- **Unlimited shortcuts** (was: 10 for free)
- **Default notebook & tag** (was: Pro-only)
- **Customizable sidebar** (was: Essential+)

### Reminders
- **Unlimited active reminders** (was: 10 for free)
- **Recurring reminders** (was: Essential+)

### Sync & Storage
- **Unlimited note versions** (was: 100 for free)
- **Full offline mode** (was: Essential+)
- **Sync controls** (was: Pro-only)

### Mobile-Only Features
- **Pin note in notification** (was: Pro-only)
- **Create note from notification drawer** (was: Pro-only)

### Settings
- **Default sidebar tab** (was: Pro-only)
- **Custom homepage** (was: Pro-only)
- **Disable trash cleanup** (was: Pro-only)

### Monographs
- **Links & embeds in monographs** (was: Essential+)
- **Monograph analytics** (was: Pro-only)

### Security
- **App lock** (was: Pro-only)
- **2FA via SMS** (was: Pro-only)
- **Notesnook Circle** (was: Essential+)

### Web Clipper
- **Screenshot mode** (was: Pro-only)
- **Complete with styles mode** (was: Pro-only)

## Technical Notes

1. **Subscription Tier Hierarchy**: The code uses BELIEVER tier (highest) as the default for all feature checks, ensuring maximum limits and capabilities.

2. **No Server-Side Validation**: These changes only affect client-side validation. Server-side features that require subscription verification (like actual storage quotas on Notesnook servers) may still apply their limits.

3. **Consistent Behavior**: All three platforms (Web, Desktop, Mobile) and the Web Clipper extension have been modified to ensure consistent premium access across all clients.

## Date of Changes

January 15, 2026
