# Component Specifications

Detailed React component breakdown for the Instagram UGC Platform frontend.

## Component Architecture

```
src/
├── components/
│   ├── common/          # Reusable UI components
│   ├── layout/          # Layout components
│   ├── content/         # Content-related components
│   ├── collections/     # Collection components
│   ├── permissions/     # Permission components
│   └── export/          # Export components
├── pages/               # Page components
├── hooks/               # Custom React hooks
├── services/            # API service layer
├── store/               # State management
└── utils/               # Utility functions
```

---

## Common Components

### Button
**Purpose:** Reusable button component with variants

**Props:**
```typescript
interface ButtonProps {
  variant?: 'primary' | 'secondary' | 'outline' | 'text';
  size?: 'sm' | 'md' | 'lg';
  disabled?: boolean;
  loading?: boolean;
  icon?: ReactNode;
  children: ReactNode;
  onClick?: () => void;
  type?: 'button' | 'submit' | 'reset';
}
```

**Variants:**
- `primary`: Solid background, primary color
- `secondary`: Solid background, secondary color
- `outline`: Outlined border, transparent background
- `text`: Text only, no border/background

**Usage:**
```tsx
<Button variant="primary" onClick={handleClick}>
  Save Content
</Button>
```

---

### Card
**Purpose:** Container component for content cards

**Props:**
```typescript
interface CardProps {
  children: ReactNode;
  hover?: boolean;
  onClick?: () => void;
  className?: string;
}
```

**Usage:**
```tsx
<Card hover onClick={handleClick}>
  <ContentThumbnail />
</Card>
```

---

### Modal
**Purpose:** Reusable modal/popup component

**Props:**
```typescript
interface ModalProps {
  isOpen: boolean;
  onClose: () => void;
  title?: string;
  size?: 'sm' | 'md' | 'lg' | 'xl';
  children: ReactNode;
  footer?: ReactNode;
}
```

**Features:**
- Dark overlay backdrop
- Close on overlay click (optional)
- Close on Escape key
- Focus trap for accessibility
- Smooth animations

**Usage:**
```tsx
<Modal isOpen={isOpen} onClose={handleClose} title="Export Content">
  <ExportForm />
</Modal>
```

---

### Input
**Purpose:** Form input component

**Props:**
```typescript
interface InputProps {
  type?: 'text' | 'email' | 'password' | 'number' | 'date';
  label?: string;
  placeholder?: string;
  value: string;
  onChange: (value: string) => void;
  error?: string;
  disabled?: boolean;
  required?: boolean;
  icon?: ReactNode;
}
```

**Usage:**
```tsx
<Input
  label="Collection Name"
  value={name}
  onChange={setName}
  placeholder="Enter collection name"
  required
/>
```

---

### SearchBar
**Purpose:** Search input with icon and clear button

**Props:**
```typescript
interface SearchBarProps {
  value: string;
  onChange: (value: string) => void;
  placeholder?: string;
  onSearch?: () => void;
  debounceMs?: number;
}
```

**Features:**
- Debounced input (300ms default)
- Clear button when has value
- Search icon

---

### FilterPanel
**Purpose:** Collapsible filter panel

**Props:**
```typescript
interface FilterPanelProps {
  filters: FilterState;
  onFilterChange: (filters: FilterState) => void;
  onClear: () => void;
}
```

**Filter Types:**
- Content type (checkboxes)
- Date range (date pickers)
- Engagement (number input)
- Permission status (dropdown)

---

### StatsCard
**Purpose:** Dashboard statistics card

**Props:**
```typescript
interface StatsCardProps {
  label: string;
  value: string | number;
  icon?: ReactNode;
  trend?: {
    value: number;
    direction: 'up' | 'down';
  };
  onClick?: () => void;
}
```

**Usage:**
```tsx
<StatsCard
  label="Content Items"
  value={1234}
  icon={<ImageIcon />}
  trend={{ value: 12, direction: 'up' }}
/>
```

---

### ContentGrid
**Purpose:** Responsive grid layout for content items

**Props:**
```typescript
interface ContentGridProps {
  items: Content[];
  loading?: boolean;
  onItemClick: (item: Content) => void;
  onLoadMore?: () => void;
  hasMore?: boolean;
  columns?: {
    mobile: number;
    tablet: number;
    desktop: number;
  };
}
```

