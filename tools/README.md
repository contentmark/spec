# ContentMark Tools & Utilities

This directory contains information about tools, utilities, and integrations for working with the ContentMark protocol.

## Official Tools

### Online Validator
**URL**: https://contentmark.org/validate  
**Purpose**: Validate your ContentMark manifests against the official schema  
**Features**:
- Real-time JSON validation
- Schema compliance checking
- Error reporting with suggestions
- URL validation for remote files

### CLI Tools
**Repository**: [@contentmark/cli](https://github.com/contentmark/cli)  
**Installation**: `npm install -g @contentmark/cli`  
**Status**: Available  
**Usage**:
```bash
# Validate local file
contentmark validate .contentmark.json

# Validate remote URL
contentmark validate https://example.com/.well-known/contentmark.json

# Generate sample file
contentmark generate --type blog > .contentmark.json

# Check website for ContentMark support
contentmark check https://example.com

# Batch check multiple websites
contentmark batch-check urls.txt --format json

# Initialize ContentMark in current directory
contentmark init --type business
```

**Features**:
- Multiple output formats (JSON, YAML, summary)
- Batch processing with progress indicators
- Configuration file support (.contentmarkrc.json)
- 7 template types: blog, business, premium, ecommerce, news, education, api
- Enhanced error reporting and validation

### Browser Extension (Planned)
**Purpose**: Detect and validate ContentMark support on websites  
**Features**:
- Visual indicator for ContentMark-enabled sites
- Quick policy overview
- Validation status
- Monetization link shortcuts

---

## Community Tools

### Generators & Converters

#### Markdown to ContentMark Generator
**Status**: Community project  
**Purpose**: Generate ContentMark manifests from Markdown frontmatter  
**Usage**:
```bash
# Convert blog post metadata to ContentMark
markdown-to-contentmark posts/*.md --output .contentmark.json
```

#### YAML to JSON Converter
**Purpose**: Convert YAML-based configurations to ContentMark JSON  
**Online Tool**: https://yaml-to-json.com  
**Usage**: Paste YAML configuration, get valid ContentMark JSON

### Configuration File Support
**File**: `.contentmarkrc.json`  
**Purpose**: Configure CLI behavior and default settings  
**Location**: Project root or user home directory  

**Example Configuration**:
```json
{
  "validation": {
    "schemaUrl": "https://schema.contentmark.org/v1/manifest.json",
    "strictMode": false,
    "customRules": []
  },
  "output": {
    "format": "json",
    "verbose": false,
    "colors": true
  },
  "batch": {
    "concurrency": 5,
    "timeout": 10000,
    "retries": 2
  }
}
```

**Configuration Sections**:
- `validation`: Schema URL, validation mode, custom rules
- `output`: Default format, verbosity, color output
- `batch`: Concurrency limits, timeouts, retry logic

### Content Management System Plugins

#### WordPress Plugin (In Development)
**Repository**: `contentmark/wordpress-plugin`  
**Features**:
- Automatic manifest generation
- Admin interface for policy configuration
- Integration with existing monetization plugins
- SEO and analytics integration

**Installation** (when available):
```bash
# Via WordPress admin
Plugins > Add New > Search "ContentMark"

# Manual installation
wp plugin install contentmark --activate
```

#### Ghost Integration (Community)
**Type**: Theme modification  
**Purpose**: Add ContentMark support to Ghost blogs  
**Implementation**:
```javascript
// Add to Ghost theme
{{#contentfor "head"}}
<link rel="contentmark" type="application/json" href="/.well-known/contentmark.json">
{{/contentfor}}
```

#### Drupal Module (Planned)
**Status**: Seeking contributors  
**Purpose**: Native ContentMark support for Drupal sites  
**Features**: Field mapping, automatic generation, multi-site support

### Static Site Generators

#### Hugo Module
**Repository**: `contentmark/hugo-module`  
**Installation**:
```yaml
# hugo.yaml
module:
  imports:
    - path: github.com/contentmark/hugo-module
```

#### Jekyll Plugin
**Gem**: `jekyll-contentmark`  
**Installation**:
```ruby
# Gemfile
gem 'jekyll-contentmark'

# _config.yml
plugins:
  - jekyll-contentmark
```

#### Next.js Integration
**Package**: `@contentmark/nextjs`  
**Installation**:
```bash
npm install @contentmark/nextjs
```

**Usage**:
```javascript
// next.config.js
const { withContentMark } = require('@contentmark/nextjs');

module.exports = withContentMark({
  contentmark: {
    autoGenerate: true,
    policies: {
      canSummarize: true,
      canTrain: false
    }
  }
});
```

---

## Development Tools

### JSON Schema Validator
**Purpose**: Programmatic validation in development workflows  
**Languages**: JavaScript, Python, PHP, Go

#### JavaScript/Node.js
```bash
npm install ajv ajv-formats
```

```javascript
import Ajv from 'ajv';
import addFormats from 'ajv-formats';

const ajv = new Ajv();
addFormats(ajv);

const schema = await fetch('https://contentmark.org/schema/v1.0.0/contentmark.schema.json')
  .then(res => res.json());

const validate = ajv.compile(schema);
const isValid = validate(yourContentMarkData);
```

#### Python
```bash
pip install jsonschema requests
```

```python
import json
import requests
from jsonschema import validate

# Load schema
schema_url = 'https://contentmark.org/schema/v1.0.0/contentmark.schema.json'
schema = requests.get(schema_url).json()

# Validate your data
try:
    validate(instance=your_data, schema=schema)
    print("✅ Valid ContentMark file!")
except ValidationError as e:
    print(f"❌ Validation error: {e.message}")
```

#### PHP
```bash
composer require justinrainbow/json-schema
```

```php
use JsonSchema\Validator;

$validator = new Validator;
$data = json_decode(file_get_contents('.contentmark.json'));
$schema = json_decode(file_get_contents('contentmark.schema.json'));

$validator->validate($data, $schema);

if ($validator->isValid()) {
    echo "✅ Valid ContentMark file!\n";
} else {
    foreach ($validator->getErrors() as $error) {
        echo sprintf("❌ [%s] %s\n", $error['property'], $error['message']);
    }
}
```

### CI/CD Integration

#### GitHub Actions
```yaml
# .github/workflows/contentmark.yml
name: Validate ContentMark
on: [push, pull_request]

jobs:
  validate:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Validate ContentMark
        run: |
          curl -s https://contentmark.org/schema/v1.0.0/contentmark.schema.json > schema.json
          npx ajv validate -s schema.json -d .well-known/contentmark.json
```

#### GitLab CI
```yaml
# .gitlab-ci.yml
validate_contentmark:
  stage: test
  script:
    - curl -s https://contentmark.org/schema/v1.0.0/contentmark.schema.json > schema.json
    - npm install -g ajv-cli
    - ajv validate -s schema.json -d .well-known/contentmark.json
```

#### Pre-commit Hook
```bash
#!/bin/sh
# .git/hooks/pre-commit

# Validate ContentMark before commit
if [ -f ".well-known/contentmark.json" ]; then
    echo "Validating ContentMark manifest..."
    npx ajv validate \
        -s https://contentmark.org/schema/v1.0.0/contentmark.schema.json \
        -d .well-known/contentmark.json
    
    if [ $? -ne 0 ]; then
        echo "❌ ContentMark validation failed. Commit aborted."
        exit 1
    fi
    echo "✅ ContentMark validation passed."
fi
```

---

## Monitoring & Analytics

### Link Tracking Tools
**Purpose**: Monitor clicks on monetization and attribution links

#### Google Analytics Integration
```json
{
  "monetization": {
    "tipJar": "https://buymeacoffee.com/user?utm_source=contentmark&utm_medium=ai&utm_campaign=tips"
  },
  "renderHints": {
    "callToAction": {
      "url": "https://example.com?utm_source=contentmark&utm_medium=ai"
    }
  }
}
```

#### Custom Analytics
```javascript
// Track ContentMark-related events
function trackContentMarkClick(type, url) {
  gtag('event', 'contentmark_click', {
    'event_category': 'ContentMark',
    'event_label': type,
    'value': url
  });
}
```

### Attribution Monitoring (Community Project)
**Purpose**: Track how AI platforms use and attribute your content  
**Status**: Seeking developers  
**Features**:
- Web scraping for attribution compliance
- Alert system for policy violations
- Attribution quality scoring
- Backlink analysis

---

## Testing Tools

### Local Development Server
**Purpose**: Test ContentMark files during development

```javascript
// simple-server.js
const express = require('express');
const app = express();

app.use(express.static('.'));
app.get('/.well-known/contentmark.json', (req, res) => {
  res.setHeader('Content-Type', 'application/json');
  res.sendFile(__dirname + '/.well-known/contentmark.json');
});

app.listen(3000, () => {
  console.log('Test server running on http://localhost:3000');
  console.log('ContentMark available at: http://localhost:3000/.well-known/contentmark.json');
});
```

### Discovery Testing
```bash
#!/bin/bash
# test-discovery.sh

URL=$1
if [ -z "$URL" ]; then
  echo "Usage: $0 <website-url>"
  exit 1
fi

echo "Testing ContentMark discovery for: $URL"
echo

# Test 1: Direct file access
echo "1. Testing direct file access..."
curl -s -o /dev/null -w "Status: %{http_code}\n" "$URL/.well-known/contentmark.json"

# Test 2: Content-Type header
echo "2. Testing Content-Type header..."
curl -s -I "$URL/.well-known/contentmark.json" | grep -i content-type

# Test 3: JSON validity
echo "3. Testing JSON validity..."
curl -s "$URL/.well-known/contentmark.json" | jq . > /dev/null && echo "✅ Valid JSON" || echo "❌ Invalid JSON"

# Test 4: Schema validation
echo "4. Testing schema validation..."
curl -s "$URL/.well-known/contentmark.json" > temp.json
curl -s https://contentmark.org/schema/v1.0.0/contentmark.schema.json > schema.json
ajv validate -s schema.json -d temp.json && echo "✅ Valid schema" || echo "❌ Schema validation failed"
rm temp.json schema.json
```

---

## API & SDKs

### ContentMark JavaScript SDK (Planned)
**Purpose**: Easy integration for web applications

```javascript
import { ContentMark } from '@contentmark/sdk';

const cm = new ContentMark('https://example.com');
await cm.load();

// Check policies
if (cm.canSummarize()) {
  console.log('Summarization allowed');
}

// Get attribution
const attribution = cm.getAttribution();
console.log(attribution); // "From Example Site - https://example.com"

// Get monetization options
const monetization = cm.getMonetization();
if (monetization.tipJar) {
  // Show tip jar option
}
```

### ContentMark Python SDK (Community)
**Purpose**: Server-side ContentMark processing

```python
from contentmark import ContentMark

cm = ContentMark.from_url('https://example.com')

# Check policies
if cm.can_summarize:
    print("Summarization allowed")

# Get attribution
attribution = cm.get_attribution()
print(attribution)

# Check monetization
if cm.monetization.consultation.available:
    print(f"Consultation available: {cm.monetization.consultation.booking_url}")
```

---

## Contributing Tools

### Tool Development Guidelines

1. **Follow ContentMark principles**:
   - Respect creator intentions
   - Support monetization opportunities
   - Maintain attribution integrity

2. **Use official schema**:
   - Always validate against official JSON Schema
   - Support all standard fields
   - Handle optional fields gracefully

3. **Community collaboration**:
   - Share tools with the community
   - Accept feedback and contributions
   - Document usage and examples

### Wanted Tools & Integrations

**High Priority:**
- [ ] Shopify app for e-commerce sites
- [ ] Squarespace plugin
- [ ] Wix application
- [ ] Medium integration
- [ ] LinkedIn article support

**Medium Priority:**
- [ ] WYSIWYG ContentMark editor
- [ ] Bulk validation tool for large sites
- [ ] Analytics dashboard
- [ ] AI platform compliance checker

**Community Requests:**
- [ ] Notion database integration
- [ ] Airtable sync tool
- [ ] Google Sheets generator
- [ ] Slack bot for team notifications

---

## Tool Submission

### Submit Your Tool

Have you built a ContentMark tool? We'd love to feature it!

**Submission Requirements:**
- Open source or free to use
- Follows ContentMark specification
- Includes documentation and examples
- Actively maintained

**How to Submit:**
1. Create a [GitHub Discussion](https://github.com/contentmark/spec/discussions)
2. Include tool description, features, and links
3. Provide usage examples
4. Show ContentMark compliance

**What We'll Add:**
- Tool listing in this directory
- Link from main documentation
- Community showcase feature
- Social media promotion

### Tool Quality Standards

**Required Features:**
- ✅ JSON Schema validation
- ✅ Error handling and reporting
- ✅ Clear documentation
- ✅ Example usage

**Recommended Features:**
- ✅ CLI and programmatic interfaces
- ✅ Integration with popular platforms
- ✅ Automated testing
- ✅ Regular updates

---

## Support & Resources

### Tool Development Support
- **Documentation**: [ContentMark Specification](../SPECIFICATION.md)
- **Schema**: [JSON Schema Reference](../schema/)
- **Examples**: [Implementation Examples](../examples/)
- **Community**: [GitHub Discussions](https://github.com/contentmark/spec/discussions)

### Report Tool Issues
- **Tool bugs**: Report to the tool maintainer
- **Specification issues**: [GitHub Issues](https://github.com/contentmark/spec/issues)
- **Feature requests**: [GitHub Discussions](https://github.com/contentmark/spec/discussions)

### Stay Updated
- **Star the repository** for tool announcements
- **Join discussions** to influence tool development
- **Follow releases** for specification updates

---

**Building a ContentMark tool?** We're here to help! Join our [community discussions](https://github.com/contentmark/spec/discussions) to get support, feedback, and promotion for your project.

*The ContentMark ecosystem grows through community contributions. Every tool makes the protocol more accessible and valuable for creators worldwide.*
