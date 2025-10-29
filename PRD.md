# NexusPHP Product Requirements Document (PRD)

## Document Information
- **Project Name**: NexusPHP
- **Version**: 1.5 Beta 4
- **Last Updated**: 2025-10-29
- **Purpose**: Reverse-engineered product requirements document for future refactoring
- **Based on**: Release dated 2010-08-19

---

## 1. Executive Summary

### 1.1 Product Overview
NexusPHP is an open-source Private BitTorrent Tracker system written in PHP. It is designed to manage and facilitate file sharing within private communities through the BitTorrent protocol. The system provides comprehensive user management, torrent tracking, community features (forums, messaging), and administrative tools for maintaining a private tracker site.

### 1.2 Target Audience
- **Primary Users**: Members of private BitTorrent tracker communities
- **Administrators**: Site operators and moderators managing the tracker
- **Secondary Users**: VIP members, donors, uploaders, and special user classes

### 1.3 Key Value Propositions
- Complete private tracker management solution
- Integrated community features (forums, private messaging, shoutbox)
- Advanced ratio and bonus point system
- Multi-language support (English, Chinese Simplified, Chinese Traditional)
- Extensive customization and theming capabilities
- Built-in anti-cheat and security measures

---

## 2. System Architecture

### 2.1 Technology Stack

#### Backend
- **Language**: PHP 5.2.x / 5.3.x
- **Database**: MySQL 5.0.x or higher
- **Caching**: Memcached
- **Email**: SMTP (Postfix recommended)
- **Web Server**: Apache 2.2.x (IIS 6.0 supported but not recommended)

#### Required PHP Extensions
- mbstring (Multibyte String)
- mysql (MySQL connectivity)
- memcache (Caching)
- gd (Image processing)

#### Optional Dependencies
- PEAR with HTTP_Request2 package (for IMDb information scraping)

#### Frontend
- HTML/XHTML
- JavaScript (custom libraries: domLib.js, domTT.js, common.js, etc.)
- CSS (multiple stylesheet support)
- Flash (for media players: flvplayer.swf)

### 2.2 System Components

#### Core Modules
1. **Tracker Engine** (`announce.php`, `scrape.php`)
2. **User Management System** (`signup.php`, `login.php`, `usercp.php`)
3. **Torrent Management** (`torrents.php`, `upload.php`, `details.php`, `download.php`)
4. **Forum System** (`forums.php`)
5. **Private Messaging** (`messages.php`)
6. **Administration Panel** (various admin*.php files)
7. **Bonus System** (`mybonus.php`)
8. **Statistics & Reporting** (`stats.php`, `topten.php`)

#### Supporting Features
- Attachment system
- Advertisement management
- Subtitle system
- IMDb integration
- RSS feeds
- Donation tracking
- Invitation system

---

## 3. Functional Requirements

### 3.1 User Management

#### 3.1.1 User Registration & Authentication
**FR-UM-001: User Registration**
- System shall support configurable registration modes:
  - Open registration
  - Invitation-only registration
  - Closed registration
- Users must provide: username, password, email
- Email verification required (configurable: automatic/manual)
- Registration timeout: configurable (default 259200 seconds / 3 days)

**FR-UM-002: User Authentication**
- Username/password-based login
- Passkey-based authentication for tracker
- Secure login option (HTTPS)
- Session management with cookies
- Maximum login attempts tracking (default: 10)
- Last login tracking

**FR-UM-003: Password Management**
- Password hash storage (MD5)
- Password recovery via email
- Password reset by administrators
- Secret key for password edits

#### 3.1.2 User Classes & Permissions
**FR-UM-004: User Class System**
The system shall support 18 user classes with hierarchical permissions:

**Regular User Classes (0-9):**
1. **Peasant (0)**: Entry-level, restricted access
2. **User (1)**: Basic member, standard access
3. **Power User (2)**: Enhanced privileges, can send invitations
4. **Elite User (3)**: Advanced user benefits
5. **Crazy User (4)**: Can be anonymous
6. **Insane User (5)**: Higher tier benefits
7. **Veteran User (6)**: Long-term member benefits
8. **Extreme User (7)**: Advanced features access
9. **Ultimate User (8)**: Near-maximum privileges
10. **Nexus Master (9)**: Highest regular user class

**Special User Classes (10-12):**
11. **VIP (10)**: Paid/special status members
12. **Retiree (11)**: Former staff members
13. **Uploader (12)**: Dedicated content uploaders

**Staff Classes (13-16):**
14. **Moderator (13)**: Content and user moderation
15. **Administrator (14)**: System configuration
16. **SysOp (15)**: Technical administration
17. **Staff Leader (16)**: Highest authority

**FR-UM-005: Class Promotion**
- Automatic promotion based on:
  - Upload amount (GB)
  - Download ratio
  - Time on site (weeks)
  - Account age
- Configurable thresholds per class
- Manual promotion by administrators

**FR-UM-006: User Privileges**
The system shall enforce class-based permissions for:
- Torrent upload (default: class ≥ 2)
- Forum posting (configurable per class)
- Private messaging
- Invitation sending (default: class ≥ 2)
- Viewing user lists (default: class ≥ 2)
- Viewing statistics (default: class ≥ 2)
- Anonymous posting (default: class ≥ 4)
- Administrative functions (class ≥ 13)

#### 3.1.3 User Profiles
**FR-UM-007: User Profile Information**
Users shall have profiles containing:
- Basic Info: username, email, country, gender, join date
- Statistics: uploaded, downloaded, ratio, seed time, leech time
- Preferences: stylesheet, language, timezone, font size
- Privacy settings: accept PMs, comment notifications, privacy level
- Custom: avatar, title, signature, info text
- Status: donor, VIP, warned, parked, banned

**FR-UM-008: User Preferences**
Users can configure:
- Site theme/stylesheet
- Language preference
- Torrents per page (default: 50)
- Topics per page (default: 20)
- Posts per page (default: 10)
- Show/hide various elements (avatars, signatures, hot movies, etc.)
- Forum click behavior (first/last page)
- Shoutbox settings (number of messages, refresh rate)

#### 3.1.4 User Status Management
**FR-UM-009: Account Status**
- **Pending**: Awaiting email confirmation
- **Confirmed**: Active account
- **Enabled/Disabled**: Account activation status
- **Parked**: Temporarily inactive (preserves account)
- **Warned**: Under warning (with expiration date)
- **Banned**: Access revoked

