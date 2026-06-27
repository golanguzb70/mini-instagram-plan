# Mini Instagram — Feature Requirements Document (FRD)

**Project type:** Learning / pet project
**Status:** Draft v1
**Goal:** Build a photo-sharing social app to practice end-to-end software engineering.

---

## 1. Purpose & Learning Goals

The product goal is a simplified Instagram: users create accounts, post photos, follow each other, and interact through likes and comments via a feed.

The *real* goal is learning. This document is scoped so that each phase deliberately exercises a distinct set of engineering skills:

| Phase | Feature focus | What it teaches |
|-------|---------------|-----------------|
| 0 | Auth & accounts | Security, password hashing, sessions/tokens, validation |
| 1 | Posts & feed | File upload/storage, data modeling, pagination |
| 2 | Social graph | Many-to-many relations, query design, fan-out |
| 3 | Engagement | Notifications, search, indexing, caching |
| 4 | Stretch | Real-time, performance, scale, advanced UX |

Treat the document as a backlog, not a contract. Ship the smallest thing that works, then iterate.

---

## 2. Scope

### In scope
- Web application (responsive). Mobile-native is a stretch goal.
- Single-region deployment.
- Image posts only (video is out of scope for v1).

### Out of scope (for now)
- Payments, ads, monetization.
- Content moderation / ML ranking of the feed.
- Multi-language / i18n.
- Native mobile apps (iOS/Android).
- Verified accounts, business accounts.

Keeping scope tight is itself a skill — resist feature creep until the core loop works.

---

## 3. Personas & Roles

- **Guest** — unauthenticated visitor. Can view the landing/login/signup pages only.
- **Registered user** — the primary actor. Can do everything in the functional spec.

---

## 4. Functional Requirements

Requirements are written as user stories with acceptance criteria, because writing testable acceptance criteria is part of what you're practicing. Each story has an ID (e.g. `AUTH-1`) so you can reference it in commits, branches, and tickets.

### Phase 0 — Foundations (Auth & Accounts)

**AUTH-1: Sign up**
> As a guest, I want to create an account with email and password so I can use the app.

Acceptance criteria:
- Email must be unique and validated for format.
- Password must meet a minimum policy (e.g. 8+ chars); enforced client- and server-side.
- Passwords are stored hashed (bcrypt/argon2), never in plaintext.
- On success, the user is logged in and redirected to the feed.
- Duplicate email returns a clear, non-leaky error.

**AUTH-2: Log in / Log out**
> As a registered user, I want to log in and out so my session is secure.

Acceptance criteria:
- Valid credentials create a session (cookie/JWT — your choice; document the tradeoff).
- Invalid credentials show a generic "invalid email or password" message.
- Logout invalidates the session client-side (and server-side if using server sessions).
- Protected routes redirect guests to login.

**USER-1: Profile**
> As a user, I want a profile page showing my username, bio, avatar, and posts.

Acceptance criteria:
- Profile is reachable at a stable URL (e.g. `/u/:username`).
- Shows post count, follower count, following count (counts can be 0 in Phase 0).
- Owner sees an "Edit profile" affordance; visitors do not.

**USER-2: Edit profile**
> As a user, I want to edit my display name, bio, and avatar.

Acceptance criteria:
- Avatar upload accepts common image formats with a size limit.
- Changes persist and are reflected immediately.

### Phase 1 — Core MVP (Posts & Feed)

**POST-1: Create post**
> As a user, I want to upload a photo with a caption.

Acceptance criteria:
- Accepts JPEG/PNG/WebP up to a defined max size (e.g. 5 MB).
- Image is stored in object storage (local disk for dev is fine; S3-style for prod).
- A thumbnail/resized version is generated (teaches image processing).
- Caption is optional, length-limited, and sanitized.
- Post appears on the author's profile immediately.

**POST-2: View a post**
> As a user, I want to open a single post and see its image, caption, author, and timestamp.

Acceptance criteria:
- Post has a permalink (e.g. `/p/:postId`).
- Shows relative time ("3h ago") and absolute time on hover.

**POST-3: Delete post**
> As a user, I want to delete my own post.

Acceptance criteria:
- Only the author can delete; others get 403.
- Deletion removes it from feeds and profile (soft-delete vs hard-delete is a design decision — document it).

**FEED-1: Home feed (basic)**
> As a user, I want a feed of recent posts so I have something to scroll.

Acceptance criteria:
- For MVP, the feed can show all recent posts (global feed); personalization comes in Phase 2.
- Feed is paginated (cursor-based preferred over offset — note why).
- Posts are ordered newest-first.

**LIKE-1: Like / unlike**
> As a user, I want to like and unlike a post.

Acceptance criteria:
- A user can like a post at most once (enforced by a unique constraint).
- Like count updates without a full page reload.
- Toggling is idempotent.

### Phase 2 — Social Graph

**FOLLOW-1: Follow / unfollow**
> As a user, I want to follow and unfollow other users.

Acceptance criteria:
- A user cannot follow themselves.
- Follow state is reflected on the profile button (Follow ↔ Following).
- Follower/following counts update accordingly.

**FEED-2: Personalized feed**
> As a user, I want my home feed to show posts only from people I follow.

Acceptance criteria:
- Feed = posts authored by followed users (and optionally self), newest-first, paginated.
- Empty state when the user follows no one, with a prompt to discover people.
- (Discussion point: read-time fan-in vs write-time fan-out. Start with fan-in; note the tradeoff.)

**COMMENT-1: Comment on a post**
> As a user, I want to comment on posts.

Acceptance criteria:
- Comments are length-limited and sanitized.
- Comments show author, text, and timestamp, oldest-first or newest-first (pick and justify).
- Comment count shown on the post.

**COMMENT-2: Delete comment**
> As a user, I want to delete my own comments; post authors can delete any comment on their post.

Acceptance criteria:
- Authorization enforced server-side.

### Phase 3 — Engagement & Discovery

**NOTIF-1: Notifications**
> As a user, I want to be notified when someone follows me, likes, or comments on my post.

Acceptance criteria:
- Notifications list with read/unread state.
- Unread badge/count in the nav.
- (Stretch: real-time delivery via WebSockets/SSE; MVP can poll.)

**SEARCH-1: Search users**
> As a user, I want to search for other users by username.

Acceptance criteria:
- Prefix/substring match on username.
- Results paginated; teaches indexing.

**TAG-1: Hashtags (optional)**
> As a user, I want captions with #hashtags to be browsable.

Acceptance criteria:
- Hashtags parsed from captions and linked.
- A hashtag page lists posts using that tag.

---

## 5. Non-Functional Requirements

- **Security:** hashed passwords; input validation everywhere; protection against XSS, CSRF, SQL injection; authz checks on every mutating endpoint; rate limiting on auth.
- **Performance:** feed should return a page in under ~300 ms with seed data; images served resized; pagination instead of loading everything.
- **Reliability:** graceful error handling; no unhandled 500s leaking stack traces.
- **Usability:** responsive layout; clear loading and empty states.
- **Observability (learning bonus):** structured logging; a basic health-check endpoint.
- **Testing:** unit tests for core logic; integration tests for key flows (signup, post, like, follow). Aim for meaningful coverage, not a number.