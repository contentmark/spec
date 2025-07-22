# ContentMark Field Reference

Complete reference for all fields in the ContentMark protocol specification.

## Table of Contents

- [Root Object](#root-object)
- [Required Fields](#required-fields)
- [Optional Fields](#optional-fields)
- [Complex Objects](#complex-objects)
- [Field Constraints](#field-constraints)
- [Usage Guidelines](#usage-guidelines)

---

## Root Object

The main ContentMark manifest structure:

```json
{
  "version": "string",
  "siteName": "string",
  "description": "string (optional)",
  "feeds": [FeedObject] (optional),
  "defaultUsagePolicy": UsagePolicyObject,
  "visibility": VisibilityObject (optional),
  "aiGuidance": AiGuidanceObject (optional),
  "businessOptimization": BusinessOptimizationObject (optional),
  "monetization": MonetizationObject (optional),
  "access": AccessObject (optional),
  "renderHints": RenderHintsObject (optional),
  "lastModified": "ISO 8601 timestamp"
}
```

---

## Required Fields

### `version`
**Type**: `string`  
**Pattern**: Semantic versioning (`^\d+\.\d+\.\d+$`)  
**Required**: ✅ Yes

Protocol version using semantic versioning format.

```json
{
  "version": "1.0.0"
}
```

**Valid Examples**:
- `"1.0.0"` ✅
- `"1.2.3"` ✅
- `"2.0.0"` ✅

**Invalid Examples**:
- `"v1.0.0"` ❌ (includes 'v' prefix)
- `"1.0"` ❌ (missing patch version)
- `1.0.0` ❌ (number instead of string)

---

### `siteName`
**Type**: `string`  
**Length**: 1-200 characters  
**Required**: ✅ Yes

Human-readable name of the website or content source.

```json
{
  "siteName": "Tech Insights Blog"
}
```

**Guidelines**:
- Use your actual site/brand name
- Keep concise but descriptive
- Avoid special characters that might break attribution
- Consider how it will appear in AI-generated text

**Examples**:
- `"Sarah's Design Blog"` ✅
- `"AI Strategy Consultants"` ✅
- `"Premium Tech Research"` ✅

---

### `defaultUsagePolicy`
**Type**: `object`  
**Required**: ✅ Yes

Defines default permissions for AI agent interactions with your content.

```json
{
  "defaultUsagePolicy": {
    "canSummarize": true,
    "canTrain": false,
    "canQuote": true,
    "mustAttribute": true,
    "attributionTemplate": "From {siteName} - {url}"
  }
}
```

See [UsagePolicyObject](#usagepolicyobject) for detailed field descriptions.

---

### `lastModified`
**Type**: `string`  
**Format**: ISO 8601 datetime  
**Required**: ✅ Yes

Timestamp of when the ContentMark file was last modified.

```json
{
  "lastModified": "2025-07-22T15:30:00Z"
}
```

**Valid Formats**:
- `"2025-07-22T15:30:00Z"` ✅ (UTC)
- `"2025-07-22T15:30:00-07:00"` ✅ (with timezone)
- `"2025-07-22T15:30:00.123Z"` ✅ (with milliseconds)

**Invalid Formats**:
- `"2025-07-22 15:30:00"` ❌ (missing 'T' separator)
- `"07/22/2025"` ❌ (not ISO 8601)
- `1721656200` ❌ (Unix timestamp)

---

## Optional Fields

### `description`
**Type**: `string`  
**Length**: 0-500 characters  
**Required**: ❌ Optional

Brief description of the website or content focus.

```json
{
  "description": "Daily insights on technology trends, programming tutorials, and startup advice"
}
```

**Best Practices**:
- Describe your content focus and expertise
- Help AI understand your site's purpose
- Use keywords relevant to your content
- Keep under 500 characters for optimal display

---

### `feeds`
**Type**: `array` of `FeedObject`  
**Required**: ❌ Optional

Array of available content feeds for structured content access.

```json
{
  "feeds": [
    {
      "type": "blog",
      "url": "https://example.com/feed.json",
      "title": "Latest Articles",
      "description": "Recent blog posts and tutorials"
    }
  ]
}
```

See [FeedObject](#feedobject) for detailed structure.

---

## Complex Objects

### UsagePolicyObject

Defines permissions for AI content usage.

```json
{
  "canSummarize": boolean,
  "canTrain": boolean,
  "canQuote": boolean,
  "mustAttribute": boolean,
  "attributionTemplate": "string (optional)"
}
```

#### `canSummarize`
**Type**: `boolean`  
**Required**: ✅ Yes (within UsagePolicy)

Whether AI agents may create summaries of your content.

```json
{
  "canSummarize": true
}
```

**When to allow (`true`)**:
- You want increased content discoverability
- You're comfortable with AI-generated summaries
- You want to help users quickly understand your content

**When to block (`false`)**:
- Your content is premium/proprietary
- You prefer direct engagement over summaries
- Content contains sensitive information

#### `canTrain`
**Type**: `boolean`  
**Required**: ✅ Yes (within UsagePolicy)

Whether AI systems may use your content for model training.

```json
{
  "canTrain": false
}
```

**When to allow (`true`)**:
- You support AI development and research
- You're comfortable contributing to training datasets
- You want to potentially influence AI model behavior

**When to block (`false`)**:
- You want to maintain content exclusivity
- You're concerned about AI competition
- Legal or proprietary restrictions apply

#### `canQuote`
**Type**: `boolean`  
**Required**: ✅ Yes (within UsagePolicy)

Whether AI agents may quote excerpts from your content.

```json
{
  "canQuote": true
}
```

**When to allow (`true`)**:
- You want attribution and backlinks
- You're comfortable with partial content sharing
- You want to be cited as a source

**When to block (`false`)**:
- Your content is highly sensitive
- You prefer all-or-nothing access
- Legal restrictions prevent quotation

#### `mustAttribute`
**Type**: `boolean`  
**Required**: ✅ Yes (within UsagePolicy)

Whether AI agents must provide attribution when using your content.

```json
{
  "mustAttribute": true
}
```

**When to require (`true`)**:
- You want credit and recognition
- You need backlinks for SEO
- You want to build brand awareness

**When to skip (`false`)**:
- You prefer anonymous contribution
- Attribution might be problematic
- You prioritize pure content sharing

#### `attributionTemplate`
**Type**: `string`  
**Length**: 0-200 characters  
**Required**: ❌ Optional

Template string for how attribution should be formatted.

```json
{
  "attributionTemplate": "From {siteName} - {url}"
}
```

**Available Placeholders**:
- `{siteName}` - Your site name
- `{url}` - URL to the content
- `{title}` - Content title (if available)

**Examples**:
- `"From {siteName} - {url}"` ✅
- `"Content by {siteName}"` ✅
- `"Source: {title} on {siteName} ({url})"` ✅

---

### VisibilityObject

Controls AI discoverability and optimization settings.

```json
{
  "aiDiscovery": "enhanced",
  "priorityContent": true,
  "aiSummaryOptimized": true,
  "preferredQueries": ["AI consulting", "machine learning"],
  "topicCategories": ["technology", "consulting"],
  "boostScore": 8,
  "expertiseAreas": ["artificial intelligence", "strategy"],
  "aiOptimizedDescription": "Leading AI consultants...",
  "keyEntities": ["AI consulting", "ML strategy"]
}
```

#### `aiDiscovery`
**Type**: `string`  
**Enum**: `"standard"`, `"enhanced"`, `"minimal"`  
**Required**: ❌ Optional

Level of AI discovery preference.

- `"minimal"` - Reduce AI visibility (prefer not to be discovered)
- `"standard"` - Normal discovery behavior (default)
- `"enhanced"` - Boost AI discovery and recommendations

#### `priorityContent`
**Type**: `boolean`  
**Required**: ❌ Optional

Mark content as high priority for AI attention.

```json
{
  "priorityContent": true
}
```

Use when your content is particularly valuable or authoritative in your domain.

#### `preferredQueries`
**Type**: `array` of `string`  
**Max Items**: 20  
**Item Length**: 1-100 characters  
**Required**: ❌ Optional

Search terms that should surface your content in AI responses.

```json
{
  "preferredQueries": [
    "AI consulting services",
    "machine learning strategy",
    "enterprise AI implementation"
  ]
}
```

**Best Practices**:
- Use natural language phrases users would search for
- Include both specific and general terms
- Think about questions people ask in your domain
- Avoid keyword stuffing

#### `boostScore`
**Type**: `integer`  
**Range**: 1-10  
**Required**: ❌ Optional

Numeric priority score for content visibility.

```json
{
  "boostScore": 8
}
```

**Score Guidelines**:
- `1-3`: Low priority, occasional mentions
- `4-6`: Standard priority, normal discovery
- `7-8`: High priority, frequent recommendations
- `9-10`: Critical priority, primary source status

#### `expertiseAreas`
**Type**: `array` of `string`  
**Max Items**: 15  
**Item Length**: 1-100 characters  
**Required**: ❌ Optional

Topics where your site demonstrates particular expertise.

```json
{
  "expertiseAreas": [
    "React development",
    "Frontend architecture",
    "Web performance optimization"
  ]
}
```

Helps AI understand when to recommend you as an authoritative source.

---

### MonetizationObject

Defines monetization options and revenue opportunities.

```json
{
  "tipJar": "https://buymeacoffee.com/username",
  "sponsor": {
    "name": "Sponsor Name",
    "url": "https://sponsor.com",
    "message": "Sponsored message"
  },
  "consultation": {
    "available": true,
    "bookingUrl": "https://calendly.com/username",
    "hourlyRate": "$200/hour"
  },
  "services": [{
    "name": "Service Name",
    "url": "https://example.com/service",
    "pricing": "from $500"
  }],
  "subscription": {
    "platform": "Platform Name",
    "url": "https://subscribe.com",
    "price": "$9.99/month"
  }
}
```

#### `tipJar`
**Type**: `string` (URL)  
**Required**: ❌ Optional

URL for tip/donation platform integration.

```json
{
  "tipJar": "https://buymeacoffee.com/techblogger"
}
```

**Supported Platforms**:
- Buy Me a Coffee
- Ko-fi
- PayPal
- Patreon
- Custom donation pages

#### `consultation`
**Type**: `object`  
**Required**: ❌ Optional

Consultation service availability and booking information.

```json
{
  "consultation": {
    "available": true,
    "bookingUrl": "https://calendly.com/consultant",
    "hourlyRate": "$150/hour"
  }
}
```

**Fields**:
- `available` (boolean, required): Whether consultations are offered
- `bookingUrl` (URL, optional): Direct booking link
- `hourlyRate` (string, optional): Rate information

---

### AccessObject

Controls content access requirements and restrictions.

```json
{
  "type": "public",
  "loginUrl": "https://example.com/login",
  "previewAvailable": true,
  "subscriptionUrl": "https://example.com/subscribe"
}
```

#### `type`
**Type**: `string`  
**Enum**: `"public"`, `"authenticated"`, `"paywall"`  
**Required**: ✅ Yes (within AccessObject)

Type of access required for content.

- `"public"` - Freely accessible content
- `"authenticated"` - Requires user login/registration
- `"paywall"` - Requires payment or subscription

#### `loginUrl`
**Type**: `string` (URL)  
**Required**: ✅ Yes (when type is "authenticated")

URL for user authentication or login.

```json
{
  "type": "authenticated",
  "loginUrl": "https://mysite.com/login"
}
```

#### `previewAvailable`
**Type**: `boolean`  
**Required**: ❌ Optional

Whether content previews are available without full access.

```json
{
  "previewAvailable": true
}
```

Useful for paywall content where you want to show summaries or teasers.

---

### FeedObject

Defines available content feeds for structured access.

```json
{
  "type": "blog",
  "url": "https://example.com/feed.json",
  "title": "Latest Articles",
  "description": "Recent blog posts and tutorials"
}
```

#### `type`
**Type**: `string`  
**Enum**: `"blog"`, `"news"`, `"podcast"`, `"research"`, `"business"`, `"documentation"`  
**Required**: ✅ Yes (within FeedObject)

Type of content feed.

#### `url`
**Type**: `string` (URL)  
**Required**: ✅ Yes (within FeedObject)

Absolute URL to the structured content feed.

#### `title`
**Type**: `string`  
**Length**: 1-100 characters  
**Required**: ✅ Yes (within FeedObject)

Human-readable title for the content feed.

---

## Field Constraints

### String Length Limits

| Field | Min Length | Max Length |
|-------|------------|------------|
| `siteName` | 1 | 200 |
| `description` | 0 | 500 |
| `attributionTemplate` | 0 | 200 |
| `aiOptimizedDescription` | 0 | 500 |
| Feed `title` | 1 | 100 |
| Feed `description` | 0 | 300 |
| Service `name` | 1 | 100 |
| Service `pricing` | 0 | 100 |

### Array Item Limits

| Field | Max Items |
|-------|-----------|
| `preferredQueries` | 20 |
| `topicCategories` | 10 |
| `expertiseAreas` | 15 |
| `keyEntities` | 20 |
| `services` | 10 |
| `callsToAction` | 5 |

### Numeric Ranges

| Field | Min Value | Max Value |
|-------|-----------|-----------|
| `boostScore` | 1 | 10 |

---

## Usage Guidelines

### Required Field Priority

Always include these fields for valid ContentMark:

1. `version` - Use "1.0.0" for current spec
2. `siteName` - Your actual site/brand name
3. `defaultUsagePolicy` - All four boolean fields required
4. `lastModified` - Current ISO 8601 timestamp

### URL Format Requirements

All URL fields must be absolute URLs:
- ✅ `"https://example.com/page"`
- ❌ `"example.com/page"` (missing protocol)
- ❌ `"/relative/path"` (relative URL)

### Timestamp Best Practices

Update `lastModified` whenever you change your ContentMark file:
- Use UTC timezone (`Z` suffix) for consistency
- Update when changing policies, monetization, or contact info
- Consider automation for dynamic sites

### Boolean Field Logic

For `defaultUsagePolicy` booleans:
- Start conservatively (block training, allow quotes)
- `mustAttribute: true` is recommended for most use cases
- `canSummarize: true` helps with content discovery
- `canTrain: false` maintains content control

### Monetization Strategy

Layer monetization options by effort:
1. **Low effort**: Add `tipJar` for donations
2. **Medium effort**: Add `consultation` with booking
3. **High effort**: Add `services` with detailed offerings

---

## Validation

All fields are validated against the [JSON Schema](../schema/contentmark.schema.json). Use our [online validator](https://contentmark.org/validate) to check your implementation.

### Common Validation Errors

- **Missing required fields**: Ensure `version`, `siteName`, `defaultUsagePolicy`, `lastModified` are present
- **Invalid URLs**: All URLs must include protocol (`https://`)
- **String length violations**: Check character limits for text fields
- **Invalid enums**: Use exact values for `type`, `aiDiscovery`, etc.
- **Invalid timestamps**: Use proper ISO 8601 format

---

## Support

- **Field questions**: [GitHub Discussions](https://github.com/contentmark/spec/discussions)
- **Validation issues**: [Online Validator](https://contentmark.org/validate)
- **Schema reference**: [JSON Schema documentation](../schema/README.md)

---

*For the complete technical specification, see [SPECIFICATION.md](../SPECIFICATION.md)*