**FR-UM-010: Account Deletion Policy**
Configurable automatic deletion based on:
- Inactivity period (no transfer)
- Low ratio for peasant class
- Download thresholds and ratio requirements by class
- Protected classes (VIP, donors, staff) never deleted

### 3.2 Torrent Management

#### 3.2.1 Torrent Upload
**FR-TM-001: Upload Requirements**
- User must have upload privileges (class-based)
- Torrent file size limit (default: 1MB)
- Required fields:
  - Torrent file (.torrent)
  - Category
  - Name/title
  - Small description
  - Full description (BBCode supported)
- Optional fields:
  - Source, medium, codec, standard, processing, team, audio codec
  - IMDb URL
  - NFO file
  - Subtitle files
  - Anonymous upload option (for eligible users)

**FR-TM-002: Torrent Validation**
- Validate torrent file structure
- Check for duplicates (info_hash)
- Modify announce URL to tracker's URL
- Add tracker name prefix (configurable)
- Store in designated directory

**FR-TM-003: Torrent Categories**
- Hierarchical category system
- Categories with icons
- Subcategories: sources, media, codecs, standards, processings, teams, audiocodecs
- Configurable category visibility and permissions

#### 3.2.2 Torrent Browsing & Search
**FR-TM-004: Torrent Listing**
- Paginated torrent list (configurable items per page)
- Filter by:
  - Category/subcategory
  - Source, medium, codec, standard, processing, team, audiocodec
  - Special states (free, 2x upload, etc.)
  - Dead/alive
  - Bookmarked
- Sort by: name, size, seeders, leechers, time, views, comments

**FR-TM-005: Search Functionality**
- Search by torrent name
- Search in descriptions
- Advanced search with multiple criteria
- Search suggestions (AJAX-based)
- Tag-based search

**FR-TM-006: Torrent Details Page**
- Torrent information: name, category, size, files, upload date
- Seeder/leecher/completion statistics
- Uploader information (or anonymous)
- Description and NFO
- File list
- Peer list (for eligible users)
- Download history/snatches
- Comments
- Related subtitles
- IMDb information (if available)

#### 3.2.3 Torrent States & Promotions
**FR-TM-007: Promotion States**
The system shall support torrent promotion states:
1. **Normal**: Standard download/upload counting
2. **Free**: Download not counted, upload counted
3. **2X Upload**: Upload counted double
4. **2X Free**: Download free, upload double
5. **Half Leech**: Download counted at 50%
6. **2X Half Leech**: Download 50%, upload double
7. **30% Leech**: Download counted at 30%

**FR-TM-008: Promotion Management**
- Manual promotion by authorized users
- Automatic random promotion (configurable percentages)
- Time-based promotion expiration
- Size-based automatic promotion (large torrents)
- Global freeleech mode (all torrents)
- Per-torrent promotion timeout setting

**FR-TM-009: Special Torrent States**
- **Hot**: High seeder count within time period
- **Recommended**: Staff picks
- **Sticky**: Pinned to top of listings
- **Banned**: Hidden from regular users
- **Dead**: No seeders for extended period (configurable deletion)

#### 3.2.4 Torrent Download
**FR-TM-010: Download Process**
- Download notice/rules acknowledgment (configurable)
- Passkey injection into torrent file
- Download count tracking
- Download restrictions:
  - Slot-based system (configurable)
  - Ratio-based restrictions
  - Class-based restrictions
- Download speed tracking

### 3.3 Tracker Engine

#### 3.3.1 Announce Protocol
**FR-TE-001: Tracker Announce**
- BitTorrent announce protocol implementation
- Announce interval (default: 1800 seconds)
- Variable announce intervals based on torrent age
- Passkey validation
- IP address tracking (configurable max IPs: 2)
- Peer information exchange
- Upload/download statistics tracking

**FR-TE-002: Peer Management**
- Track active seeders and leechers
- Peer timeout/cleanup
- Connection tracking
- Port and client agent tracking
- Seeding/leeching time tracking

**FR-TE-003: Client Validation**
- Allowed/banned client detection
- Client family and version tracking
- Exception list for buggy clients
- Cheater detection algorithms

**FR-TE-004: Scrape Support**
- Torrent statistics scraping
- Multi-torrent scrape support
- Anonymous scrape data

### 3.4 Ratio & Bonus System

#### 3.4.1 Ratio Management
**FR-RB-001: Ratio Calculation**
- Real-time ratio: uploaded / downloaded
- Ratio display with color coding
- Ratio requirements per user class
- Ratio exemptions for:
  - Donors
  - Staff members
  - VIP users
  - Free leech torrents

**FR-RB-002: Ratio Enforcement**
- Download restrictions based on ratio
- Warning system for low ratio
- Leech warning status
- Grace periods for new users

#### 3.4.2 Bonus Point System
**FR-RB-003: Earning Bonus Points**
Users earn bonus points ("seedbonus") for:
- Seeding torrents (per hour, configurable)
- Maximum seeding bonus per period
- Multipliers for torrent age and size
- Donor multiplier (default: 2x)
- Activity bonuses:
  - Uploading torrents
  - Uploading subtitles
  - Forum participation (topics, posts, comments)
  - Poll voting
  - Offer voting
  - Saying thanks

**FR-RB-004: Spending Bonus Points**
Users can spend bonus points on:
- Upload amount (1GB, 5GB, 10GB packages)
- Invitations
- Custom title
- VIP status
- Ratio improvement
- Download amount increase
- Gift to other users (with tax)

**FR-RB-005: Charity System**
- Separate charity fund tracking
- Tax on bonus gifts (configurable percentage)

### 3.5 Community Features

#### 3.5.1 Forum System
**FR-CF-001: Forum Structure**
- Hierarchical forum categories
- Public and private forums
- Class-based forum access
- Forum moderator assignment
- Forum statistics tracking

**FR-CF-002: Topics & Posts**
- Create, edit, delete topics
- Post replies
- Quote functionality
- BBCode formatting support
- Post editing (with edit history)
- Topic locking/unlocking
- Topic stickying
- Topic merging/splitting (moderator)

**FR-CF-003: Forum Features**
- Read/unread tracking
- Post search
- Active topics
- Subscriptions/notifications
- Smilies/emoticons
- Attachment support (class-based)
- Polls in topics

**FR-CF-004: Forum Moderation**
- Delete posts/topics
- Move topics between forums
- Edit posts
- Lock/unlock topics
- Ban users from forums
- Moderation log

