# Database Schema

Complete database schema for the Instagram UGC Platform.

## Database: PostgreSQL 15+

---

## Tables

### users
Stores user account information.

```sql
CREATE TABLE users (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    email VARCHAR(255) UNIQUE NOT NULL,
    name VARCHAR(255) NOT NULL,
    password_hash VARCHAR(255), -- Nullable if using OAuth only
    role VARCHAR(50) NOT NULL DEFAULT 'member', -- admin, member, viewer
    instagram_user_id VARCHAR(255), -- Instagram account ID
    instagram_username VARCHAR(255),
    instagram_access_token TEXT, -- Encrypted
    instagram_token_expires_at TIMESTAMP,
    created_at TIMESTAMP NOT NULL DEFAULT NOW(),
    updated_at TIMESTAMP NOT NULL DEFAULT NOW(),
    last_login_at TIMESTAMP
);

CREATE INDEX idx_users_email ON users(email);
CREATE INDEX idx_users_instagram_user_id ON users(instagram_user_id);
```

### teams
Stores team/organization information.

```sql
CREATE TABLE teams (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    name VARCHAR(255) NOT NULL,
    plan VARCHAR(50) NOT NULL DEFAULT 'free', -- free, pro, enterprise
    created_at TIMESTAMP NOT NULL DEFAULT NOW(),
    updated_at TIMESTAMP NOT NULL DEFAULT NOW()
);
```

### team_members
Junction table for users and teams.

```sql
CREATE TABLE team_members (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    team_id UUID NOT NULL REFERENCES teams(id) ON DELETE CASCADE,
    user_id UUID NOT NULL REFERENCES users(id) ON DELETE CASCADE,
    role VARCHAR(50) NOT NULL DEFAULT 'member', -- admin, member, viewer
    joined_at TIMESTAMP NOT NULL DEFAULT NOW(),
    UNIQUE(team_id, user_id)
);

CREATE INDEX idx_team_members_team_id ON team_members(team_id);
CREATE INDEX idx_team_members_user_id ON team_members(user_id);
```

### content
Stores Instagram content items.

```sql
CREATE TABLE content (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    instagram_id VARCHAR(255) NOT NULL UNIQUE,
    instagram_media_url TEXT NOT NULL,
    type VARCHAR(50) NOT NULL, -- photo, video, reel, carousel
    thumbnail_url TEXT,
    full_image_url TEXT, -- Downloaded/cached version
    caption TEXT,
    creator_username VARCHAR(255) NOT NULL,
    creator_email VARCHAR(255),
    creator_profile_picture TEXT,
    engagement_likes INTEGER DEFAULT 0,
    engagement_comments INTEGER DEFAULT 0,
    posted_at TIMESTAMP NOT NULL,
    location_name VARCHAR(255),
    instagram_url TEXT NOT NULL,
    saved_by_user_id UUID NOT NULL REFERENCES users(id),
    saved_at TIMESTAMP NOT NULL DEFAULT NOW(),
    permission_status VARCHAR(50) DEFAULT 'pending', -- pending, approved, denied, expired
    permission_expires_at TIMESTAMP,
    usage_count INTEGER DEFAULT 0,
    metadata JSONB, -- Flexible storage for hashtags, mentions, etc.
    created_at TIMESTAMP NOT NULL DEFAULT NOW(),
    updated_at TIMESTAMP NOT NULL DEFAULT NOW()
);

CREATE INDEX idx_content_saved_by_user_id ON content(saved_by_user_id);
CREATE INDEX idx_content_permission_status ON content(permission_status);
CREATE INDEX idx_content_posted_at ON content(posted_at);
CREATE INDEX idx_content_instagram_id ON content(instagram_id);
CREATE INDEX idx_content_metadata ON content USING GIN(metadata);
```

### collections
Stores content collections/folders.

```sql
CREATE TABLE collections (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    name VARCHAR(255) NOT NULL,
    description TEXT,
    created_by_user_id UUID NOT NULL REFERENCES users(id),
    team_id UUID REFERENCES teams(id) ON DELETE CASCADE,
    item_count INTEGER DEFAULT 0,
    created_at TIMESTAMP NOT NULL DEFAULT NOW(),
    updated_at TIMESTAMP NOT NULL DEFAULT NOW()
);

CREATE INDEX idx_collections_created_by_user_id ON collections(created_by_user_id);
CREATE INDEX idx_collections_team_id ON collections(team_id);
```

### collection_content
Junction table for collections and content (many-to-many).

