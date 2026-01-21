# Instagram UGC Platform - Wireframes & Screen Designs

This document describes what each screen looks like and how users interact with it. Think of it as a blueprint for building the application.

---

## 📊 Screen 1: Dashboard (Home Page)

### What is this screen?
The first thing users see when they log in. It shows an overview of everything in their account.

### What does it look like?

**Top Bar (Header):**
- Left side: Logo and app name "Instagram UGC Platform"
- Right side: User profile picture (click to see menu) and Settings button

**Main Content Area:**

**1. Statistics Cards (4 boxes in a row):**
- **Box 1**: Total Content Items (e.g., "1,234 items")
  - Shows how many Instagram posts they've saved
- **Box 2**: Collections Count (e.g., "45 collections")
  - Shows how many organized groups they've created
- **Box 3**: Approval Rate (e.g., "89% approved")
  - Shows what percentage of permission requests were approved
- **Box 4**: Pending Requests (e.g., "12 pending")
  - Shows how many creators haven't responded yet

**2. Recent Activity Feed:**
A list showing the latest things that happened:
- "📸 New content discovered: #summer2024 (5 items)"
- "✅ Permission approved: @creator123"
- "📤 Content exported: Summer Campaign Collection"
- "⏳ Permission pending: @creator456 (2 days)"

**3. Quick Action Buttons (3 big buttons):**
- **"🔍 Discover Content"** - Takes you to search Instagram
- **"📁 View Library"** - Shows all saved content
- **"➕ New Collection"** - Creates a new folder to organize content

**4. Top Collections Preview:**
Shows your 3 most-used collections as cards:
- Each card shows: Collection name, thumbnail images, number of items
- Example: "Summer 2024" with 234 items, "Product Launch" with 156 items

### What can users do here?
- See at a glance how much content they have
- Quickly access the most common tasks
- See what's been happening recently
- Jump to their favorite collections

---

## 🔍 Screen 2: Content Discovery (Search Instagram)

### What is this screen?
Where users search Instagram to find posts they want to use for marketing.

### What does it look like?

**Top Bar:** Same as Dashboard (Logo, user menu, settings)

**Page Title:** "🔍 Discover Content"

**Search Section:**
- **Big search box** at the top
  - Placeholder text: "Search hashtags, mentions, keywords..."
  - Search button on the right
- **Filter buttons** below search:
  - "Content Type" dropdown (Photo, Video, Reel)
  - "Date Range" dropdown (Last week, Last month, Custom dates)
  - "Engagement" dropdown (Minimum likes/comments)
  - "Clear Filters" button

**When filters are expanded, you see:**
- Checkboxes: ☑ Photo ☑ Video ☑ Reel
- Date pickers: "From: [calendar] To: [calendar]"
- Number input: "Minimum engagement: [____] likes"