#### 3.5.2 Private Messaging
**FR-CF-005: PM System**
- Send/receive private messages
- Inbox/Outbox/Sent folders
- Custom mailboxes (configurable number)
- Message search
- Mass PM (staff only)
- PM storage limits (class-based)
- Read receipts
- Message deletion settings (delete on read option)
- Block PM from specific users
- PM acceptance settings (yes/friends only/no)

**FR-CF-006: PM Features**
- BBCode support
- Smilies
- Subject and body
- Forward messages
- Save messages
- Multiple recipient support (staff)

#### 3.5.3 Comments
**FR-CF-007: Comment System**
- Comments on torrents
- Comments on offers
- BBCode formatting
- Edit/delete own comments (time-limited)
- Moderator comment management
- Comment notifications (configurable)
- Thanks system on comments

**FR-CF-008: Thanks System**
- Thank button on comments/torrents
- Thank count tracking
- Bonus points for giving/receiving thanks
- View who thanked

#### 3.5.4 Shoutbox
**FR-CF-009: Shoutbox Features**
- Real-time chat display
- AJAX auto-refresh (configurable interval)
- BBCode support
- Smilies
- Configurable message display count
- Staff shoutbox management
- Shoutbox moderation (delete messages)

#### 3.5.5 Social Features
**FR-CF-010: Friends System**
- Add/remove friends
- Friend list view
- Block users
- Friend-based PM filtering

**FR-CF-011: User Interaction**
- Send messages
- View profiles
- View user history
- User search
- Online user tracking
- Recent user activity

### 3.6 Administrative Features

#### 3.6.1 User Administration
**FR-AD-001: User Management**
Administrators can:
- Create user accounts
- Delete user accounts
- Edit user profiles
- Change user class
- Reset passwords
- Ban/unban users
- Warn users (with timeout)
- Park accounts
- View user logs (IP, actions)
- Manage invitations
- View login attempts

**FR-AD-002: User Monitoring**
- View user statistics
- Track downloads/uploads
- Monitor ratio
- View connection history
- Cheater detection and listing
- Inactive user reports

#### 3.6.2 Torrent Administration
**FR-AD-003: Torrent Management**
Staff can:
- Edit torrent details
- Delete torrents
- Move torrents between categories
- Ban/unban torrents
- Promote/demote torrents
- Sticky torrents
- View torrent structure/files
- Mass torrent operations

**FR-AD-004: Torrent Monitoring**
- View all torrents
- Search by uploader
- View deleted torrents
- View banned torrents
- Orphan torrent detection

#### 3.6.3 Site Configuration
**FR-AD-005: Settings Management**
Administrators can configure:
- Site name, URL, email
- Registration mode
- Invitation system
- Maximum users
- Email restrictions
- Time zones
- Language defaults
- Theme defaults
- Feature toggles (polls, stats, forums, etc.)
- Cleanup intervals
- Announce intervals
- Ratio requirements
- Bonus point values
- Upload/download limits
- Security settings

**FR-AD-006: Content Management**
- News management (add/edit/delete)
- FAQ management
- Rules management
- Links management
- Poll management
- Advertisement management
- Fun box management
- Offer management

**FR-AD-007: Category Management**
- Add/edit/delete categories
- Category icons
- Category ordering
- Subcategory management (sources, codecs, etc.)

#### 3.6.4 Moderation Tools
**FR-AD-008: Moderation Panel**
- Reports management
- Forum moderation
- Comment moderation
- Torrent moderation
- User moderation
- Moderator task assignment
- Moderation log viewing

**FR-AD-009: Staff Communication**
- Staff messages/announcements
- Staff panel
- Staff box for internal coordination
- Contact staff form

#### 3.6.5 System Administration
**FR-AD-010: System Tools**
- Database backup/restore
- Cache clearing (memcache)
- Cleanup triggers (manual/automatic)
- MySQL statistics
- Log viewing (site log, confirmation log)
- Email testing
- IP testing/lookup

**FR-AD-011: Security Management**
- Banned emails
- Allowed emails
- IP bans
- Client bans
- Maximum connection limits
- Cheater detection settings
- Secure login enforcement

### 3.7 Additional Features

#### 3.7.1 Invitation System
**FR-AF-001: Invitations**
- Invitation code generation
- Email-based invitations
- Invitation tracking (who invited whom)
- Invitation count per user (class-based)
- Invitation expiration (configurable timeout)
- Invitation purchase with bonus points
- Bonus invitations on class promotion

**FR-AF-002: Invitation Management**
- View sent invitations
- Invitation statistics
- Admin view of all invitations

#### 3.7.2 Donation System
**FR-AF-003: Donations**
- Donation tracking
- Donor status (with expiration)
- Multiple payment methods (Alipay, PayPal)
- Donation history
- Donor benefits:
  - Bonus multiplier
  - No auto-deletion
  - Special privileges
  - Ad removal
- Donor list display

#### 3.7.3 Offer System
**FR-AF-004: Offers**
- Users propose wanted content
- Voting on offers
- Offer approval process
- Offer fulfillment tracking
- Offer timeout (configurable)
- Minimum votes for approval
- Offer management (staff)

#### 3.7.4 Subtitle System
**FR-AF-005: Subtitles**
- Upload subtitles for torrents
- Subtitle file management
- Subtitle download tracking
- Multiple subtitle files per torrent
- Language specification
- Subtitle editing/deletion
- Bonus points for subtitle uploads

#### 3.7.6 Advertisement System
**FR-AF-006: Advertisements**
- Multiple ad positions:
  - Header, footer, below nav, below search box
  - Torrent detail pages
  - Comment sections
  - Forum areas
- Ad types: BBCode, XHTML, text, image, flash
- Ad scheduling (start/end time)
- Ad display order
- Click tracking
- Bonus for ad clicks
- No-ad status (purchasable, donor benefit)

#### 3.7.7 External Integration
**FR-AF-007: IMDb Integration**
- IMDb URL input
- Automatic movie info scraping
- Display movie details on torrent page
- Poster image display
- Movie rating and information

**FR-AF-008: UCenter Integration**
- Single sign-on with UCenter
- User synchronization
- Configurable UCenter settings

#### 3.7.9 RSS Feeds
**FR-AF-009: RSS Support**
- Torrent RSS feed generation
- Personalized RSS (with passkey)
- Category-based RSS
- Search-based RSS feeds

#### 3.7.10 Promotion System
**FR-AF-010: Promotion Links**
- User-specific promotion links
- Click tracking
- Bonus for promotion clicks
- Referral tracking

#### 3.7.11 Bitbucket System
**FR-AF-011: Bitbucket**
- File upload to shared storage
- File download tracking
- File management
- Bitbucket log
- Storage quota

