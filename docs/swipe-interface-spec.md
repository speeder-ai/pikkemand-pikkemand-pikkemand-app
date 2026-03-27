# Pikkemand Swipe Interface — Technical Spec

## Overview
Tinder-style card swiping interface for homeowners to browse and match with pre-vetted handymen.

**Live Demo:** https://pikkemand-pikkemand-pikkemand.speederai.app/app-demo

## Architecture

### State Management
```js
let deck = [...handymen];     // Remaining cards
let requestedCount = 0;       // Right-swipe count
let savedCount = 0;           // Star-save count
let activeCard = null;        // Top card DOM reference
let isDragging = false;       // Drag lock flag
```

### Swipe Detection Logic
- **Threshold:** 80px horizontal drag triggers a swipe action
- **Direction:** `currentX > 80` → Request (right); `currentX < -80` → Skip (left)
- **Rotation:** Cards tilt at `currentX * 0.08` degrees during drag
- **Feedback:** Opacity labels ('REQUEST ✓' / 'SKIP ✗') fade in proportional to drag distance
- **Tint:** Green/red overlay fades in during drag to reinforce direction

### Event Handling
- Uses **Pointer Events API** for unified mouse + touch support
- `pointerdown` → start drag, capture pointer
- `pointermove` → transform card in real-time
- `pointerup` / `pointercancel` → evaluate threshold, fly out or snap back
- Keyboard: ← skip, → request, ↑ save (desktop testing)

### Card Stack Rendering
- Maximum 3 cards rendered at once (performance optimization)
- CSS transforms create "peeking" cards behind the active card:
  - Card 2: `translateY(10px) scale(0.95)`
  - Card 3: `translateY(20px) scale(0.90)`
- After each swipe: top card removed, `renderDeck()` rebuilds from `deck[]`

### Animation Phases
1. **Drag:** Real-time transform, no CSS transition
2. **Fly-out:** `animating` class adds `transition: transform 0.35s cubic-bezier(0.25, 1, 0.5, 1)`
3. **Snap-back:** Same `animating` class, transform reset to `''`
4. **Save:** Float up + scale down (`translateY(-120px) scale(0.85)`)

## Handyman Profile Data Structure
```js
{
  id: number,
  name: string,           // Full name
  initials: string,       // 2-letter initials for avatar
  color: string,          // CSS gradient for avatar background
  rating: number,         // 0-5 star rating
  reviews: number,        // Total review count
  rate: number,           // Hourly rate in EUR
  distance: string,       // Distance string e.g. "2.3 km"
  jobs: number,           // Completed jobs count
  responseTime: string,   // e.g. "< 30 min"
  responseRate: string,   // e.g. "98%"
  specialties: string[],  // Array of 3 specialty tags
  tagTypes: string[],     // Color type per tag: 'primary'|'secondary'|'tertiary'
  bio: string,            // Short bio text
  available: boolean,     // Current availability status
}
```

## Mobile-First Design
- Max width: 430px (iPhone 15 Pro Max)
- Phone frame with dark background on desktop
- Touch targets: minimum 58px for action buttons
- `touch-action: none` + `user-select: none` on body to prevent scroll interference
- `user-scalable=no` to prevent pinch-zoom breaking the interface

## Next Steps for Full Integration

### Database
- [ ] Create `handymen` table in Supabase with profile fields
- [ ] Create `swipe_actions` table: `user_id`, `handyman_id`, `action` (request/skip/save), `timestamp`
- [ ] Create `job_postings` table for homeowner tasks
- [ ] Add geolocation query for distance-based sorting

### API Routes
- [ ] `GET /api/handymen?jobId=&lat=&lng=` — Returns filtered handymen for a job
- [ ] `POST /api/swipes` — Records swipe action (request/skip/save)
- [ ] `POST /api/requests` — Creates a booking request (on right-swipe)
- [ ] `GET /api/matches` — Returns mutual matches

### React Components (for Next.js integration)
- [ ] `<SwipeCard />` — Individual handyman card with drag handlers
- [ ] `<CardStack />` — Container managing deck state
- [ ] `<ActionBar />` — Skip/Request/Save buttons
- [ ] `<JobContextBar />` — Shows active job at top
- [ ] `<BottomNav />` — Navigation tabs

### Features to Add
- [ ] Real handyman photos (from Supabase Storage)
- [ ] Undo last swipe (30-second window)
- [ ] Super-like / priority request
- [ ] Handyman online/offline status via Supabase Realtime
- [ ] Match notification when handyman accepts request
- [ ] Filter modal (price range, specialties, max distance)
- [ ] Profile detail view (tap card = expand)
- [ ] Push notifications for matches
