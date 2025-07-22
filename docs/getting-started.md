# Getting Started with ContentMark

Welcome to ContentMark! This guide will help you implement the ContentMark protocol on your website in just a few minutes.

## What is ContentMark?

ContentMark is an open protocol that gives you control over how AI agents use your content. Instead of hoping AI respects your content or blocking it entirely, ContentMark lets you:

- **Define usage permissions** (summarize ✅, train ❌, quote ✅)
- **Monetize AI traffic** (tips, consultations, subscriptions)
- **Optimize AI visibility** (get discovered for relevant queries)
- **Control presentation** (how AI should display your content)

Think of it as "robots.txt for AI" but with monetization and optimization built in.

## Quick Start (5 Minutes)

### Step 1: Choose Your Template

Pick the example that best matches your site:

- **Personal blog or small site**: [`basic-blog.json`](../examples/basic-blog.json)
- **Professional services**: [`business-optimized.json`](../examples/business-optimized.json)  
- **Premium/paid content**: [`premium-content.json`](../examples/premium-content.json)

### Step 2: Download and Customize

```bash
# Download the basic template
curl -o .contentmark.json https://raw.githubusercontent.com/contentmark/spec/main/examples/basic-blog.json
```

Or copy-paste from the examples and save as `.contentmark.json`.

### Step 3: Update Your Information

Edit these key fields:

```json
{
  "siteName": "Your Website Name",
  "description": "What your site is about",
  "feeds": [
    {
      "url": "https://yoursite.com/contentmark-feed.json",
      "title": "Your Content Feed"
    }
  ],
  "monetization": {
    "tipJar": "https://buymeacoffee.com/yourusername"
  },
  "lastModified": "2025-07-22T12:00:00Z"
}
```

### Step 4: Deploy to Your Website

Place the file at: `https://yoursite.com/.well-known/contentmark.json`

#### Static Sites
Upload to your `/.well-known/` directory

#### WordPress
Use an FTP client or file manager in your hosting control panel

#### Next.js/React
Place in your `public/.well-known/` directory