---

## 4. Data Model

### 4.1 Core Entities

#### 4.1.1 Database Tables (75 total)

**User & Authentication (8 tables)**
- `users`: Main user data (100+ fields including stats, settings, status)
- `invites`: Invitation tracking
- `loginattempts`: Failed login tracking
- `iplog`: IP address logging
- `blocks`: User blocking
- `friends`: Friend relationships
- `regimages`: Registration CAPTCHA images
- `schools`: Educational institution affiliations

**Torrent & Tracker (12 tables)**
- `torrents`: Main torrent data (50+ fields)
- `files`: Torrent file listings
- `peers`: Active peer connections
- `snatched`: Download completion records
- `torrents_state`: Torrent promotion states
- `categories`: Torrent categories
- `sources`, `media`, `codecs`, `standards`, `processings`, `teams`, `audiocodecs`: Metadata subcategories
- `bookmarks`: User bookmarks

**Community (12 tables)**
- `forums`: Forum categories
- `topics`: Forum topics
- `posts`: Forum posts
- `readposts`: Read tracking
- `comments`: Torrent/offer comments
- `messages`: Private messages
- `pmboxes`: PM mailbox configuration
- `polls`, `pollanswers`: Polling system
- `shoutbox`: Shoutbox messages
- `overforums`: Forum overviews
- `forummods`: Forum moderators

**Content & Media (8 tables)**
- `news`: Site news/announcements
- `faq`: FAQ entries
- `rules`: Site rules
- `links`: External links
- `subs`: Subtitle files
- `attachments`: File attachments
- `fun`: Fun box entries
- `funvotes`: Fun box votes

**Bonus & Economy (6 tables)**
- `thanks`: Thank tracking
- `offers`: Content offers
- `offervotes`: Offer voting
- `funds`: Donation records
- `adclicks`: Advertisement clicks
- `prolinkclicks`: Promotion link clicks

**Administration (10 tables)**
- `adminpanel`: Admin panel menu items
- `modpanel`: Moderator panel items
- `sysoppanel`: SysOp panel items
- `staffmessages`: Staff announcements
- `sitelog`: Site activity log
- `chronicle`: User action chronicle
- `reports`: User reports
- `cheaters`: Detected cheaters
- `bans`: IP/user bans
- `modtask`: Moderation tasks (implied)

**Configuration & Metadata (12 tables)**
- `language`: Language settings
- `stylesheets`: Theme/stylesheet definitions
- `caticons`: Category icon sets
- `secondicons`: Additional icons
- `searchbox`: Search configuration
- `countries`: Country list
- `locations`: Geographic locations
- `isp`: ISP providers
- `advertisements`: Ad management
- `agent_allowed_family`: Allowed BitTorrent clients
- `agent_allowed_exception`: Client exceptions
- `avps`: Anti-VIP protection (assumed)

**System & Utilities (7 tables)**
- `bitbucket`: File sharing storage
- `bannedemails`, `allowedemails`: Email filtering
- `downloadspeed`, `uploadspeed`: Speed tracking
- `suggest`: Search suggestions
- `regimages`: Registration verification images

### 4.2 Key Relationships

```
users (1) ←→ (N) torrents (uploaded by)
users (1) ←→ (N) peers (seeding/leeching)
users (1) ←→ (N) posts (forum activity)
users (1) ←→ (N) messages (sent/received)
users (1) ←→ (N) invites (invited by/invited users)
users (1) ←→ (N) friends (friendships)
users (1) ←→ (N) snatched (download history)

torrents (1) ←→ (N) files (torrent contents)
torrents (1) ←→ (N) peers (active connections)
torrents (1) ←→ (N) comments (torrent comments)
torrents (1) ←→ (N) subs (subtitles)
torrents (1) ←→ (1) categories (classification)
torrents (1) ←→ (1) sources, media, codecs, etc. (metadata)

forums (1) ←→ (N) topics
topics (1) ←→ (N) posts
posts (N) ←→ (1) users (author)

polls (1) ←→ (N) pollanswers
offers (1) ←→ (N) offervotes
```

### 4.3 Critical Fields

**users table key fields:**
- Authentication: `username`, `passhash`, `secret`, `email`, `passkey`
- Stats: `uploaded`, `downloaded`, `seedtime`, `leechtime`, `class`
- Status: `enabled`, `warned`, `donor`, `vip_added`, `parked`
- Economy: `seedbonus`, `invites`, `charity`, `donated`
- Preferences: `stylesheet`, `lang`, `privacy`, `acceptpms`

**torrents table key fields:**
- Identity: `info_hash`, `name`, `filename`, `size`
- Classification: `category`, `source`, `medium`, `codec`, etc.
- Stats: `seeders`, `leechers`, `times_completed`, `views`
- Content: `descr`, `small_descr`, `nfo`
- Status: `visible`, `banned`, `sp_state` (promotion)
- Ownership: `owner`, `anonymous`

---

## 5. Non-Functional Requirements

### 5.1 Performance

**NFR-P-001: Response Time**
- Page load time: < 3 seconds for typical operations
- Torrent listing: < 5 seconds for 50 items
- Announce response: < 1 second

**NFR-P-002: Scalability**
- Support up to 50,000 users (configurable limit)
- Handle hundreds of concurrent announce requests
- Memcached for database query caching
- Configurable cleanup intervals to manage database size

**NFR-P-003: Database Optimization**
- Indexed tables for fast queries
- Automatic cleanup of old data:
  - Inactive peers
  - Old IP logs
  - Read post tracking
  - Expired login attempts

### 5.2 Security

**NFR-S-001: Authentication Security**
- Password hashing (MD5 - note: should be upgraded)
- Session security with secret keys
- Passkey-based tracker authentication
- Configurable secure login (HTTPS)
- Login attempt limiting and tracking

**NFR-S-002: Data Protection**
- SQL injection prevention via parameterized queries
- XSS protection through HTML escaping
- CSRF protection
- IP logging for security tracking
- Configurable email change restrictions

**NFR-S-003: Privacy**
- Anonymous upload option
- Privacy levels (strong/normal/low)
- IP address hashing/encryption
- Configurable user profile visibility

**NFR-S-004: Anti-Cheating**
- BitTorrent client validation
- Upload/download ratio monitoring
- Peer connection validation
- Cheater detection algorithms
- Banned client list

**NFR-S-005: Access Control**
- Role-based access control (18 user classes)
- Class-based feature restrictions
- Forum-level permissions
- Private forum support
- Administrative action logging