```sql
CREATE TABLE collection_content (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    collection_id UUID NOT NULL REFERENCES collections(id) ON DELETE CASCADE,
    content_id UUID NOT NULL REFERENCES content(id) ON DELETE CASCADE,
    added_at TIMESTAMP NOT NULL DEFAULT NOW(),
    added_by_user_id UUID NOT NULL REFERENCES users(id),
    UNIQUE(collection_id, content_id)
);

CREATE INDEX idx_collection_content_collection_id ON collection_content(collection_id);
CREATE INDEX idx_collection_content_content_id ON collection_content(content_id);
```

### permissions
Stores permission requests and status.

```sql
CREATE TABLE permissions (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    content_id UUID NOT NULL REFERENCES content(id) ON DELETE CASCADE,
    creator_username VARCHAR(255) NOT NULL,
    creator_email VARCHAR(255),
    contact_method VARCHAR(50) NOT NULL, -- email, instagram_dm, both
    status VARCHAR(50) NOT NULL DEFAULT 'pending', -- pending, approved, denied, expired, cancelled
    requested_by_user_id UUID NOT NULL REFERENCES users(id),
    requested_at TIMESTAMP NOT NULL DEFAULT NOW(),
    approved_at TIMESTAMP,
    denied_at TIMESTAMP,
    expires_at TIMESTAMP,
    usage_rights TEXT[], -- Array of rights: social_media, website, email, advertising, print
    template_id VARCHAR(255), -- Reference to permission template
    custom_message TEXT,
    message_sent TEXT, -- Copy of message sent to creator
    reminder_sent_at TIMESTAMP,
    reminder_count INTEGER DEFAULT 0,
    notes TEXT,
    created_at TIMESTAMP NOT NULL DEFAULT NOW(),
    updated_at TIMESTAMP NOT NULL DEFAULT NOW()
);

CREATE INDEX idx_permissions_content_id ON permissions(content_id);
CREATE INDEX idx_permissions_status ON permissions(status);
CREATE INDEX idx_permissions_requested_by_user_id ON permissions(requested_by_user_id);
CREATE INDEX idx_permissions_creator_email ON permissions(creator_email);
CREATE INDEX idx_permissions_expires_at ON permissions(expires_at);
```

### permission_templates
Stores reusable permission request templates.

```sql
CREATE TABLE permission_templates (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    name VARCHAR(255) NOT NULL,
    description TEXT,
    default_usage_rights TEXT[],
    default_expiration_days INTEGER DEFAULT 365,
    message_template TEXT NOT NULL,
    created_by_user_id UUID REFERENCES users(id),
    team_id UUID REFERENCES teams(id) ON DELETE CASCADE,
    is_default BOOLEAN DEFAULT FALSE,
    created_at TIMESTAMP NOT NULL DEFAULT NOW(),
    updated_at TIMESTAMP NOT NULL DEFAULT NOW()
);

CREATE INDEX idx_permission_templates_team_id ON permission_templates(team_id);
```

### tags
Stores content tags.

```sql
CREATE TABLE tags (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    name VARCHAR(100) NOT NULL UNIQUE,
    created_at TIMESTAMP NOT NULL DEFAULT NOW()
);

CREATE INDEX idx_tags_name ON tags(name);
```

### content_tags
Junction table for content and tags.

```sql
CREATE TABLE content_tags (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    content_id UUID NOT NULL REFERENCES content(id) ON DELETE CASCADE,
    tag_id UUID NOT NULL REFERENCES tags(id) ON DELETE CASCADE,
    added_by_user_id UUID NOT NULL REFERENCES users(id),
    added_at TIMESTAMP NOT NULL DEFAULT NOW(),
    UNIQUE(content_id, tag_id)
);

CREATE INDEX idx_content_tags_content_id ON content_tags(content_id);
CREATE INDEX idx_content_tags_tag_id ON content_tags(tag_id);
```

### usage_log
Tracks where and when content is used.

```sql
CREATE TABLE usage_log (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    content_id UUID NOT NULL REFERENCES content(id) ON DELETE CASCADE,
    used_by_user_id UUID NOT NULL REFERENCES users(id),
    usage_type VARCHAR(100) NOT NULL, -- email_campaign, social_post, website, ad, print
    campaign_name VARCHAR(255),
    project_name VARCHAR(255),
    notes TEXT,
    used_at TIMESTAMP NOT NULL DEFAULT NOW(),
    created_at TIMESTAMP NOT NULL DEFAULT NOW()
);

CREATE INDEX idx_usage_log_content_id ON usage_log(content_id);
CREATE INDEX idx_usage_log_used_by_user_id ON usage_log(used_by_user_id);
CREATE INDEX idx_usage_log_used_at ON usage_log(used_at);
```

### exports
Tracks export jobs.