**Features:**
- Responsive columns (2/3/6)
- Lazy loading images
- Infinite scroll option
- Loading skeleton states

---

### ContentCard
**Purpose:** Individual content item card

**Props:**
```typescript
interface ContentCardProps {
  content: Content;
  selected?: boolean;
  onSelect?: (id: string) => void;
  onClick: () => void;
  showPermissionStatus?: boolean;
}
```

**Displays:**
- Thumbnail image
- Content type icon
- Engagement count
- Permission status badge
- Selection checkbox (if selectable)

---

### PermissionBadge
**Purpose:** Visual indicator for permission status

**Props:**
```typescript
interface PermissionBadgeProps {
  status: 'approved' | 'pending' | 'denied' | 'expired';
  expiresAt?: Date;
  size?: 'sm' | 'md' | 'lg';
}
```

**Visual States:**
- ✅ Approved: Green badge
- ⏳ Pending: Yellow badge
- ❌ Denied: Red badge
- ⏰ Expired: Gray badge

---

### LoadingSpinner
**Purpose:** Loading indicator

**Props:**
```typescript
interface LoadingSpinnerProps {
  size?: 'sm' | 'md' | 'lg';
  fullScreen?: boolean;
  message?: string;
}
```

---

### EmptyState
**Purpose:** Empty state when no content/data

**Props:**
```typescript
interface EmptyStateProps {
  icon?: ReactNode;
  title: string;
  message: string;
  action?: {
    label: string;
    onClick: () => void;
  };
}
```

---

## Layout Components

### Header
**Purpose:** Top navigation bar

**Props:**
```typescript
interface HeaderProps {
  user: User;
  onNavigate: (path: string) => void;
}
```

**Contains:**
- Logo (links to dashboard)
- Navigation menu
- User avatar dropdown
- Settings button

---

### Sidebar
**Purpose:** Left sidebar navigation

**Props:**
```typescript
interface SidebarProps {
  collections: Collection[];
  selectedCollectionId?: string;
  onCollectionSelect: (id: string) => void;
  onCreateCollection: () => void;
}
```

**Contains:**
- Collections list
- Tags list
- Create collection button

**Mobile:** Collapses to hamburger menu

---

### PageLayout
**Purpose:** Main page wrapper

**Props:**
```typescript
interface PageLayoutProps {
  children: ReactNode;
  title?: string;
  actions?: ReactNode;
}
```

**Structure:**
- Header (always visible)
- Sidebar (desktop) / Menu (mobile)
- Main content area
- Footer (optional)

---

## Content Components

### ContentDetailModal
**Purpose:** Full content detail view popup

**Props:**
```typescript
interface ContentDetailModalProps {
  contentId: string;
  isOpen: boolean;
  onClose: () => void;
  onSave?: () => void;
  onRequestPermission?: () => void;
  onExport?: () => void;
}
```

**Sections:**
- Large media display (left)
- Content details (right)
  - Creator info
  - Engagement stats
  - Caption
  - Hashtags
  - Permission status card
  - Collections
  - Tags
  - Usage history
  - Action buttons

---

### ContentDiscoveryPage
**Purpose:** Search and discover Instagram content

**State:**
```typescript
interface DiscoveryState {
  query: string;
  filters: FilterState;
  results: Content[];
  loading: boolean;
  pagination: Pagination;
}
```

**Components Used:**
- SearchBar
- FilterPanel
- ContentGrid
- LoadingSpinner
- EmptyState

---

### ContentLibraryPage
**Purpose:** View and manage saved content

**State:**
```typescript
interface LibraryState {
  view: 'grid' | 'list';
  sort: 'newest' | 'oldest' | 'engagement';
  filters: {
    collectionId?: string;
    tag?: string;
    permissionStatus?: string;
  };
  selectedItems: string[];
  content: Content[];
}
```

**Features:**
- Grid/List view toggle
- Sort options
- Filter by collection/tag/status
- Bulk selection
- Bulk actions

---

## Collection Components

### CollectionCard
**Purpose:** Collection preview card

**Props:**
```typescript
interface CollectionCardProps {
  collection: Collection;
  onClick: () => void;
  onEdit?: () => void;
  onDelete?: () => void;
}
```