### 5.3 Reliability

**NFR-R-001: Availability**
- Configurable maintenance mode
- Site offline message
- Database connection error handling
- Graceful degradation on component failure

**NFR-R-002: Data Integrity**
- Torrent info_hash validation
- File structure validation
- Referential integrity in database
- Transaction support for critical operations

**NFR-R-003: Backup & Recovery**
- Database backup capability
- Torrent file backup (file system)
- Configuration file preservation

### 5.4 Usability

**NFR-U-001: Internationalization**
- Multi-language support (English, Chinese Simplified, Chinese Traditional)
- Language files for easy translation
- Per-user language preference
- Language-specific content

**NFR-U-002: User Interface**
- Multiple theme/stylesheet support
- Responsive design elements
- Configurable interface elements
- Tooltip support (domTT library)
- AJAX for dynamic content updates

**NFR-U-003: Accessibility**
- Configurable font sizes
- Alternative text for images
- Keyboard navigation support (basic)

### 5.5 Maintainability

**NFR-M-001: Code Organization**
- Modular structure (include/, classes/, config/)
- Separation of concerns (presentation, logic, data)
- Configuration files for easy customization
- Language files separated from code

**NFR-M-002: Logging & Monitoring**
- Site activity logging
- Error logging
- Security event logging
- User action tracking
- MySQL query debugging (optional)

**NFR-M-003: Documentation**
- Installation guide (INSTALL file)
- Configuration examples
- Database structure documentation
- Release notes

### 5.6 Compatibility

**NFR-C-001: Browser Support**
- Modern browsers (Firefox, Chrome, Safari, Internet Explorer)
- JavaScript required for enhanced features
- Graceful degradation without JavaScript

**NFR-C-002: Server Compatibility**
- Linux/Unix primary support
- Windows server support (secondary)
- Apache HTTP Server (primary)
- IIS support (limited)

**NFR-C-003: Client Compatibility**
- Support for major BitTorrent clients
- Client whitelist/blacklist configuration
- Peer ID and user agent validation

---

## 6. User Workflows

### 6.1 Primary User Workflows

#### 6.1.1 New User Registration & First Torrent Download
1. User visits site, clicks "Sign Up"
2. System checks registration mode (open/invite-only/closed)
3. User fills registration form (username, password, email)
4. System validates email format and uniqueness
5. System sends confirmation email OR auto-confirms (based on config)
6. User confirms email (if required)
7. User logs in with credentials
8. User browses torrent listings
9. User views torrent details
10. User clicks download (may see download notice first)
11. System generates torrent with user's passkey
12. User opens torrent in BitTorrent client
13. Client announces to tracker with passkey
14. Tracker validates and responds with peer list
15. User begins downloading

#### 6.1.2 Torrent Upload Workflow
1. User navigates to Upload page
2. System checks upload privileges (class ≥ 2 or special permission)
3. User selects .torrent file
4. User fills required fields: category, name, description
5. User optionally adds: NFO, subtitles, IMDb link, screenshots
6. User selects subcategories (source, codec, etc.) if applicable
7. User chooses anonymous upload (if eligible)
8. System validates torrent file structure
9. System checks for duplicate (info_hash)
10. System modifies announce URL in torrent
11. System stores torrent file
12. System inserts torrent record in database
13. System awards bonus points to uploader
14. System redirects to torrent details page

#### 6.1.3 Ratio Management Workflow
1. User checks profile to view ratio
2. If ratio is low:
   - Option A: Seed more torrents to improve ratio
   - Option B: Earn bonus points through activity
   - Option C: Spend bonus points to buy upload credit
3. User navigates to "My Bonus" page
4. User selects "Buy Upload" option
5. User chooses package size (1GB, 5GB, 10GB)
6. System deducts bonus points
7. System adds upload credit to user's account
8. User's ratio improves

### 6.2 Administrative Workflows

#### 6.2.1 User Class Promotion
1. System runs automatic cleanup/maintenance
2. System checks eligible users for promotion
3. For each user, system evaluates:
   - Current class
   - Time on site
   - Upload amount
   - Download ratio
   - Account status
4. If criteria met, system promotes user to next class
5. System grants new class privileges
6. System may award invitation credits
7. System optionally notifies user via PM

#### 6.2.2 Torrent Moderation Workflow
1. User reports problematic torrent
2. System creates report entry
3. Moderator views reports in mod panel
4. Moderator reviews torrent details
5. Moderator decides action:
   - Edit details (if incorrect info)
   - Delete torrent (if violates rules)
   - Ban torrent (if malicious)
   - Move to different category
   - Contact uploader
6. Moderator executes action
7. System logs moderation action
8. System may notify uploader of action

---

## 7. Business Rules

### 7.1 Ratio & Economy Rules

**BR-RE-001: Ratio Calculation**
- Ratio = Total Uploaded / Total Downloaded
- Division by zero handling: unlimited ratio if downloaded = 0
- Ratio displayed to 2 decimal places

**BR-RE-002: Ratio Requirements**
- Different ratio requirements per user class
- Lower classes have stricter requirements
- Peasant class most restrictive (multiple thresholds)
- High classes and special status (donor, VIP, staff) exempted

**BR-RE-003: Download Restrictions**
- Users below ratio threshold may be restricted from downloading
- Download slots may be limited based on ratio
- Free leech torrents don't count toward download total

**BR-RE-004: Bonus Point Economy**
- Points earned passively through seeding
- Points earned through community participation
- Points have multiple spending options
- Gift taxation to prevent point farming
- Donor bonus multiplier encourages donations

### 7.2 Content Management Rules

**BR-CM-001: Torrent Lifecycle**
- Torrents remain active as long as seeders exist
- Dead torrents (no seeders) may be auto-deleted after timeout
- Staff-promoted torrents never auto-deleted
- Banned torrents hidden from regular users

**BR-CM-002: Upload Restrictions**
- Minimum class requirement (default: User class 2)
- File size limits enforced
- Duplicate detection prevents re-uploads
- Category selection required
- Description required (minimum length may apply)

**BR-CM-003: Content Promotion**
- Automatic promotion for large files
- Random promotion percentages configurable
- Time-based promotion expiration
- Manual promotion by authorized staff
- Global promotion modes available

### 7.3 User Management Rules

**BR-UM-001: Account Creation**
- Unique username required
- Unique email required
- Email domain restrictions (if enabled)
- Invitation code required (if invite-only mode)
- Email confirmation required (if not auto-confirm)