```sql
CREATE TABLE exports (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    requested_by_user_id UUID NOT NULL REFERENCES users(id),
    status VARCHAR(50) NOT NULL DEFAULT 'pending', -- pending, processing, completed, failed
    content_ids UUID[] NOT NULL, -- Array of content IDs
    format VARCHAR(50) NOT NULL, -- original, compressed, web_optimized
    include_metadata BOOLEAN DEFAULT TRUE,
    metadata_format VARCHAR(50), -- json, csv, exif
    delivery_method VARCHAR(50) NOT NULL, -- download, email, cloud_storage
    download_url TEXT,
    expires_at TIMESTAMP,
    progress INTEGER DEFAULT 0, -- 0-100
    error_message TEXT,
    created_at TIMESTAMP NOT NULL DEFAULT NOW(),
    updated_at TIMESTAMP NOT NULL DEFAULT NOW(),
    completed_at TIMESTAMP
);

CREATE INDEX idx_exports_requested_by_user_id ON exports(requested_by_user_id);
CREATE INDEX idx_exports_status ON exports(status);
```

### activity_log
General activity log for dashboard feed.

```sql
CREATE TABLE activity_log (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    user_id UUID NOT NULL REFERENCES users(id),
    team_id UUID REFERENCES teams(id),
    activity_type VARCHAR(100) NOT NULL, -- content_discovered, permission_approved, content_exported, etc.
    entity_type VARCHAR(100), -- content, permission, collection, etc.
    entity_id UUID,
    message TEXT NOT NULL,
    metadata JSONB,
    created_at TIMESTAMP NOT NULL DEFAULT NOW()
);

CREATE INDEX idx_activity_log_user_id ON activity_log(user_id);
CREATE INDEX idx_activity_log_team_id ON activity_log(team_id);
CREATE INDEX idx_activity_log_created_at ON activity_log(created_at);
CREATE INDEX idx_activity_log_activity_type ON activity_log(activity_type);
```

### api_rate_limits
Tracks API rate limiting per user.

```sql
CREATE TABLE api_rate_limits (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    user_id UUID NOT NULL REFERENCES users(id),
    endpoint VARCHAR(255) NOT NULL,
    request_count INTEGER DEFAULT 1,
    window_start TIMESTAMP NOT NULL,
    created_at TIMESTAMP NOT NULL DEFAULT NOW(),
    UNIQUE(user_id, endpoint, window_start)
);

CREATE INDEX idx_api_rate_limits_user_id ON api_rate_limits(user_id);
CREATE INDEX idx_api_rate_limits_window_start ON api_rate_limits(window_start);
```

---

## Views

### content_with_permission_status
View combining content with latest permission status.

```sql
CREATE VIEW content_with_permission_status AS
SELECT 
    c.*,
    p.status as permission_status,
    p.expires_at as permission_expires_at,
    p.approved_at as permission_approved_at
FROM content c
LEFT JOIN LATERAL (
    SELECT * FROM permissions 
    WHERE content_id = c.id 
    ORDER BY requested_at DESC 
    LIMIT 1
) p ON TRUE;
```

### collection_stats
View for collection statistics.

```sql
CREATE VIEW collection_stats AS
SELECT 
    col.id,
    col.name,
    col.created_at,
    COUNT(cc.content_id) as item_count,
    COUNT(DISTINCT c.permission_status) FILTER (WHERE c.permission_status = 'approved') as approved_count
FROM collections col
LEFT JOIN collection_content cc ON col.id = cc.collection_id
LEFT JOIN content c ON cc.content_id = c.id
GROUP BY col.id, col.name, col.created_at;
```

---

## Functions & Triggers

### Update updated_at timestamp

```sql
CREATE OR REPLACE FUNCTION update_updated_at_column()
RETURNS TRIGGER AS $$
BEGIN
    NEW.updated_at = NOW();
    RETURN NEW;
END;
$$ language 'plpgsql';

-- Apply to all tables with updated_at
CREATE TRIGGER update_users_updated_at BEFORE UPDATE ON users
    FOR EACH ROW EXECUTE FUNCTION update_updated_at_column();

CREATE TRIGGER update_content_updated_at BEFORE UPDATE ON content
    FOR EACH ROW EXECUTE FUNCTION update_updated_at_column();

CREATE TRIGGER update_collections_updated_at BEFORE UPDATE ON collections
    FOR EACH ROW EXECUTE FUNCTION update_updated_at_column();

CREATE TRIGGER update_permissions_updated_at BEFORE UPDATE ON permissions
    FOR EACH ROW EXECUTE FUNCTION update_updated_at_column();
```

### Update collection item count