**Displays:**
- Collection name
- Thumbnail grid (6 images)
- Item count
- Created date
- Action buttons

---

### CollectionFormModal
**Purpose:** Create/edit collection form

**Props:**
```typescript
interface CollectionFormModalProps {
  isOpen: boolean;
  onClose: () => void;
  onSubmit: (data: CollectionFormData) => void;
  initialData?: Collection;
}
```

**Form Fields:**
- Name (required)
- Description (optional)
- Add content now (checkbox)
- Content selector (if checked)

---

### CollectionsPage
**Purpose:** Manage all collections

**Features:**
- Grid of collection cards
- Create new collection
- Edit collection
- Delete collection
- View collection contents

---

## Permission Components

### PermissionDashboard
**Purpose:** View and manage permission requests

**Props:**
```typescript
interface PermissionDashboardProps {
  filters: {
    status?: string;
    creator?: string;
    dateRange?: DateRange;
  };
}
```

**Features:**
- Tabs for status filtering
- Permission request cards
- Filter panel
- Bulk actions (remind, cancel)

---

### PermissionRequestCard
**Purpose:** Individual permission request display

**Props:**
```typescript
interface PermissionRequestCardProps {
  permission: Permission;
  onView: () => void;
  onRemind: () => void;
  onCancel: () => void;
}
```

**Displays:**
- Content thumbnail
- Creator username
- Status badge
- Request date
- Contact method
- Days pending
- Action buttons

---

### PermissionRequestModal
**Purpose:** Form to request permission

**Props:**
```typescript
interface PermissionRequestModalProps {
  contentId: string;
  isOpen: boolean;
  onClose: () => void;
  onSubmit: (data: PermissionRequestData) => void;
}
```

**Form Fields:**
- Contact method (radio)
- Creator email (if email selected)
- Permission template (dropdown)
- Usage rights (checkboxes)
- Expiration date (date picker)
- Message preview (textarea)

---

## Export Components

### ExportModal
**Purpose:** Export content configuration

**Props:**
```typescript
interface ExportModalProps {
  contentIds: string[];
  isOpen: boolean;
  onClose: () => void;
  onExport: (options: ExportOptions) => void;
}
```

**Form Sections:**
- Export format (radio)
- Metadata options (checkboxes)
- Metadata format (radio)
- Delivery method (radio)

---

### ExportProgress
**Purpose:** Show export progress

**Props:**
```typescript
interface ExportProgressProps {
  exportId: string;
  progress: number;
  status: 'processing' | 'completed' | 'failed';
  downloadUrl?: string;
}
```

**Displays:**
- Progress bar
- Item-by-item status
- Download button (when complete)
- Error message (if failed)

---

## Dashboard Components

### DashboardPage
**Purpose:** Main dashboard/home page

**Components:**
- StatsCard (4 cards)
- ActivityFeed
- QuickActions
- TopCollections

---

### ActivityFeed
**Purpose:** Recent activity timeline

**Props:**
```typescript
interface ActivityFeedProps {
  activities: Activity[];
  limit?: number;
}
```

**Activity Types:**
- Content discovered
- Permission approved/denied
- Content exported
- Collection created
- Content saved

---

### QuickActions
**Purpose:** Quick action buttons

**Actions:**
- Discover Content
- View Library
- New Collection

---

### TopCollections
**Purpose:** Preview of top collections

**Props:**
```typescript
interface TopCollectionsProps {
  collections: Collection[];
  limit?: number;
  onCollectionClick: (id: string) => void;
}
```

---

## Hooks

### useContent
**Purpose:** Fetch and manage content

```typescript
function useContent(filters?: ContentFilters) {
  return {
    content: Content[];
    loading: boolean;
    error: Error | null;
    refetch: () => void;
    loadMore: () => void;
    hasMore: boolean;
  };
}
```

---

### useCollections
**Purpose:** Manage collections

```typescript
function useCollections() {
  return {
    collections: Collection[];
    loading: boolean;
    createCollection: (data: CollectionFormData) => Promise<void>;
    updateCollection: (id: string, data: CollectionFormData) => Promise<void>;
    deleteCollection: (id: string) => Promise<void>;
  };
}
```

---

### usePermissions
**Purpose:** Manage permissions