**BR-UM-002: Account Deletion**
- Automatic deletion after inactivity period
- Class-based deletion exemptions
- Donor/VIP never auto-deleted
- Staff never auto-deleted
- Manual deletion by administrators only

**BR-UM-003: Class Progression**
- Users can only advance, never demoted automatically
- Promotion based on objective criteria
- Manual demotion by administrators allowed
- Staff promotion manual only
- Special classes (VIP, Uploader) assigned manually

**BR-UM-004: Warning & Banning**
- Warnings temporary with expiration date
- Multiple warnings tracked
- Bans can be permanent or temporary
- Warning affects privileges
- Automatic unban after expiration

### 7.4 Community Rules

**BR-CR-001: Forum Posting**
- Minimum class for posting (configurable)
- Post editing time limit (for non-staff)
- Edit history preserved
- Double posting may be restricted
- Flood control on rapid posting

**BR-CR-002: Private Messaging**
- PM storage limits based on class
- Recipients can block senders
- Mass PM restricted to staff
- PM forwarding allowed
- Saved messages count toward limit

**BR-CR-003: Comment System**
- Comments on torrents and offers
- Edit time limit for regular users
- Deletion restricted to author and staff
- Spam detection may apply
- Thanks limited to one per comment

### 7.5 Security Rules

**BR-SR-001: Authentication**
- Maximum login attempts before lockout
- IP tracking for security
- Multiple concurrent sessions may be restricted
- Password complexity (basic, can be enhanced)
- Passkey regeneration on compromise

**BR-SR-002: Client Validation**
- Only whitelisted clients allowed (if enforced)
- Client version checking
- Exception list for specific versions
- Peer ID validation
- User agent validation

**BR-SR-003: IP Restrictions**
- Maximum IPs per user account (default: 2)
- IP change tracking
- Suspicious IP patterns flagged
- VPN/proxy detection (basic)

---

## 8. Interface Requirements

### 8.1 User Interface Pages

**Main Pages:**
- Home/Index page (news, stats, polls, shoutbox)
- Torrent listing/browse page
- Torrent details page
- Upload page
- User profile page
- User control panel (settings)
- Forum listing page
- Forum topic view
- Private messages (inbox/outbox)
- My Bonus page
- Top 10 statistics
- User search
- FAQ page
- Rules page

**User Account Pages:**
- Login page
- Signup/registration page
- Password recovery page
- Email confirmation page
- Invitation page
- User details (public profile)
- User history
- Edit profile

**Administrative Pages:**
- Admin panel (dashboard)
- User management
- Torrent management
- Category management
- Forum management
- News management
- FAQ management
- Advertisement management
- Site configuration/settings
- Logs viewing
- Reports management
- Staff messaging

### 8.2 UI Components

**Navigation:**
- Top navigation bar (Home, Torrents, Forums, Upload, etc.)
- User menu (Profile, Messages, Bonus, Logout)
- Breadcrumb navigation
- Search box (torrents, users, forums)

**Display Components:**
- Data tables (sortable, paginated)
- Forms (with validation)
- BBCode editor with toolbar
- File upload interface
- Smilies selector
- Category selector
- Torrent file list
- Peer list
- User statistics display
- Ratio display with color coding
- Progress bars

**Interactive Elements:**
- AJAX search suggestions
- Auto-refreshing shoutbox
- Tooltip popups (domTT)
- Image resizing
- Flash media player
- FLV video player
- Collapsible sections
- Tab interfaces

### 8.3 Visual Design Requirements

**Theme System:**
- Multiple stylesheets supported
- User-selectable themes
- Consistent layout across themes
- Color schemes for different user classes
- Icon sets for categories

**Responsive Elements:**
- Configurable font sizes
- Adjustable table widths
- Image auto-resizing
- Print-friendly views

---

## 9. External Interfaces

### 9.1 BitTorrent Protocol Interface

**Announce Interface:**
- Endpoint: `/announce.php`
- Protocol: HTTP GET
- Required parameters: info_hash, peer_id, port, uploaded, downloaded, left
- Optional parameters: ip, numwant, key, compact, event
- Response: Bencoded dictionary

**Scrape Interface:**
- Endpoint: `/scrape.php`
- Protocol: HTTP GET
- Parameters: info_hash (single or multiple)
- Response: Bencoded dictionary with torrent stats

### 9.2 Email Interface

**SMTP Integration:**
- Outbound email for:
  - Registration confirmation
  - Password recovery
  - User notifications
  - Staff messages
  - System alerts
- Configurable SMTP server
- Email templating
- Multi-language email support

### 9.3 External Service Integrations

**IMDb Integration:**
- HTTP requests to IMDb
- HTML parsing for movie information
- Poster image fetching
- Caching of scraped data

**UCenter Integration:**
- User synchronization
- Single sign-on
- API communication
- Configurable endpoints

### 9.4 File System Interface

**File Storage:**
- Torrent file storage: `/torrents/`
- Attachment storage: `/attachments/`
- Subtitle storage: `/subs/`
- Bitbucket storage: `/bitbucket/`
- NFO file storage (embedded in torrents table)
- Avatar storage: `/pic/`

**File Operations:**
- Upload/download
- Validation
- Deletion
- Directory organization (date-based, hash-based)

### 9.5 Cache Interface

**Memcached Integration:**
- Cache configuration settings
- Cache torrent data
- Cache user sessions
- Cache query results
- Cache frequently accessed data
- TTL-based expiration
- Manual cache clearing

---

## 10. Deployment & Infrastructure

### 10.1 Server Requirements

**Minimum Requirements:**
- **OS**: Linux/Unix (Ubuntu Server recommended), Windows Server supported
- **CPU**: 2+ cores recommended
- **RAM**: 2GB minimum, 4GB+ recommended
- **Storage**: 50GB+ depending on torrent count
- **Network**: Stable internet connection with adequate bandwidth

**Software Stack:**
- **Web Server**: Apache 2.2.x with mod_rewrite
- **PHP**: 5.2.x or 5.3.x with extensions (mbstring, mysql, memcache, gd)
- **Database**: MySQL 5.0.x or higher
- **Cache**: Memcached daemon
- **Email**: SMTP server (Postfix recommended)

### 10.2 Installation Process

1. **Environment Setup**
   - Install web server (Apache)
   - Install PHP with required extensions
   - Install MySQL server
   - Install Memcached
   - Install optional PEAR with HTTP_Request2

2. **Application Deployment**
   - Copy files to web server document root
   - Set file permissions (777 for development, restricted for production)
   - Configure Apache virtual host
   - Disable directory access to sensitive folders (_db, config, lang)

