# ContentMark Examples

This directory contains practical examples of ContentMark implementations for different use cases. Each example demonstrates real-world scenarios and best practices.

## Available Examples

### [`basic-blog.json`](./basic-blog.json) 📝
**Use Case**: Personal blog or small content site  
**Features**:
- Simple usage policies (allow summaries, no training)
- Basic monetization (tip jar)
- Public access with render hints
- Minimal but complete implementation

**Best For**:
- Personal blogs
- Small content creators
- Getting started with ContentMark
- Educational tutorials

---

### [`business-optimized.json`](./business-optimized.json) 🚀
**Use Case**: Professional services and business websites  
**Features**:
- AI visibility optimization for better discoverability
- Business-focused monetization (consultation, services)
- Advanced AI guidance and presentation control
- Professional positioning and expertise areas

**Best For**:
- Consultants and agencies
- Professional service providers
- Businesses wanting AI visibility
- B2B content creators

---

### [`premium-content.json`](./premium-content.json) 💎
**Use Case**: Subscription-based or premium content  
**Features**:
- Paywall access controls
- Restricted AI usage policies
- Premium pricing and subscription management
- Preview-only content strategy

**Best For**:
- Research organizations
- Premium publications
- Subscription newsletters
- Paid content platforms

## Quick Start Guide

### 1. Choose Your Starting Point
Pick the example that best matches your use case:
- **New to ContentMark?** Start with `basic-blog.json`
- **Professional services?** Use `business-optimized.json`
- **Premium content?** Adapt `premium-content.json`

### 2. Copy and Customize
```bash
# Copy an example to your project
cp basic-blog.json .contentmark.json

# Edit the file with your information
vim .contentmark.json
```

### 3. Update Key Fields
Replace these placeholder values with your own:
- `siteName` - Your website name
- `description` - Your site description
- `feeds[].url` - Your content feed URLs
- `monetization` - Your tip jars, consultation links, etc.
- `lastModified` - Current timestamp