```typescript
function usePermissions(filters?: PermissionFilters) {
  return {
    permissions: Permission[];
    loading: boolean;
    requestPermission: (data: PermissionRequestData) => Promise<void>;
    remindCreator: (id: string) => Promise<void>;
    cancelRequest: (id: string) => Promise<void>;
  };
}
```

---

### useExport
**Purpose:** Handle exports

```typescript
function useExport() {
  return {
    export: (contentIds: string[], options: ExportOptions) => Promise<string>;
    getExportStatus: (exportId: string) => Promise<ExportStatus>;
  };
}
```

---

### useDebounce
**Purpose:** Debounce values

```typescript
function useDebounce<T>(value: T, delay: number): T;
```

---

### useInfiniteScroll
**Purpose:** Infinite scroll detection

```typescript
function useInfiniteScroll(
  callback: () => void,
  hasMore: boolean,
  loading: boolean
): React.RefObject<HTMLDivElement>;
```

---

## State Management

### Zustand Stores

**useAuthStore:**
```typescript
interface AuthStore {
  user: User | null;
  token: string | null;
  login: (credentials: LoginCredentials) => Promise<void>;
  logout: () => void;
  isAuthenticated: boolean;
}
```

**useUIControlStore:**
```typescript
interface UIControlStore {
  modals: {
    contentDetail: { isOpen: boolean; contentId?: string };
    export: { isOpen: boolean; contentIds: string[] };
    permissionRequest: { isOpen: boolean; contentId?: string };
  };
  openModal: (modal: string, data?: any) => void;
  closeModal: (modal: string) => void;
  sidebarOpen: boolean;
  toggleSidebar: () => void;
}
```

**useSelectionStore:**
```typescript
interface SelectionStore {
  selectedItems: string[];
  selectItem: (id: string) => void;
  deselectItem: (id: string) => void;
  selectAll: (ids: string[]) => void;
  clearSelection: () => void;
  isSelected: (id: string) => boolean;
}
```

---

## Styling

### Tailwind CSS Classes

**Colors:**
- Primary: `bg-gradient-to-r from-purple-500 via-pink-500 to-orange-500`
- Success: `bg-green-500`
- Warning: `bg-yellow-500`
- Error: `bg-red-500`

**Spacing:**
- Card padding: `p-4`
- Section spacing: `space-y-8`
- Grid gap: `gap-4`

**Components:**
- Cards: `bg-white rounded-lg shadow-md`
- Buttons: `px-6 py-3 rounded-lg font-medium`
- Inputs: `border border-gray-300 rounded-lg px-4 py-2`

---

## Component Hierarchy Example

```
App
├── Router
│   ├── DashboardPage
│   │   ├── Header
│   │   ├── StatsCard (×4)
│   │   ├── ActivityFeed
│   │   ├── QuickActions
│   │   └── TopCollections
│   │
│   ├── ContentDiscoveryPage
│   │   ├── Header
│   │   ├── SearchBar
│   │   ├── FilterPanel
│   │   └── ContentGrid
│   │       └── ContentCard (×N)
│   │
│   └── ContentLibraryPage
│       ├── Header
│       ├── Sidebar
│       └── ContentGrid
│           └── ContentCard (×N)
│
└── Modals (Portal)
    ├── ContentDetailModal
    ├── ExportModal
    ├── PermissionRequestModal
    └── CollectionFormModal
```

---

## Performance Considerations

1. **Lazy Loading:**
   - Code splitting for pages
   - React.lazy() for heavy components
   - Image lazy loading with Intersection Observer

2. **Memoization:**
   - React.memo() for expensive components
   - useMemo() for computed values
   - useCallback() for event handlers

3. **Virtualization:**
   - react-window for long lists
   - Only render visible items

4. **Caching:**
   - React Query for API caching
   - Local storage for user preferences

---

## Accessibility

1. **Keyboard Navigation:**
   - Tab order
   - Enter/Space for actions
   - Escape to close modals
   - Arrow keys for navigation

2. **Screen Readers:**
   - ARIA labels
   - Semantic HTML
   - Alt text for images

3. **Focus Management:**
   - Focus trap in modals
   - Focus restoration on close
   - Visible focus indicators

4. **Color Contrast:**
   - WCAG AA compliance
   - Not relying on color alone