3. **Database Setup**
   - Create database
   - Import structure from `_db/dbstructure.sql`
   - Set SQL mode to empty string

4. **Configuration**
   - Edit `config/allconfig.php`
   - Set database connection parameters
   - Set site name and URL
   - Configure announce URL
   - Set timezone in PHP

5. **Initial User**
   - Register first user via web interface
   - Manually set user class to Staff Leader (16) via MySQL

6. **Service Startup**
   - Start/restart Apache
   - Start/restart MySQL
   - Start Memcached daemon

### 10.3 Production Considerations

**Security Hardening:**
- Disable magic quotes in PHP
- Set restrictive file permissions
- Enable HTTPS for secure sections
- Configure firewall rules
- Regular security updates
- Strong database passwords

**Performance Optimization:**
- Enable opcode caching (APC, OPcache)
- Configure Memcached memory limits
- Optimize MySQL configuration (max_connections, query cache)
- Enable gzip compression
- CDN for static assets (optional)

**Monitoring:**
- Web server access/error logs
- PHP error logs
- MySQL slow query log
- Application-level logging (sitelog table)
- Tracker announce performance
- Memcached hit rates

**Backup Strategy:**
- Regular database dumps
- Torrent file backups
- Configuration file backups
- User avatar and attachment backups
- Offsite backup storage recommended

**Scaling Considerations:**
- Database replication for read-heavy loads
- Separate tracker and web servers
- Load balancing for web frontend
- Dedicated Memcached server(s)
- Storage scaling for torrent files

---

## 11. Migration & Upgrade Paths

### 11.1 Data Migration

**From Other Tracker Software:**
- User data import scripts needed
- Torrent metadata migration
- Forum content migration
- Peer data may not be transferable
- Custom scripts per source system

**Database Schema Updates:**
- Incremental update scripts
- Backup before upgrade mandatory
- Rollback procedures documented
- Test on staging environment first

### 11.2 Version Upgrade Process

1. Backup current installation
2. Backup database
3. Put site in maintenance mode
4. Upload new files (preserve config)
5. Run database migration scripts
6. Clear cache (Memcached)
7. Test functionality
8. Bring site online
9. Monitor for issues

---

## 12. Known Limitations & Technical Debt

### 12.1 Current Limitations

**Security:**
- MD5 password hashing (outdated, should use bcrypt/argon2)
- No CSRF tokens on all forms
- XSS protection relies on manual escaping
- SQL injection prevention not consistently parameterized

**Scalability:**
- No built-in sharding or partitioning
- Single database architecture
- Limited horizontal scaling options
- Session management not distributed-ready

**Code Quality:**
- Mixed PHP and HTML (not using MVC pattern)
- Global variables heavily used
- Limited object-oriented design
- Code duplication across files
- Minimal automated testing

**Browser Compatibility:**
- Relies on older JavaScript libraries
- Not fully responsive for mobile devices
- Limited accessibility features
- Uses outdated Flash for media

**Database:**
- Uses MyISAM engine (no transactions)
- Character encoding issues possible (UTF-8 not enforced everywhere)
- No ORM, raw SQL queries
- Schema not normalized in some areas

### 12.2 Refactoring Opportunities

**High Priority:**
1. **Security Improvements**
   - Upgrade password hashing algorithm
   - Implement CSRF protection site-wide
   - Use prepared statements consistently
   - Add input validation framework

2. **Architecture Modernization**
   - Implement MVC pattern
   - Use dependency injection
   - Create service layer
   - Separate business logic from presentation

3. **Database Optimization**
   - Convert to InnoDB for transactions
   - Normalize schema where appropriate
   - Implement ORM (Doctrine, Eloquent)
   - Add database migrations

**Medium Priority:**
4. **Code Organization**
   - Create proper class structure
   - Implement autoloading
   - Use namespaces
   - Reduce global variable usage

5. **Frontend Modernization**
   - Responsive design implementation
   - Modern JavaScript framework (Vue, React)
   - Replace Flash with HTML5
   - Improve accessibility (WCAG compliance)

6. **API Development**
   - RESTful API for mobile apps
   - JSON responses for AJAX
   - API authentication (OAuth2)
   - API documentation

**Low Priority:**
7. **Testing Infrastructure**
   - Unit test framework
   - Integration tests
   - End-to-end tests
   - Continuous integration setup

8. **Developer Experience**
   - Composer for dependency management
   - Development environment setup (Docker)
   - Code style enforcement (PSR-12)
   - Documentation generation

---

## 13. Future Enhancements (Potential)

### 13.1 Feature Enhancements

**User Experience:**
- Mobile application (iOS, Android)
- Progressive Web App (PWA)
- Dark mode theme
- Advanced user dashboard with analytics
- Customizable homepage widgets
- Better search with filters and facets

**Community Features:**
- User groups/teams
- Achievements/badges system
- User-to-user trading system
- Social media integration
- Advanced notification system (email, push, in-app)
- Live chat instead of basic shoutbox

**Content Management:**
- Automatic torrent tagging
- Machine learning for content categorization
- Better duplicate detection
- Torrent collections/series management
- Auto-generated torrent metadata
- Video preview/streaming

**Gamification:**
- Leaderboards
- Challenges/quests
- Seasonal events
- Reputation system beyond ratio
- User levels beyond class
- Special rewards and perks

### 13.2 Technical Enhancements

**Performance:**
- Full-page caching (Redis, Varnish)
- Database query optimization
- Lazy loading for images
- CDN integration for static assets
- GraphQL API for efficient data fetching

**Infrastructure:**
- Microservices architecture (separate tracker, web, API)
- Container orchestration (Kubernetes)
- Cloud-native deployment options
- Auto-scaling capabilities
- Better monitoring and alerting

**Integration:**
- Plex/Jellyfin integration
- Sonarr/Radarr automation
- Discord/Slack bots
- Third-party authentication (OAuth, LDAP)
- Webhook system for events

---

## 14. Compliance & Legal

### 14.1 Licensing

**Project License:**
- Open source (specific license in LICENSE file)
- Forked from TBSource and other open source projects
- Must comply with source project licenses
- Attribution required

### 14.2 Privacy Considerations

**User Data:**
- Personal information storage (email, IP)
- Data retention policies needed
- User data export capability
- Account deletion procedures
- GDPR compliance considerations (if applicable)

