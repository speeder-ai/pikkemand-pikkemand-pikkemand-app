# Pikkemand Prototypes

This directory contains interactive HTML prototypes for validating product concepts.

## app-demo

**Live URL:** https://pikkemand-pikkemand-pikkemand.speederai.app/app-demo

**Description:** Core swipe-matching interface prototype — a Tinder-style card swiping UI for browsing handyman profiles.

### Features
- 6 pre-populated handyman profile cards (Estonian/Nordic names)
- Drag-to-swipe interaction (mouse + touch)
- Left swipe = Skip, Right swipe = Request
- Action buttons: Skip (✕), Save (⭐), Request (♥)
- Top bar: Active job context ('Fix leaky kitchen faucet')
- Bottom nav: Home, My Tasks, Messages, Profile
- Phone frame presentation for demos
- Animated toasts: 'Request Sent!', 'Saved!', 'Skipped'
- Empty state with reset
- Keyboard shortcuts: ← skip, → request, ↑ save
- Mobile-first, max-width 390px, amber/amber color system

### Design Decisions
- Amber/orange primary (#F59E0B) signals warmth and trust
- Dark navy phone frame makes the UI pop in investor demos
- Colored avatars with initials (realistic names: Jüri, Mati, Kristjan, Annika, Toomas, Peeter)
- Match badges on specialty tags for plumbing-relevant skills
- 'Available now' vs response time indicators add realism
