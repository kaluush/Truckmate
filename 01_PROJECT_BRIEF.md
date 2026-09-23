# TruckMate / CabPilot — Project Brief

## Public brand
- **Product name:** CabPilot
- **Domain:** cabpilotapp.com
- **Repository/internal working history:** TruckMate may remain as the repo/project working name.

## Mission
TruckMate is a clean, simple mobile work companion for CDL drivers and owner-operators. It keeps the active load, essential documents, trip records, inspection records, mileage, detention evidence, and—when needed—business financial information organized in one place.

## Core promise
**Your trucking day in one place. Collect once, use everywhere.**

## Target users
1. Mileage-paid company drivers.
2. Percentage-paid drivers who need load financial visibility.
3. Owner-operators who need operational and business visibility.

Functional needs determine the tier, not job title.

## Problem
Drivers repeatedly search email, PDFs, phone photos, paperwork, notes, and settlements for information needed during the working day. TruckMate should remove that repetitive searching and reconstruction.

## V1 boundaries
TruckMate is not an ELD and should not attempt to replace one. It is not a carrier surveillance tool. It is not a large roadside-service marketplace. It is not tax/accounting software. V1 should favor high-frequency, easy-to-use workflows over broad feature count.

## Product principles
- Clean, easy, simple.
- Minimal taps and typing.
- Current Load Card is the operational center.
- Context-aware information: show what the driver needs next.
- Original documents remain accessible alongside extracted data.
- Privacy by default; sharing is user initiated.
- Full-product field testing before monetization gates are emphasized.

## Lean V1 operations layer
TruckMate includes a deliberately small admin/operations panel for subscriber support, tier controls, support/recovery cases, and basic system visibility. Admin tooling SHALL not become a second product and SHALL not expose private driver document contents.

## Current technical direction
Mobile app; Firebase Authentication; TruckMate-owned backend/business layer; Firestore or equivalent; Cloud Storage; Gemini API; push notifications; location/maps; in-app scanner using existing/native technology; subscriptions later; Gmail import later.

## Current phase
V1 requirements are owner-approved and frozen for UX/design. The final V1 additions are the unified Upcoming due/reminder module and the lean admin/operations panel. Public naming is resolved as **CabPilot** with **cabpilotapp.com**; any new feature idea now goes to V2 unless field evidence exposes a contradiction in an approved requirement.
