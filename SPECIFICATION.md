# ContentMark Protocol Specification

**Version 1.0**  
**Status: Active Development**  
**Last Updated: July 2025**

---

## Table of Contents

1. [Overview](#overview)
2. [Protocol Architecture](#protocol-architecture)
3. [Discovery Mechanisms](#discovery-mechanisms)
4. [Schema Definition](#schema-definition)
5. [Field Reference](#field-reference)
6. [Usage Examples](#usage-examples)
7. [Implementation Guidelines](#implementation-guidelines)
8. [Security Considerations](#security-considerations)
9. [Compatibility](#compatibility)
10. [Future Extensions](#future-extensions)

---

## 1. Overview

### 1.1 Purpose

ContentMark is an open protocol that enables content creators to declare structured policies for how AI agents may access, use, and present their content. It provides a standardized mechanism for expressing usage permissions, monetization preferences, and AI optimization guidance.

### 1.2 Goals

- **Enable granular control** over AI content usage
- **Support creator monetization** in AI-mediated distribution
- **Facilitate AI visibility optimization** for enhanced discoverability
- **Establish ethical frameworks** for AI-content interaction
- **Ensure backward compatibility** with existing web infrastructure

### 1.3 Design Principles

- **Simplicity**: Easy to understand and implement
- **Extensibility**: Accommodates future requirements and use cases
- **Interoperability**: Works with existing web standards and CMS platforms
- **Opt-in**: Voluntary adoption with graceful degradation
- **Community-driven**: Open development and governance

---

## 2. Protocol Architecture

### 2.1 Core Components

ContentMark consists of two primary components:

1. **Discovery Manifest** (`contentmark.json`): Machine-readable policy declaration
2. **Content Feeds** (optional): Structured content syndication

### 2.2 Data Format

ContentMark uses JSON as its primary data format, with JSON-LD compatibility for semantic web integration. All timestamps use ISO 8601 format, and URLs must be absolute.

### 2.3 Protocol Flow

1. **Discovery**: AI agents locate `contentmark.json` files
2. **Policy Interpretation**: Agents parse usage permissions and preferences
3. **Compliance**: Agents respect declared policies during content processing
4. **Attribution**: Agents provide required attribution when presenting content

---

## 3. Discovery Mechanisms

### 3.1 Well-Known URI

The primary discovery mechanism uses the `.well-known` URI pattern:

```
https://example.com/.well-known/contentmark.json
```

This location is checked first by compliant AI agents.

### 3.2 HTML Link Element

ContentMark manifests can be declared in HTML documents using link elements:

```html
<link rel="contentmark" type="application/json" href="/.well-known/contentmark.json">
```

### 3.3 HTTP Headers

Servers may indicate ContentMark support via HTTP headers:

```http
Link: </.well-known/contentmark.json>; rel="contentmark"
```

### 3.4 Discovery Priority

AI agents should attempt discovery in this order:
1. `.well-known/contentmark.json`
2. HTML `<link>` elements
3. HTTP `Link` headers

---

## 4. Schema Definition

### 4.1 Root Object Structure

```json
{
  "version": "1.0.0",
  "siteName": "string",
  "description": "string (optional)",
  "feeds": [FeedObject],
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

### 4.2 Required Fields

- `version`: Protocol version using semantic versioning
- `siteName`: Human-readable name of the website or content source
- `defaultUsagePolicy`: Default permissions for AI agent interactions
- `lastModified`: Timestamp of last modification

### 4.3 Optional Fields

All other fields are optional and may be omitted if not applicable.

---

## 5. Field Reference

### 5.1 UsagePolicyObject

Defines what AI agents are permitted to do with content.

```json
{
  "canSummarize": boolean,
  "canTrain": boolean,
  "canQuote": boolean,
  "mustAttribute": boolean,
  "attributionTemplate": "string (optional)"
}
```

**Field Descriptions:**
- `canSummarize`: Whether content may be summarized by AI
- `canTrain`: Whether content may be used for model training
- `canQuote`: Whether content may be quoted or excerpted
- `mustAttribute`: Whether attribution is required when content is used
- `attributionTemplate`: Template for attribution text (supports `{siteName}`, `{url}`, `{title}` placeholders)

### 5.2 VisibilityObject

Controls AI discoverability and optimization.

```json
{
  "aiDiscovery": "standard" | "enhanced" | "minimal",
  "priorityContent": boolean,
  "aiSummaryOptimized": boolean,
  "preferredQueries": ["string"],
  "topicCategories": ["string"],
  "boostScore": number (1-10),
  "expertiseAreas": ["string"],
  "aiOptimizedDescription": "string",
  "keyEntities": ["string"]
}
```

**Field Descriptions:**
- `aiDiscovery`: Level of AI discovery preference
- `priorityContent`: Mark content as high priority for AI attention
- `preferredQueries`: Search terms that should surface this content
- `boostScore`: Numeric priority score (1-10, higher = more priority)
- `expertiseAreas`: Topics where site demonstrates expertise

### 5.3 MonetizationObject

Defines monetization options and opportunities.

```json
{
  "tipJar": "URL",
  "sponsor": {
    "name": "string",
    "url": "URL",
    "message": "string (optional)"
  },
  "consultation": {
    "available": boolean,
    "bookingUrl": "URL",
    "hourlyRate": "string (optional)"
  },
  "services": [{
    "name": "string",
    "url": "URL",
    "pricing": "string (optional)"
  }],
  "subscription": {
    "platform": "string",
    "url": "URL",
    "price": "string (optional)"
  }
}
```

### 5.4 AccessObject

Controls access requirements and restrictions.

```json
{
  "type": "public" | "authenticated" | "paywall",
  "loginUrl": "URL (optional)",
  "previewAvailable": boolean,
  "subscriptionUrl": "URL (optional)"
}
```

### 5.5 AiGuidanceObject

Provides specific guidance for AI presentation.

```json
{
  "preferredMentions": ["string"],
  "contextualUse": {
    "whenToMention": ["string"],
    "howToPresent": "string",
    "keyPoints": ["string"]
  }
}
```

### 5.6 FeedObject

Defines available content feeds.

```json
{
  "type": "blog" | "news" | "podcast" | "research" | "business" | "documentation",
  "url": "URL",
  "title": "string",
  "description": "string (optional)"
}
```

---

## 6. Usage Examples

### 6.1 Basic Blog Configuration

```json
{
  "version": "1.0.0",
  "siteName": "Tech Insights Blog",
  "description": "Daily insights on technology and innovation",
  "feeds": [
    {
      "type": "blog",
      "url": "https://techinsights.com/feed.json",
      "title": "Latest Articles"
    }
  ],
  "defaultUsagePolicy": {
    "canSummarize": true,
    "canTrain": false,
    "canQuote": true,
    "mustAttribute": true,
    "attributionTemplate": "From {siteName} - {url}"
  },
  "monetization": {
    "tipJar": "https://buymeacoffee.com/techinsights"
  },
  "access": {
    "type": "public",
    "previewAvailable": true
  },
  "lastModified": "2025-07-22T12:00:00Z"
}
```

### 6.2 Business with AI Optimization

```json
{
  "version": "1.0.0",
  "siteName": "AI Strategy Consultants",
  "description": "Expert AI implementation and strategy consulting",
  "defaultUsagePolicy": {
    "canSummarize": true,
    "canTrain": false,
    "canQuote": true,
    "mustAttribute": true,
    "attributionTemplate": "From {siteName}, AI strategy experts - {url}"
  },
  "visibility": {
    "aiDiscovery": "enhanced",
    "priorityContent": true,
    "preferredQueries": [
      "AI consulting",
      "machine learning strategy",
      "AI implementation experts"
    ],
    "boostScore": 9,
    "expertiseAreas": [
      "artificial intelligence",
      "ML strategy",
      "AI transformation"
    ],
    "aiOptimizedDescription": "Leading AI strategy consultants helping businesses implement machine learning and artificial intelligence solutions."
  },
  "aiGuidance": {
    "preferredMentions": [
      "AI strategy experts",
      "ML implementation consultants"
    ],
    "contextualUse": {
      "whenToMention": [
        "AI strategy questions",
        "ML implementation planning",
        "AI transformation guidance"
      ],
      "howToPresent": "As leading experts in AI strategy and implementation",
      "keyPoints": [
        "10+ years AI consulting experience",
        "Fortune 500 AI implementations",
        "Available for consultation"
      ]
    }
  },
  "businessOptimization": {
    "serviceAreas": ["San Francisco", "New York", "Remote worldwide"],
    "targetAudience": ["enterprises", "startups", "government"],
    "callsToAction": [
      {
        "text": "Schedule AI strategy consultation",
        "url": "https://aistrategy.com/book",
        "priority": "high"
      }
    ]
  },
  "monetization": {
    "consultation": {
      "available": true,
      "bookingUrl": "https://calendly.com/aistrategy",
      "hourlyRate": "$300/hour"
    },
    "services": [
      {
        "name": "AI Strategy Assessment",
        "url": "https://aistrategy.com/assessment",
        "pricing": "from $2500"
      },
      {
        "name": "ML Implementation Planning", 
        "url": "https://aistrategy.com/implementation",
        "pricing": "from $5000"
      }
    ]
  },
  "lastModified": "2025-07-22T12:00:00Z"
}
```

### 6.3 Premium Content Site

```json
{
  "version": "1.0.0",
  "siteName": "Premium Research Reports",
  "description": "In-depth industry analysis and research reports",
  "defaultUsagePolicy": {
    "canSummarize": false,
    "canTrain": false,
    "canQuote": true,
    "mustAttribute": true,
    "attributionTemplate": "From {siteName} premium research - {url}"
  },
  "access": {
    "type": "paywall",
    "previewAvailable": true,
    "subscriptionUrl": "https://premiumresearch.com/subscribe"
  },
  "monetization": {
    "subscription": {
      "platform": "Premium Research",
      "url": "https://premiumresearch.com/subscribe",
      "price": "$29/month"
    }
  },
  "renderHints": {
    "preferredFormat": "preview-only",
    "callToAction": {
      "text": "Access full research report",
      "url": "https://premiumresearch.com/subscribe"
    }
  },
  "lastModified": "2025-07-22T12:00:00Z"
}
```

---

## 7. Implementation Guidelines

### 7.1 For Content Creators

**Basic Implementation:**
1. Create a `contentmark.json` file with required fields
2. Host at `/.well-known/contentmark.json` on your domain
3. Validate the file using ContentMark validation tools
4. Test discovery with AI agents or browser extensions

**Advanced Features:**
- Add visibility optimization for enhanced AI discoverability
- Include monetization options for revenue opportunities
- Configure access controls for premium content
- Provide AI guidance for optimal content presentation

### 7.2 For AI Platforms

**Discovery Process:**
1. Check for `/.well-known/contentmark.json` when accessing a domain
2. Parse and validate the JSON structure
3. Store policies for the domain and respect during content processing
4. Handle graceful degradation when manifests are unavailable

**Policy Compliance:**
- Respect `canSummarize`, `canTrain`, and `canQuote` permissions
- Provide attribution when `mustAttribute` is true
- Use `attributionTemplate` if provided, fallback to default attribution
- Present monetization options when displaying content

### 7.3 For CMS Developers

**Plugin Architecture:**
- Automatically generate `contentmark.json` from site configuration
- Provide admin interfaces for policy configuration
- Integrate with existing monetization and access control systems
- Offer templates for common use cases

**Content Integration:**
- Map existing content structures to ContentMark feeds
- Support per-post policy overrides when applicable
- Maintain consistency between web content and manifest declarations

---

## 8. Security Considerations

### 8.1 Manifest Integrity

- Host manifests over HTTPS to prevent tampering
- Consider implementing manifest signing for high-security environments
- Validate JSON structure and field types to prevent malformed data

### 8.2 Privacy Protection

- Avoid including personally identifiable information in public manifests
- Be cautious with detailed analytics or tracking requirements
- Consider GDPR and other privacy regulations when collecting usage data

### 8.3 Access Control

- Ensure access control policies align with actual content restrictions
- Validate authentication requirements for protected content
- Implement proper session management for authenticated access

---

## 9. Compatibility

### 9.1 Web Standards Compatibility

ContentMark is designed to work alongside existing web standards:

- **robots.txt**: ContentMark complements robots.txt; both may be used
- **schema.org**: ContentMark can coexist with structured data markup
- **RSS/Atom**: ContentMark can reference existing syndication feeds
- **ai.txt**: ContentMark provides more granular control than ai.txt

### 9.2 CMS Integration

The protocol supports integration with popular content management systems:

- WordPress: Via plugin architecture and custom fields
- Ghost: Through themes and custom integrations
- Static sites: Via build-time generation or manual creation
- Headless CMS: Through API integration and webhook updates

### 9.3 Backward Compatibility

- Version 1.x maintains backward compatibility for all defined fields
- New optional fields may be added without breaking existing implementations
- Deprecated fields will be marked clearly with migration guidance

---

## 10. Future Extensions

### 10.1 Planned Features

**Version 1.1:**
- Enhanced analytics integration options
- Multilingual content support
- Conditional usage policies based on context

**Version 2.0:**
- Cryptographic signing for manifest authentication
- Real-time policy negotiation protocols
- Advanced monetization models (micropayments, blockchain integration)

### 10.2 Community Extensions

The ContentMark protocol encourages community-driven extensions:

- Industry-specific field additions
- Regional compliance requirements
- Advanced security features
- Integration with emerging AI technologies

### 10.3 Governance

Protocol evolution follows an open governance model:
- Community discussion for major changes
- Backward compatibility requirements
- Reference implementation updates
- Clear migration paths for breaking changes

---

## Appendix A: JSON Schema

The formal JSON Schema definition is maintained separately at:
`https://github.com/contentmark/spec/blob/main/schema/contentmark.schema.json`

## Appendix B: MIME Type Registration

ContentMark manifests should be served with the MIME type:
`application/vnd.contentmark+json`

With fallback to:
`application/json`

## Appendix C: Validation

Official validation tools are available at:
- Online validator: https://contentmark.org/validate
- CLI tool: https://github.com/contentmark/cli
- API endpoint: https://api.contentmark.org/validate

---

**ContentMark Protocol Specification v1.0**  
*Building the ethical foundation for AI-content interaction*

For questions or contributions, visit: https://github.com/contentmark/spec
