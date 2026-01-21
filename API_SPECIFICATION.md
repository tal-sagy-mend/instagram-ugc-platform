# API Specification

Complete API endpoint documentation for the Instagram UGC Platform backend.

## Base URL
```
https://api.instagram-ugc-platform.com/v1
```

## Authentication
All endpoints require authentication via JWT token in the Authorization header:
```
Authorization: Bearer <jwt_token>
```

---

## Endpoints

### Authentication

#### POST /auth/login
Authenticate user with Instagram/Facebook OAuth.

**Request:**
```json
{
  "provider": "instagram",
  "code": "oauth_code_from_instagram"
}
```

**Response:**
```json
{
  "access_token": "jwt_token_here",
  "refresh_token": "refresh_token_here",
  "user": {
    "id": "user_123",
    "email": "user@example.com",
    "name": "John Doe",
    "role": "admin"
  }
}
```

---

### Content Discovery

#### GET /content/discover
Search Instagram for content.

**Query Parameters:**
- `query` (string, required): Search term (hashtag, mention, keyword)
- `type` (string, optional): `hashtag`, `mention`, `keyword`
- `content_type` (string, optional): `photo`, `video`, `reel`, `all`
- `date_from` (date, optional): ISO 8601 format
- `date_to` (date, optional): ISO 8601 format
- `min_engagement` (integer, optional): Minimum likes/comments
- `page` (integer, optional): Page number (default: 1)
- `per_page` (integer, optional): Items per page (default: 24, max: 100)

**Response:**
```json
{
  "results": [
    {
      "id": "ig_post_123",
      "instagram_id": "1234567890",
      "type": "photo",
      "media_url": "https://instagram.com/p/abc123",
      "thumbnail_url": "https://cdn.example.com/thumb.jpg",
      "caption": "Amazing product! #summer2024",
      "creator": {
        "username": "@creator123",
        "profile_picture": "https://cdn.example.com/profile.jpg"
      },
      "engagement": {
        "likes": 1234,
        "comments": 89
      },
      "posted_at": "2024-01-15T10:30:00Z",
      "hashtags": ["summer", "lifestyle"],
      "location": "New York, NY"
    }
  ],
  "pagination": {
    "page": 1,
    "per_page": 24,
    "total": 1234,
    "total_pages": 52
  }
}
```

#### POST /content/discover/batch
Batch search with multiple queries.

**Request:**
```json
{
  "queries": [
    {"type": "hashtag", "value": "#summer2024"},
    {"type": "mention", "value": "@brandname"}
  ],
  "filters": {
    "content_type": "photo",
    "date_from": "2024-01-01",
    "min_engagement": 100
  }
}
```

**Response:** Same as GET /content/discover

---

### Content Library

#### GET /content
Get all saved content.

**Query Parameters:**
- `collection_id` (string, optional): Filter by collection
- `tag` (string, optional): Filter by tag
- `permission_status` (string, optional): `approved`, `pending`, `denied`, `expired`
- `sort` (string, optional): `newest`, `oldest`, `engagement`
- `page` (integer, optional)
- `per_page` (integer, optional)

**Response:**
```json
{
  "content": [
    {
      "id": "content_123",
      "instagram_id": "1234567890",
      "type": "photo",
      "thumbnail_url": "https://cdn.example.com/thumb.jpg",
      "media_url": "https://instagram.com/p/abc123",
      "caption": "Amazing product!",
      "creator": {
        "username": "@creator123",
        "email": "creator@example.com"
      },
      "engagement": {
        "likes": 1234,
        "comments": 89
      },
      "posted_at": "2024-01-15T10:30:00Z",
      "saved_at": "2024-01-16T08:00:00Z",
      "permission_status": "approved",
      "permission_expires_at": "2024-12-31T23:59:59Z",
      "collections": ["collection_1", "collection_2"],
      "tags": ["summer", "product"],
      "usage_count": 3
    }
  ],
  "pagination": {
    "page": 1,
    "per_page": 24,
    "total": 1234
  }
}
```

#### GET /content/:id
Get single content item details.

**Response:**
```json
{
  "id": "content_123",
  "instagram_id": "1234567890",
  "type": "photo",
  "media_url": "https://cdn.example.com/full.jpg",
  "thumbnail_url": "https://cdn.example.com/thumb.jpg",
  "caption": "Amazing product! #summer2024",
  "creator": {
    "username": "@creator123",
    "email": "creator@example.com",
    "profile_picture": "https://cdn.example.com/profile.jpg"
  },
  "engagement": {
    "likes": 1234,
    "comments": 89
  },
  "posted_at": "2024-01-15T10:30:00Z",
  "saved_at": "2024-01-16T08:00:00Z",
  "hashtags": ["summer", "lifestyle"],
  "location": "New York, NY",
  "permission": {
    "status": "approved",
    "requested_at": "2024-01-16T09:00:00Z",
    "approved_at": "2024-01-17T14:30:00Z",
    "expires_at": "2024-12-31T23:59:59Z",
    "usage_rights": ["social_media", "website", "email"],
    "terms": "Standard UGC License"
  },
  "collections": [
    {
      "id": "collection_1",
      "name": "Summer Campaign 2024"
    }
  ],
  "tags": ["summer", "product", "ugc"],
  "usage_history": [
    {
      "id": "usage_1",
      "type": "email_campaign",
      "campaign_name": "Summer Sale",
      "used_at": "2024-01-20T10:00:00Z"
    }
  ]
}
```