```sql
CREATE OR REPLACE FUNCTION update_collection_item_count()
RETURNS TRIGGER AS $$
BEGIN
    IF TG_OP = 'INSERT' THEN
        UPDATE collections 
        SET item_count = item_count + 1 
        WHERE id = NEW.collection_id;
        RETURN NEW;
    ELSIF TG_OP = 'DELETE' THEN
        UPDATE collections 
        SET item_count = item_count - 1 
        WHERE id = OLD.collection_id;
        RETURN OLD;
    END IF;
END;
$$ language 'plpgsql';

CREATE TRIGGER update_collection_count_on_insert
    AFTER INSERT ON collection_content
    FOR EACH ROW EXECUTE FUNCTION update_collection_item_count();

CREATE TRIGGER update_collection_count_on_delete
    AFTER DELETE ON collection_content
    FOR EACH ROW EXECUTE FUNCTION update_collection_item_count();
```

### Update content usage count

```sql
CREATE OR REPLACE FUNCTION update_content_usage_count()
RETURNS TRIGGER AS $$
BEGIN
    UPDATE content 
    SET usage_count = usage_count + 1 
    WHERE id = NEW.content_id;
    RETURN NEW;
END;
$$ language 'plpgsql';

CREATE TRIGGER update_usage_count_on_insert
    AFTER INSERT ON usage_log
    FOR EACH ROW EXECUTE FUNCTION update_content_usage_count();
```

### Log activity on permission status change

```sql
CREATE OR REPLACE FUNCTION log_permission_activity()
RETURNS TRIGGER AS $$
BEGIN
    IF OLD.status != NEW.status THEN
        INSERT INTO activity_log (user_id, activity_type, entity_type, entity_id, message)
        VALUES (
            NEW.requested_by_user_id,
            'permission_' || NEW.status,
            'permission',
            NEW.id,
            'Permission ' || NEW.status || ' for content from @' || NEW.creator_username
        );
    END IF;
    RETURN NEW;
END;
$$ language 'plpgsql';

CREATE TRIGGER log_permission_status_change
    AFTER UPDATE ON permissions
    FOR EACH ROW EXECUTE FUNCTION log_permission_activity();
```

---

## Indexes Summary

**Content:**
- Index on `saved_by_user_id` for user's content queries
- Index on `permission_status` for filtering by status
- Index on `posted_at` for date range queries
- GIN index on `metadata` JSONB for flexible queries

**Permissions:**
- Index on `status` for status filtering
- Index on `expires_at` for finding expired permissions
- Index on `creator_email` for creator lookups

**Collections:**
- Index on `created_by_user_id` for user's collections
- Index on `team_id` for team collections

**Activity Log:**
- Index on `created_at` for recent activity queries
- Index on `activity_type` for filtering by type

---

## Data Relationships

```
users
  ├── content (saved_by_user_id)
  ├── collections (created_by_user_id)
  ├── permissions (requested_by_user_id)
  └── team_members (user_id)

teams
  ├── team_members (team_id)
  ├── collections (team_id)
  └── permission_templates (team_id)

content
  ├── collection_content (content_id)
  ├── permissions (content_id)
  ├── content_tags (content_id)
  └── usage_log (content_id)

collections
  └── collection_content (collection_id)

permissions
  └── content (content_id)

tags
  └── content_tags (tag_id)
```

---

## Sample Queries

### Get user's content with permission status
```sql
SELECT 
    c.*,
    p.status as permission_status,
    p.expires_at
FROM content c
LEFT JOIN permissions p ON c.id = p.content_id
WHERE c.saved_by_user_id = $1
ORDER BY c.saved_at DESC;
```

### Get collection with content
```sql
SELECT 
    col.*,
    json_agg(
        json_build_object(
            'id', c.id,
            'thumbnail_url', c.thumbnail_url,
            'permission_status', c.permission_status
        )
    ) as content
FROM collections col
LEFT JOIN collection_content cc ON col.id = cc.collection_id
LEFT JOIN content c ON cc.content_id = c.id
WHERE col.id = $1
GROUP BY col.id;
```

### Get pending permissions
```sql
SELECT 
    p.*,
    c.thumbnail_url,
    c.caption
FROM permissions p
JOIN content c ON p.content_id = c.id
WHERE p.status = 'pending'
  AND p.requested_by_user_id = $1
ORDER BY p.requested_at DESC;
```

### Get dashboard stats
```sql
SELECT 
    (SELECT COUNT(*) FROM content WHERE saved_by_user_id = $1) as total_content,
    (SELECT COUNT(*) FROM collections WHERE created_by_user_id = $1) as total_collections,
    (SELECT COUNT(*) FROM permissions WHERE status = 'approved' AND requested_by_user_id = $1)::float / 
    NULLIF((SELECT COUNT(*) FROM permissions WHERE requested_by_user_id = $1), 0) as approval_rate,
    (SELECT COUNT(*) FROM permissions WHERE status = 'pending' AND requested_by_user_id = $1) as pending_requests;
```