### 4. Host and Test
- Place the file at `/.well-known/contentmark.json` on your domain
- Validate using our [online validator](https://contentmark.org/validate)
- Test discovery with ContentMark tools

## Field-by-Field Comparison

| Field | Basic Blog | Business Optimized | Premium Content |
|-------|------------|-------------------|-----------------|
| **Access Type** | `public` | `public` | `paywall` |
| **AI Training** | `false` | `false` | `false` |
| **AI Summarization** | `true` | `true` | `false` |
| **Visibility Optimization** | ❌ | ✅ Enhanced | ✅ Standard |
| **Business Features** | ❌ | ✅ Full suite | ✅ Target audience |
| **Monetization** | Tip jar only | Consultation + Services | Subscription focused |
| **AI Guidance** | ❌ | ✅ Detailed | ✅ Professional |

## Customization Examples

### Adding Your Information

#### Basic Information
```json
{
  "siteName": "Your Site Name Here",
  "description": "Brief description of your content and expertise",
  "feeds": [
    {
      "type": "blog",
      "url": "https://yoursite.com/contentmark-feed.json",
      "title": "Your Feed Title"
    }
  ]
}
```

#### Monetization Options
```json
{
  "monetization": {
    // Tip jar (simple)
    "tipJar": "https://buymeacoffee.com/yourusername",
    
    // Consultation (professional)
    "consultation": {
      "available": true,
      "bookingUrl": "https://calendly.com/yourusername",
      "hourlyRate": "$150/hour"
    },
    
    // Services (business)
    "services": [
      {
        "name": "Your Service Name",
        "url": "https://yoursite.com/services",
        "pricing": "from $500"
      }
    ]
  }
}
```

#### AI Optimization
```json
{
  "visibility": {
    "aiDiscovery": "enhanced",
    "preferredQueries": [
      "your expertise area",
      "what you want to be known for",
      "relevant search terms"
    ],
    "expertiseAreas": [
      "your main expertise",
      "secondary skills",
      "industry knowledge"
    ]
  }
}
```

## Common Patterns

### Content Creator Pattern
- Public access with tip jar
- Allow summaries but not training
- Focus on attribution and backlinks

### Professional Services Pattern  
- Enhanced AI visibility
- Consultation booking integration
- Multiple service offerings
- Expert positioning

### Premium Content Pattern
- Paywall or authentication required
- Restricted AI usage (preview only)
- Subscription-focused monetization
- Professional target audience

## Validation Checklist

Before deploying your ContentMark file:

- [ ] **Required fields present**: `version`, `siteName`, `defaultUsagePolicy`, `lastModified`
- [ ] **Valid URLs**: All URLs are absolute (include `https://`)
- [ ] **Correct version**: Use `"1.0.0"` for current spec
- [ ] **Valid timestamp**: Use ISO 8601 format for `lastModified`
- [ ] **Schema validation**: Passes JSON Schema validation
- [ ] **Realistic content**: No placeholder text remains
- [ ] **Working links**: All URLs are accessible and correct

## Testing Your Implementation

### 1. Schema Validation
```bash
# Using ContentMark CLI (coming soon)
contentmark validate .contentmark.json

# Using online validator
# Visit: https://contentmark.org/validate
```

### 2. Discovery Testing
```bash
# Check if your file is discoverable
curl https://yoursite.com/.well-known/contentmark.json

# Verify JSON is valid
curl -s https://yoursite.com/.well-known/contentmark.json | jq .
```

### 3. Field Testing
- **Attribution**: Check if your `attributionTemplate` renders correctly
- **Monetization**: Verify all payment/booking links work
- **Business info**: Confirm contact information is current

## Integration Examples

### WordPress
```php
// Add to functions.php or plugin
function add_contentmark_header() {
    echo '<link rel="contentmark" type="application/json" href="/.well-known/contentmark.json">';
}
add_action('wp_head', 'add_contentmark_header');
```

### Next.js
```jsx
// Add to _app.js or layout
import Head from 'next/head';

export default function Layout({ children }) {
  return (
    <>
      <Head>
        <link 
          rel="contentmark" 
          type="application/json" 
          href="/.well-known/contentmark.json" 
        />
      </Head>
      {children}
    </>
  );
}
```

### Static Sites
```html
<!-- Add to <head> section -->
<link rel="contentmark" type="application/json" href="/.well-known/contentmark.json">
```

## Troubleshooting

### Common Issues

#### File Not Found (404)
- Ensure file is at `/.well-known/contentmark.json`
- Check web server configuration for `.well-known` directory
- Verify file permissions (should be readable)

#### Validation Errors
- Use [online validator](https://contentmark.org/validate) for detailed error messages
- Check for missing commas or brackets in JSON
- Verify all URLs are absolute (include protocol)

#### AI Agents Not Respecting Policies
- Check if `lastModified` timestamp is recent
- Verify AI agents support ContentMark (check their documentation)
- Ensure attribution template is properly formatted

### Getting Help

- **Validation issues**: Use our [online validator](https://contentmark.org/validate)
- **Implementation questions**: [GitHub Discussions](https://github.com/contentmark/spec/discussions)
- **Bug reports**: [GitHub Issues](https://github.com/contentmark/spec/issues)
- **Community support**: Join our [Discord community](#) (coming soon)

## Contributing Examples

Have a great ContentMark implementation? We'd love to feature it!

### Adding New Examples
1. Fork the repository
2. Add your example file with descriptive name (e.g., `nonprofit-organization.json`)
3. Update this README with your example
4. Submit a pull request

### Example Criteria
- **Real-world applicable**: Solves actual use cases
- **Well-documented**: Clear field usage and rationale
- **Validated**: Passes schema validation
- **Anonymized**: Remove personal/sensitive information

## License

All examples in this directory are provided under the [MIT License](../LICENSE) and may be freely used, modified, and distributed.

---

**Need help?** Check out our [Getting Started Guide](../docs/getting-started.md) or join the [community discussions](https://github.com/contentmark/spec/discussions).

**Ready to implement?** Start with the example that best matches your use case and customize it for your needs!