#### POST /content
Save content to library.

**Request:**
```json
{
  "instagram_id": "1234567890",
  "collection_ids": ["collection_1"],
  "tags": ["summer", "product"]
}
```

**Response:**
```json
{
  "id": "content_123",
  "message": "Content saved successfully"
}
```

#### DELETE /content/:id
Remove content from library.

**Response:**
```json
{
  "message": "Content removed successfully"
}
```

#### POST /content/bulk
Bulk operations on content.

**Request:**
```json
{
  "action": "add_to_collection",
  "content_ids": ["content_1", "content_2"],
  "collection_id": "collection_1"
}
```

**Response:**
```json
{
  "success": 2,
  "failed": 0,
  "message": "2 items added to collection"
}
```

---

### Collections

#### GET /collections
Get all collections.

**Response:**
```json
{
  "collections": [
    {
      "id": "collection_1",
      "name": "Summer Campaign 2024",
      "description": "Content for summer marketing campaign",
      "item_count": 234,
      "created_at": "2024-01-01T00:00:00Z",
      "updated_at": "2024-01-20T10:00:00Z",
      "thumbnail_urls": [
        "https://cdn.example.com/thumb1.jpg",
        "https://cdn.example.com/thumb2.jpg"
      ]
    }
  ]
}
```

#### GET /collections/:id
Get collection details with content.

**Query Parameters:**
- `page` (integer, optional)
- `per_page` (integer, optional)

**Response:**
```json
{
  "id": "collection_1",
  "name": "Summer Campaign 2024",
  "description": "Content for summer marketing campaign",
  "item_count": 234,
  "created_at": "2024-01-01T00:00:00Z",
  "content": [
    {
      "id": "content_123",
      "thumbnail_url": "https://cdn.example.com/thumb.jpg",
      "permission_status": "approved"
    }
  ],
  "pagination": {
    "page": 1,
    "per_page": 24,
    "total": 234
  }
}
```

#### POST /collections
Create new collection.

**Request:**
```json
{
  "name": "Summer Campaign 2024",
  "description": "Content for summer marketing campaign",
  "content_ids": ["content_1", "content_2"]
}
```

**Response:**
```json
{
  "id": "collection_1",
  "name": "Summer Campaign 2024",
  "message": "Collection created successfully"
}
```

#### PUT /collections/:id
Update collection.

**Request:**
```json
{
  "name": "Updated Name",
  "description": "Updated description"
}
```

**Response:**
```json
{
  "id": "collection_1",
  "message": "Collection updated successfully"
}
```

#### DELETE /collections/:id
Delete collection.

**Response:**
```json
{
  "message": "Collection deleted successfully"
}
```

---

### Permissions

#### GET /permissions
Get all permission requests.

**Query Parameters:**
- `status` (string, optional): `pending`, `approved`, `denied`, `expired`
- `creator` (string, optional): Filter by creator username
- `date_from` (date, optional)
- `date_to` (date, optional)
- `page` (integer, optional)

**Response:**
```json
{
  "permissions": [
    {
      "id": "perm_123",
      "content_id": "content_123",
      "content_thumbnail": "https://cdn.example.com/thumb.jpg",
      "creator": {
        "username": "@creator123",
        "email": "creator@example.com"
      },
      "status": "pending",
      "requested_at": "2024-01-20T10:00:00Z",
      "contact_method": "email",
      "expires_at": "2024-12-31T23:59:59Z",
      "usage_rights": ["social_media", "website"]
    }
  ],
  "pagination": {
    "page": 1,
    "per_page": 24,
    "total": 89
  },
  "summary": {
    "pending": 12,
    "approved": 89,
    "denied": 5,
    "expired": 3
  }
}
```

#### GET /permissions/:id
Get permission request details.

**Response:**
```json
{
  "id": "perm_123",
  "content": {
    "id": "content_123",
    "thumbnail_url": "https://cdn.example.com/thumb.jpg",
    "caption": "Amazing product!"
  },
  "creator": {
    "username": "@creator123",
    "email": "creator@example.com",
    "profile_picture": "https://cdn.example.com/profile.jpg"
  },
  "status": "approved",
  "requested_at": "2024-01-20T10:00:00Z",
  "approved_at": "2024-01-21T14:30:00Z",
  "expires_at": "2024-12-31T23:59:59Z",
  "contact_method": "email",
  "usage_rights": ["social_media", "website", "email"],
  "terms": "Standard UGC License",
  "message_sent": "Hi @creator123, we love your post..."
}
```