**Results Section:**
- Text showing: "Results: 1,234 items found"
- **Grid of content thumbnails** (like Instagram's grid view)
  - Each thumbnail shows:
    - Small preview image
    - Icon showing type (📸 photo, 🎬 video/reel)
    - Engagement count (👍 1,234 likes)
  - 6 items per row on desktop, 3 on tablet, 2 on mobile
- **"Load More" button** at the bottom to see more results

### How users interact:
1. Type a hashtag (like #summer2024) or @mention in search box
2. Click "Search" or press Enter
3. See results appear in grid below
4. Click any thumbnail to see full details
5. Use filters to narrow down results
6. Scroll or click "Load More" to see more

### What happens when you click a thumbnail?
A popup/modal opens showing the full content (see Screen 4)

---

## 📁 Screen 3: Content Library (All Saved Content)

### What is this screen?
A place to see and organize all the Instagram content you've saved.

### What does it look like?

**Top Bar:** Same navigation as other screens

**Page Title:** "📁 Content Library"

**Left Sidebar:**
- **Collections List:**
  - "📁 All (1,234)" - Shows everything
  - "📁 Summer Campaign (234)" - Individual collections
  - "📁 Product Launch (156)"
  - "📁 Holiday Campaign (89)"
  - "+ New Collection" button at bottom
- **Tags Section:**
  - List of tags you've used: #summer, #product, #lifestyle
  - "+ Add Tag" button

**Main Content Area:**

**Top Toolbar:**
- View toggle: [Grid] [List] buttons
- Sort dropdown: "Newest", "Oldest", "Most Engagement"
- Search box: "🔍 Search library..."
- Filter button
- Tags dropdown

**Content Grid:**
- Same grid layout as Discovery screen
- Each item shows:
  - Thumbnail image
  - Content type icon
  - **Permission status badge:**
    - ✅ Green checkmark = Approved (can use)
    - ⏳ Yellow clock = Pending (waiting for response)
    - ❌ Red X = Denied (can't use)

**Bulk Selection:**
- Checkboxes appear when you hover over items
- Can select multiple items for bulk actions (export, add to collection, delete)

### How users interact:
1. Click a collection in sidebar to filter content
2. Click a tag to filter by tag
3. Switch between Grid and List view
4. Search within saved content
5. Click items to see details
6. Select multiple items for bulk operations

---

## 🖼️ Screen 4: Content Detail View (Popup/Modal)

### What is this screen?
A popup window that appears when you click on any content item. Shows all the details about that Instagram post.

### What does it look like?

**Modal/Popup Window** (centered on screen, dark overlay behind it)

**Close button:** X in top right corner

**Two-column layout:**

**Left Side (60% width):**
- **Large image or video player**
  - Full-size preview of the Instagram post
  - If video/reel: Play button and video controls
  - Can zoom in/out on images

**Right Side (40% width):**

**Content Information:**
- **Creator:** @username (clickable link)
- **Posted:** January 15, 2024
- **Engagement:** 👍 1,234 likes 💬 89 comments
- **Caption:** Full text of the Instagram caption
  - "Amazing product! #summer2024 #lifestyle..."
- **Hashtags:** #summer #lifestyle #product (clickable)
- **Location:** New York, NY (if available)

**Permission Status Card (highlighted box):**
- Status badge: ✅ Approved / ⏳ Pending / ❌ Denied
- Expiration date: "Expires: December 31, 2024"
- "View Details" button to see full permission info

**Collections:**
- Shows which collections this content is in
- Icons: 📁 Summer 2024 📁 Product Launch
- Can add to more collections

**Tags:**
- User-added tags: #summer #product #ugc
- Can add/remove tags

**Usage History:**
- List of where this content was used:
  - "• Used in Email Campaign (January 20)"
  - "• Used in Instagram Ad (January 22)"

**Action Buttons (bottom):**
- **"Save to Collection"** - Add to a collection
- **"Request Permission"** - Ask creator for permission
- **"Export"** - Download the content
- **"Delete"** - Remove from library

### How users interact:
1. View full-size image/video
2. Read all details about the post
3. Check permission status
4. See where content has been used
5. Take actions (save, request permission, export)
6. Click X or outside modal to close

---

## 📂 Screen 5: Collections Management

### What is this screen?
Where users create and manage collections (folders to organize content).

### What does it look like?

**Top Bar:** Standard navigation

**Page Title:** "📁 Collections"

**Top Actions:**
- "+ New Collection" button (big, prominent)
- "Bulk Actions" dropdown (for managing multiple collections)

**Main Content:**

**Collection Cards** (like folders):
Each collection shown as a card with:
- **Collection name** at top (e.g., "Summer Campaign 2024")
- **Preview thumbnails** (6 small images showing what's inside)
- **Stats:** "234 items • Created: January 1, 2024"
- **Action buttons:** [View] [Edit] [Export] [Delete]

Collections displayed in a grid, 2-3 per row.

### Create Collection Modal (popup):

When you click "+ New Collection", a popup appears:

**Title:** "Create New Collection"

**Form Fields:**
- **Collection Name:** Text input box
  - Placeholder: "Enter collection name..."
- **Description:** Optional text area
  - Placeholder: "Enter description..."
- **Add content now?** Checkbox
  - If checked: Shows content browser to add items immediately
  - If unchecked: Creates empty collection

**Buttons:**
- [Cancel] - Closes modal
- [Create Collection] - Creates the collection

### How users interact:
1. Click "+ New Collection" to create new
2. Fill in name and description
3. Optionally add content right away
4. Click "Create"
5. View collections as cards
6. Click "View" to see collection contents
7. Click "Edit" to change name/description
8. Click "Export" to download all content in collection
9. Click "Delete" to remove collection

---

## ✅ Screen 6: Permission Management Dashboard

### What is this screen?
A dashboard to track all permission requests sent to Instagram creators.

### What does it look like?

**Top Bar:** Standard navigation

**Page Title:** "✅ Permission Management"

**Tabs at top:**
- [All] [Pending] [Approved] [Denied] [Expired]
- Click tabs to filter by status

**Filter Bar:**
- Dropdowns: "Creator", "Date Range", "Status"
- Apply filters to narrow results

**Main Content:**

**Permission Request Cards:**

Each request shown as a card with:
- **Content thumbnail** (small image on left)
- **Creator username:** @creator123
- **Status badge:** ⏳ Pending / ✅ Approved / ❌ Denied
- **Request date:** "Requested: January 20, 2024"
- **Contact method:** "Sent via: Email" or "Sent via: Instagram DM"
- **Action buttons:**
  - [View Request] - See full details
  - [Remind] - Send reminder email/DM
  - [Cancel] - Cancel the request

**Grouped by Status:**
- **Pending Requests section** (12 items)
- **Approved section** (89 items)
- Each shows list of relevant cards

### Request Permission Modal:

When requesting permission, a popup appears:

**Title:** "Request Permission"

**Content Preview:**
- Shows the Instagram post thumbnail
- Creator username and post date

**Form Fields:**
- **Contact Method:** Radio buttons
  - ○ Email
  - ○ Instagram DM
  - ○ Both
- **Creator Email:** Text input (if email selected)
- **Permission Template:** Dropdown
  - "Standard UGC License"
  - "Extended License"
  - "Custom"
- **Usage Rights:** Checkboxes
  - ☑ Social media posts
  - ☑ Website
  - ☑ Email campaigns
  - ☑ Paid advertising
  - ☑ Print materials
- **Expiration Date:** Date picker
  - "December 31, 2024"
- **Message Preview:** Text area showing the email/DM that will be sent

**Buttons:**
- [Cancel]
- [Send Request]

### How users interact:
1. View all permission requests in one place
2. Filter by status using tabs
3. See which requests need attention (pending)
4. Send reminders to creators who haven't responded
5. Click "Request Permission" from content detail view
6. Fill out permission form
7. Send request to creator
8. Track status as it changes

---

## 📤 Screen 7: Export Content

### What is this screen?
A popup that appears when users want to download content.

### What does it look like?

**Export Modal (popup):**

**Title:** "Export Content"

**Top:** "Selected: 5 items" (shows how many items being exported)

**Export Options:**

**1. Export Format:**
Radio buttons:
- ○ Original quality (full resolution)
- ○ Compressed (smaller file size)
- ○ Web optimized (for websites)

**2. Include Metadata:**
Checkboxes:
- ☑ Creator information
- ☑ Post URL
- ☑ Engagement stats (likes, comments)
- ☑ Caption
- ☑ Hashtags
- ☑ Permission status

**3. Metadata Format:**
Radio buttons:
- ○ JSON file (for developers)
- ○ CSV file (for spreadsheets)
- ○ Embedded (EXIF data in image files)

**4. Delivery Method:**
Radio buttons:
- ○ Download ZIP file (to your computer)
- ○ Email (send download link)
- ○ Cloud Storage (upload to S3, Dropbox, etc.)

**Buttons:**
- [Cancel]
- [Export] (starts the process)

### Export Progress Screen:

After clicking "Export", shows progress:

**Title:** "Exporting Content..."

**Progress Bar:**
- Visual progress bar: ████████████░░░░░░░░ 60%
- Percentage: "60%"

**Item Status List:**
- ✓ Item 1 - Downloaded
- ✓ Item 2 - Downloaded
- ✓ Item 3 - Downloaded
- ⏳ Item 4 - Processing...
- ⏳ Item 5 - Queued

**Button:**
- [Cancel Export] (can stop if needed)

### How users interact:
1. Select content items (from library or collection)
2. Click "Export" button
3. Choose export options (format, metadata, delivery)
4. Click "Export"
5. Watch progress bar
6. Receive download/email/cloud link when complete

---

## ⚙️ Screen 8: Settings

### What is this screen?
Where users manage their account, team, and preferences.

### What does it look like?

**Top Bar:** Standard navigation

**Page Title:** "⚙️ Settings"

**Two-column layout:**

**Left Sidebar (menu):**
- General (selected)
- Account
- Team
- Integrations
- API
- Billing
- Notifications

**Main Content Area:**

**General Settings Section:**

**Profile Information:**
- **Name:** Text input - "John Doe"
- **Email:** Text input - "john@company.com"
- **Role:** Dropdown - "Admin" / "Member" / "Viewer"

**Instagram Connection:**
- Status: ✅ Connected / ❌ Not Connected
- Account: @brandname
- Buttons: [Disconnect] [Reconnect]

**Default Settings:**
- **Default export format:** Dropdown
  - "Web optimized" / "Original quality" / "Compressed"
- **Auto-save discovered content:** Checkbox
  - ☑ Yes / ☐ No
  - (If checked, content is automatically saved when discovered)

**Save Button:** [Save Changes] at bottom

### Team Management Tab:

**Title:** "Team Members"

**Top:** "+ Invite User" button

**Team Members Table:**
Columns:
- Name
- Email
- Role (Admin/Member/Viewer)
- Actions ([Edit] [Remove])

**Roles Explanation:**
- **Admin:** Full access to all features
- **Member:** Can discover, organize, and export content
- **Viewer:** Read-only access to library

### How users interact:
1. Navigate to Settings from user menu
2. Update profile information
3. Connect/disconnect Instagram account
4. Set default preferences
5. Manage team members (if admin)
6. Invite new team members
7. Change user roles
8. Save changes

---

## 📱 Mobile/Responsive Design

### How screens adapt to smaller screens:

**Dashboard:**
- Stats cards stack vertically (1 per row)
- Activity feed becomes scrollable list
- Quick actions become full-width buttons
- Collections preview shows 1 per row

**Content Discovery:**
- Search bar full width
- Filters collapse into dropdown menu
- Grid shows 2 items per row (instead of 6)
- Thumbnails larger for easier tapping

**Content Library:**
- Sidebar becomes hamburger menu (☰)
- Collections list slides in from side
- Grid shows 2 items per row
- Swipe gestures for navigation

**Content Detail View:**
- Modal takes full screen
- Single column layout (image on top, details below)
- Action buttons full width, stacked vertically

**All Screens:**
- Touch-friendly buttons (minimum 44px height)
- Larger text for readability
- Simplified navigation
- Bottom navigation bar for quick access

---

## 🎨 Design Guidelines

### Colors:
- **Primary:** Instagram gradient (purple → pink → orange)
- **Success/Approved:** Green (#10B981)
- **Warning/Pending:** Yellow (#F59E0B)
- **Error/Denied:** Red (#EF4444)
- **Background:** White/Light gray
- **Text:** Dark gray/Black

### Typography:
- **Page Titles:** Large, bold (24-32px)
- **Section Headers:** Medium, bold (18-20px)
- **Body Text:** Regular (16px)
- **Small Text/Metadata:** Smaller (14px)

### Spacing:
- **Between sections:** 32px
- **Between cards:** 16px
- **Card padding:** 16px
- **Button padding:** 12px vertical, 24px horizontal

### Components:
- **Buttons:** Rounded corners (8px), clear labels
- **Cards:** White background, subtle shadow, rounded (8px)
- **Inputs:** Border, focus ring, clear placeholders
- **Modals:** Dark overlay (80% opacity), centered, max-width 800px

---

## 🔄 Common User Flows Explained Simply

### Flow 1: Finding and Saving Content
1. User goes to Dashboard
2. Clicks "Discover Content"
3. Types "#summer2024" in search box
4. Clicks Search
5. Sees grid of Instagram posts
6. Clicks on a post they like
7. Popup shows full details
8. Clicks "Save to Collection"
9. Chooses "Summer Campaign" collection
10. Content is saved!

### Flow 2: Getting Permission to Use Content
1. User finds content they want to use
2. Clicks "Request Permission"
3. Popup form appears
4. Chooses to contact via Email
5. Enters creator's email
6. Selects "Standard UGC License" template
7. Checks boxes for how they'll use it (website, social media)
8. Sets expiration date
9. Reviews the message that will be sent
10. Clicks "Send Request"
11. Status changes to "Pending"
12. Creator receives email
13. When creator approves, status changes to "Approved"
14. User can now use the content!

### Flow 3: Downloading Content
1. User goes to Content Library
2. Selects multiple posts (checkboxes)
3. Clicks "Export" button
4. Popup appears with options
5. Chooses "Web optimized" format
6. Checks boxes for metadata to include
7. Chooses "Download ZIP file"
8. Clicks "Export"
9. Progress bar shows download progress
10. ZIP file downloads to computer
11. User can now use content in marketing materials!

---

## 💡 Key Design Principles

1. **Simple and Clear:** Every screen has a clear purpose
2. **Visual First:** Content thumbnails are prominent
3. **Quick Actions:** Common tasks are easy to find
4. **Status Visibility:** Permission status is always visible
5. **Mobile Friendly:** Works well on phones and tablets
6. **Fast:** Loading states and progress indicators
7. **Helpful:** Tooltips and hints where needed

---

## 🛠️ For Developers

### Technical Notes:
- Use React components for reusable UI elements
- Implement lazy loading for images
- Use infinite scroll or pagination for large lists
- Cache API responses to reduce loading time
- Show loading spinners during API calls
- Handle errors gracefully with user-friendly messages
- Make all interactive elements keyboard accessible
- Ensure color contrast meets accessibility standards

### Component Structure:
- **Header Component:** Logo, navigation, user menu (used on all pages)
- **StatsCard Component:** Reusable for dashboard statistics
- **ContentGrid Component:** Displays content in grid layout
- **ContentCard Component:** Individual content item display
- **Modal Component:** Reusable popup/modal wrapper
- **FilterPanel Component:** Search and filter controls
- **PermissionBadge Component:** Shows permission status

---

This document should give developers a clear picture of what to build. Each screen is described in plain language with explanations of what users see and how they interact with it.