#### Other CMS
Check our [platform-specific guides](#platform-specific-guides) below

### Step 5: Validate and Test

Visit https://contentmark.org/validate and enter your URL to check if everything works correctly.

🎉 **That's it!** Your site now has ContentMark support.

---

## Detailed Implementation Guide

### Understanding ContentMark Structure

Every ContentMark file has four required sections:

```json
{
  "version": "1.0.0",              // Protocol version
  "siteName": "Your Site",         // Human-readable name
  "defaultUsagePolicy": {          // AI permissions
    "canSummarize": true,
    "canTrain": false,
    "canQuote": true,
    "mustAttribute": true
  },
  "lastModified": "2025-07-22T12:00:00Z"  // When you last updated
}
```

### Usage Policies Explained

#### `canSummarize` 
**What it means**: AI can create summaries of your content  
**When to allow**: You want AI to help people discover your content  
**When to block**: Your content is premium or highly proprietary

#### `canTrain`
**What it means**: AI can use your content to train models  
**When to allow**: You want to contribute to AI development  
**When to block**: You want to maintain exclusivity or control

#### `canQuote`
**What it means**: AI can quote excerpts from your content  
**When to allow**: You want attribution and backlinks  
**When to block**: Your content is sensitive or confidential

#### `mustAttribute`
**What it means**: AI must credit you when using your content  
**When to require**: You want traffic and recognition  
**When to skip**: You prefer anonymous contribution

### Adding Monetization

ContentMark supports various ways to monetize AI traffic:

#### Tip Jars (Easiest)
```json
{
  "monetization": {
    "tipJar": "https://buymeacoffee.com/yourusername"
  }
}
```

#### Professional Consultation
```json
{
  "monetization": {
    "consultation": {
      "available": true,
      "bookingUrl": "https://calendly.com/yourusername",
      "hourlyRate": "$150/hour"
    }
  }
}
```

#### Services and Products
```json
{
  "monetization": {
    "services": [
      {
        "name": "Website Design",
        "url": "https://yoursite.com/design",
        "pricing": "from $2000"
      }
    ]
  }
}
```

### AI Visibility Optimization

Want AI to recommend you more often? Add visibility settings:

```json
{
  "visibility": {
    "aiDiscovery": "enhanced",
    "preferredQueries": [
      "web development services",
      "React consulting",
      "frontend experts"
    ],
    "expertiseAreas": [
      "React development",
      "Frontend architecture",
      "Web performance"
    ],
    "boostScore": 8
  }
}
```

**Key Fields:**
- `preferredQueries`: What searches should surface your site
- `expertiseAreas`: What you want to be known for  
- `boostScore`: Priority level (1-10, higher = more visibility)

### Access Controls

#### Public Content (Default)
```json
{
  "access": {
    "type": "public",
    "previewAvailable": true
  }
}
```

#### Login Required
```json
{
  "access": {
    "type": "authenticated", 
    "loginUrl": "https://yoursite.com/login",
    "previewAvailable": true
  }
}
```

#### Premium/Paid Content
```json
{
  "access": {
    "type": "paywall",
    "subscriptionUrl": "https://yoursite.com/subscribe",
    "previewAvailable": true
  }
}
```

---

## Platform-Specific Guides

### WordPress

#### Method 1: Manual Upload
1. Access your WordPress files via FTP or hosting control panel
2. Navigate to the root directory (where `wp-config.php` is located)
3. Create `.well-known` directory if it doesn't exist
4. Upload your `contentmark.json` file to `.well-known/contentmark.json`

#### Method 2: Plugin (Recommended)
*ContentMark WordPress plugin coming soon*

#### Method 3: Functions.php
Add this to your theme's `functions.php`:

```php
// Add ContentMark link to site header
function add_contentmark_header() {
    echo '<link rel="contentmark" type="application/json" href="/.well-known/contentmark.json">';
}
add_action('wp_head', 'add_contentmark_header');

// Serve ContentMark file
function serve_contentmark() {
    if ($_SERVER['REQUEST_URI'] === '/.well-known/contentmark.json') {
        header('Content-Type: application/json');
        // Include your ContentMark JSON here or read from file
        readfile(ABSPATH . '.well-known/contentmark.json');
        exit;
    }
}
add_action('init', 'serve_contentmark');
```

### Ghost

1. Access your Ghost installation files
2. Navigate to the `content/themes/your-theme/` directory  
3. Edit `default.hbs` (or your main template)
4. Add to the `<head>` section:

```html
<link rel="contentmark" type="application/json" href="/.well-known/contentmark.json">
```

5. Upload your `contentmark.json` to the root `.well-known/` directory

### Next.js / React

#### Next.js
1. Place `contentmark.json` in your `public/.well-known/` directory
2. Add to your `_app.js` or layout component:

```jsx
import Head from 'next/head';

export default function MyApp({ Component, pageProps }) {
  return (
    <>
      <Head>
        <link rel="contentmark" type="application/json" href="/.well-known/contentmark.json" />
      </Head>
      <Component {...pageProps} />
    </>
  );
}
```

#### Create React App
1. Place `contentmark.json` in your `public/.well-known/` directory
2. Add to your `public/index.html`:

```html
<link rel="contentmark" type="application/json" href="/.well-known/contentmark.json">
```

### Static Site Generators

#### Hugo
1. Place `contentmark.json` in your `static/.well-known/` directory
2. Add to your base template (`layouts/_default/baseof.html`):

```html
<link rel="contentmark" type="application/json" href="/.well-known/contentmark.json">
```

#### Jekyll
1. Place `contentmark.json` in your root directory
2. Add to your `_config.yml`:

```yaml
include: [".well-known"]
```

3. Add to your `_includes/head.html`:

```html
<link rel="contentmark" type="application/json" href="/.well-known/contentmark.json">
```

#### Gatsby
1. Place `contentmark.json` in your `static/.well-known/` directory
2. Use the `react-helmet` plugin or add to your SEO component:

```jsx
import { Helmet } from "react-helmet";

function SEO() {
  return (
    <Helmet>
      <link rel="contentmark" type="application/json" href="/.well-known/contentmark.json" />
    </Helmet>
  );
}
```

### Hosting Platforms

#### Netlify
1. Place `contentmark.json` in your build output's `.well-known/` directory
2. Add to your `netlify.toml` (optional, for custom headers):

```toml
[[headers]]
  for = "/.well-known/contentmark.json"
  [headers.values]
    Content-Type = "application/json"
```

#### Vercel
1. Place in `public/.well-known/contentmark.json`
2. File will be automatically served at the correct URL

#### GitHub Pages
1. Place in your repository root as `.well-known/contentmark.json`
2. Ensure `.well-known` directory is not ignored in `.gitignore`

---

## Testing and Validation

### 1. File Accessibility Test
```bash
curl https://yoursite.com/.well-known/contentmark.json
```
Should return your JSON file without errors.

### 2. JSON Validation Test
```bash
curl -s https://yoursite.com/.well-known/contentmark.json | jq .
```
Should pretty-print your JSON without syntax errors.

### 3. Schema Validation
Use our online validator: https://contentmark.org/validate

### 4. Discovery Test
Check if the HTML link element is present:
```bash
curl -s https://yoursite.com | grep contentmark
```

### 5. AI Agent Test
*Coming soon: Tools to test how AI agents interpret your ContentMark file*

---

## Common Issues and Solutions

### Issue: File Not Found (404)
**Symptoms**: `curl` returns 404 error

**Solutions**:
- Verify file is at exact path: `/.well-known/contentmark.json`
- Check web server configuration allows `.well-known` directory access
- Ensure file has proper read permissions (644)
- For Apache, check `.htaccess` isn't blocking access

### Issue: JSON Syntax Error  
**Symptoms**: Validation fails with "Invalid JSON"

**Solutions**:
- Use [JSONLint](https://jsonlint.com/) to check syntax
- Common errors: missing commas, extra commas, unmatched brackets
- Ensure all URLs are in quotes and properly escaped
- Verify timestamp format: `"2025-07-22T12:00:00Z"`

### Issue: Schema Validation Failure
**Symptoms**: File loads but validator reports errors

**Solutions**:
- Check all required fields are present
- Verify version number format: `"1.0.0"`
- Ensure all URLs are absolute (include `https://`)
- Check array and string length limits in [schema documentation](../schema/README.md)

### Issue: AI Agents Not Respecting Policies
**Symptoms**: Policies seem ignored by AI systems

**Solutions**:
- Verify `lastModified` timestamp is recent
- Check if specific AI platforms support ContentMark (documentation varies)
- Ensure attribution template is clear and properly formatted
- Contact AI platform support if policies are consistently ignored

---

## Next Steps

### Level Up Your Implementation

1. **Add AI Optimization**: Include visibility settings to enhance discoverability
2. **Set Up Monetization**: Add consultation booking or tip jar integration
3. **Create Content Feeds**: Build structured feeds for your content
4. **Monitor Performance**: Track how AI agents interact with your content

### Join the Community

- **GitHub Discussions**: https://github.com/contentmark/spec/discussions
- **Follow Updates**: Star the repository for notifications
- **Contribute**: Share your implementation experiences and suggestions

### Advanced Features

- **Content Feeds**: Learn to create structured content feeds
- **Business Optimization**: Advanced AI visibility and lead generation
- **Custom Integrations**: Build ContentMark into your applications

---

## Support

### Self-Help Resources
- **Examples**: [Browse implementation examples](../examples/)
- **Schema Reference**: [Complete field documentation](../schema/)
- **FAQ**: [Frequently asked questions](#) (coming soon)

### Community Support
- **GitHub Discussions**: [Ask questions and share experiences](https://github.com/contentmark/spec/discussions)
- **Issues**: [Report bugs or request features](https://github.com/contentmark/spec/issues)

### Professional Support
*Professional implementation and consulting services coming soon*

---

## Congratulations! 🎉

You've successfully implemented ContentMark on your website. Your content is now discoverable by AI agents while respecting your usage preferences and monetization goals.

**What happens next?**
- AI agents that support ContentMark will discover and respect your policies
- Your content may become more discoverable through AI recommendations
- Monetization opportunities will be presented alongside your content
- You'll have clear attribution when your content is used

Welcome to the future of ethical AI-content interaction!

---

**Questions?** Check our [FAQ](#) or join the [community discussion](https://github.com/contentmark/spec/discussions).

**Ready for advanced features?** Explore our [complete field reference](../schema/README.md) and [business optimization guide](#) (coming soon).