#### POST /permissions
Request permission from creator.

**Request:**
```json
{
  "content_id": "content_123",
  "contact_method": "email",
  "creator_email": "creator@example.com",
  "template_id": "template_standard",
  "usage_rights": ["social_media", "website", "email"],
  "expires_at": "2024-12-31T23:59:59Z",
  "custom_message": "Optional custom message"
}
```

**Response:**
```json
{
  "id": "perm_123",
  "status": "pending",
  "message": "Permission request sent successfully"
}
```

#### POST /permissions/:id/remind
Send reminder to creator.

**Response:**
```json
{
  "message": "Reminder sent successfully"
}
```

#### PUT /permissions/:id/status
Update permission status (when creator responds).

**Request:**
```json
{
  "status": "approved"
}
```

**Response:**
```json
{
  "id": "perm_123",
  "status": "approved",
  "message": "Permission status updated"
}
```

#### DELETE /permissions/:id
Cancel permission request.

**Response:**
```json
{
  "message": "Permission request cancelled"
}
```

---

### Export

#### POST /export
Export content.

**Request:**
```json
{
  "content_ids": ["content_1", "content_2", "content_3"],
  "format": "web_optimized",
  "include_metadata": true,
  "metadata_format": "json",
  "delivery_method": "download"
}
```

**Response:**
```json
{
  "export_id": "export_123",
  "status": "processing",
  "estimated_completion": "2024-01-20T10:05:00Z"
}
```

#### GET /export/:id
Get export status.

**Response:**
```json
{
  "export_id": "export_123",
  "status": "completed",
  "progress": 100,
  "download_url": "https://cdn.example.com/exports/export_123.zip",
  "expires_at": "2024-01-27T10:00:00Z"
}
```

---

### Usage Tracking

#### POST /usage
Log content usage.

**Request:**
```json
{
  "content_id": "content_123",
  "type": "email_campaign",
  "campaign_name": "Summer Sale",
  "notes": "Used in promotional email"
}
```

**Response:**
```json
{
  "id": "usage_123",
  "message": "Usage logged successfully"
}
```

#### GET /content/:id/usage
Get usage history for content.

**Response:**
```json
{
  "content_id": "content_123",
  "usage_history": [
    {
      "id": "usage_1",
      "type": "email_campaign",
      "campaign_name": "Summer Sale",
      "used_at": "2024-01-20T10:00:00Z",
      "notes": "Used in promotional email"
    }
  ],
  "total_usage_count": 3
}
```

---

### Dashboard Stats

#### GET /dashboard/stats
Get dashboard statistics.

**Response:**
```json
{
  "total_content": 1234,
  "total_collections": 45,
  "approval_rate": 0.89,
  "pending_requests": 12,
  "recent_activity": [
    {
      "type": "content_discovered",
      "message": "New content discovered: #summer2024 (5 items)",
      "timestamp": "2024-01-20T09:00:00Z"
    },
    {
      "type": "permission_approved",
      "message": "Permission approved: @creator123",
      "timestamp": "2024-01-20T08:30:00Z"
    }
  ],
  "top_collections": [
    {
      "id": "collection_1",
      "name": "Summer Campaign 2024",
      "item_count": 234
    }
  ]
}
```

---

### Tags

#### GET /tags
Get all tags.

**Response:**
```json
{
  "tags": [
    {
      "name": "summer",
      "count": 234
    },
    {
      "name": "product",
      "count": 156
    }
  ]
}
```

#### POST /content/:id/tags
Add tags to content.

**Request:**
```json
{
  "tags": ["summer", "product"]
}
```

**Response:**
```json
{
  "message": "Tags added successfully"
}
```

---

## Error Responses

All errors follow this format:

```json
{
  "error": {
    "code": "ERROR_CODE",
    "message": "Human-readable error message",
    "details": {}
  }
}
```

### Common Error Codes:
- `400` - Bad Request (invalid parameters)
- `401` - Unauthorized (missing/invalid token)
- `403` - Forbidden (insufficient permissions)
- `404` - Not Found
- `429` - Rate Limit Exceeded
- `500` - Internal Server Error
- `503` - Service Unavailable

### Rate Limiting:
- 100 requests per minute per user
- 1000 requests per hour per user
- Headers included in response:
  - `X-RateLimit-Limit`: 100
  - `X-RateLimit-Remaining`: 95
  - `X-RateLimit-Reset`: 1640000000

---

## Webhooks (Future)

Webhooks for permission status updates (Phase 2):

#### POST /webhooks
Register webhook endpoint.

**Request:**
```json
{
  "url": "https://your-app.com/webhook",
  "events": ["permission.approved", "permission.denied"]
}
```