**Content:**
- User-generated content responsibility
- Copyright compliance (tracker operators' responsibility)
- DMCA takedown procedures
- Content moderation policies

### 14.3 Terms of Service Requirements

Recommended site policies:
- Acceptable use policy
- Copyright policy
- Privacy policy
- User agreement
- Donation terms
- Refund policy (if applicable)

---

## 15. Success Metrics & KPIs

### 15.1 User Engagement Metrics

- Daily/Weekly/Monthly Active Users (DAU/WAU/MAU)
- User retention rate
- Average session duration
- Page views per session
- Forum posts per day
- Shoutbox messages per day
- Private messages sent
- Torrent comments posted

### 15.2 Content Metrics

- Total active torrents
- New torrent uploads per day
- Average seeders per torrent
- Torrent completion rate
- Dead torrent percentage
- Category distribution
- Subtitle uploads
- NFO file usage

### 15.3 Community Health Metrics

- User registration rate
- Invitation conversion rate
- Donor/VIP conversion rate
- Average user class distribution
- User ratio distribution
- Banned/warned user percentage
- Support ticket response time
- Moderator action frequency

### 15.4 Technical Metrics

- Server uptime percentage
- Average page load time
- Announce response time
- Database query performance
- Cache hit rate
- Error rate
- Bandwidth usage
- Storage usage growth

### 15.5 Economic Metrics

- Bonus points economy (total circulation, inflation rate)
- Donation revenue (if applicable)
- Upload/download traffic volume
- Invitation usage rate
- Offer completion rate

---

## 16. Glossary

**Terms & Definitions:**

- **Announce**: Communication from BitTorrent client to tracker reporting status
- **BBCode**: Bulletin Board Code, markup language for formatting
- **Freeleech**: Torrent that doesn't count toward download quota
- **Info Hash**: Unique identifier for a torrent (SHA-1 hash)
- **Leech/Leecher**: User downloading without complete file
- **Passkey**: Unique key per user for tracker authentication
- **Peasant**: Lowest user class
- **Peer**: Any participant in torrent swarm (seed or leech)
- **Ratio**: Upload amount divided by download amount
- **Scrape**: Request for torrent statistics from tracker
- **Seed/Seeder**: User with complete file sharing to others
- **Snatch**: Successful download completion of a torrent
- **Swarm**: All peers (seeders + leechers) for a torrent
- **Tracker**: Server coordinating BitTorrent file sharing
- **Torrent**: Metadata file describing content and tracker
- **VIP**: Special paid/privileged user status

**Abbreviations:**
- API: Application Programming Interface
- CSRF: Cross-Site Request Forgery
- FAQ: Frequently Asked Questions
- IMDb: Internet Movie Database
- NFO: Info file (typically ASCII art with release information)
- PM: Private Message
- PRD: Product Requirements Document
- RSS: Really Simple Syndication
- SQL: Structured Query Language
- XSS: Cross-Site Scripting

---

## 17. Appendices

### Appendix A: Configuration Parameters

Key configuration arrays in `config/allconfig.php`:
- `$ACCOUNT`: User class progression rules
- `$ADVERTISEMENT`: Ad system settings
- `$ATTACHMENT`: File attachment rules
- `$AUTHORITY`: Permission levels per feature
- `$BASIC`: Database and site core settings
- `$BONUS`: Bonus point economy values
- `$CODE`: Version information
- `$MAIN`: Primary site settings
- `$SECURITY`: Security-related settings
- `$SMTP`: Email configuration
- `$TORRENT`: Torrent promotion rules
- `$TWEAK`: Additional customizations

### Appendix B: Database Table Reference

See section 4.1.1 for complete list of 75 database tables.

### Appendix C: File Structure

```
/NexusPHP/
├── _db/                    # Database structure
├── _doc/                   # Documentation
├── api/                    # API endpoints
├── attachments/            # Uploaded attachments
├── bitbucket/              # Shared file storage
├── classes/                # PHP class files
├── config/                 # Configuration files
├── imdb/                   # IMDb integration
├── include/                # Core PHP includes
│   ├── browser/            # Browser detection
│   └── smtp/               # SMTP classes
├── lang/                   # Language files
│   ├── _target/            # Translation targets
│   ├── chs/                # Chinese Simplified
│   ├── cht/                # Chinese Traditional
│   └── en/                 # English
├── pic/                    # Images and icons
├── styles/                 # CSS stylesheets
├── subs/                   # Subtitle files
├── torrents/               # Torrent files
├── uc_client/              # UCenter integration
└── [447 PHP files]         # Application pages
```

### Appendix D: User Class Progression

Example default progression:
- Peasant (0) → User (1): Default on signup
- User (1) → Power User (2): 4 weeks, 50GB upload, ratio ≥ 1.05
- Power User (2) → Elite User (3): 8 weeks, 120GB upload, ratio ≥ 1.55
- Elite User (3) → Crazy User (4): 15 weeks, 300GB upload, ratio ≥ 2.05
- Crazy User (4) → Insane User (5): 25 weeks, 500GB upload, ratio ≥ 2.55
- Insane User (5) → Veteran User (6): 40 weeks, 750GB upload, ratio ≥ 3.05
- Veteran User (6) → Extreme User (7): 60 weeks, 1TB upload, ratio ≥ 3.55
- Extreme User (7) → Ultimate User (8): 80 weeks, 1.5TB upload, ratio ≥ 4.05
- Ultimate User (8) → Nexus Master (9): 100 weeks, 3TB upload, ratio ≥ 4.55

Special classes assigned manually:
- VIP (10): Purchase or special grant
- Retiree (11): Former staff
- Uploader (12): Dedicated uploaders
- Moderator (13): Staff appointment
- Administrator (14): Staff appointment
- SysOp (15): Staff appointment
- Staff Leader (16): Owner/highest authority

---

## Document Control

**Version History:**
- v1.0 (2025-10-29): Initial PRD created through reverse engineering

**Review & Approval:**
- This document should be reviewed by:
  - Development team
  - System administrators
  - Stakeholders/site owners
  - Security team

**Maintenance:**
- This PRD should be updated when:
  - New features are implemented
  - System architecture changes
  - User requirements evolve
  - Security vulnerabilities are discovered
  - Performance optimizations are needed

**Related Documents:**
- INSTALL: Installation guide
- LICENSE: Licensing information
- RELEASENOTE: Version history
- Database schema: _db/dbstructure.sql
- Configuration template: config/allconfig.php

---

**End of Product Requirements Document**

This comprehensive PRD provides a detailed foundation for understanding the NexusPHP system architecture, features, and requirements. It can serve as a reference for future refactoring, modernization, or reimplementation efforts.
