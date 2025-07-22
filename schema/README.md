# ContentMark JSON Schema

This directory contains the JSON Schema definitions for the ContentMark protocol.

## Schema Files

- [`contentmark.schema.json`](./contentmark.schema.json) - Complete JSON Schema for ContentMark v1.0.0

## What is JSON Schema?

[JSON Schema](https://json-schema.org/) is a vocabulary that allows you to annotate and validate JSON documents. It provides:

- **Structure validation** - Ensures required fields are present
- **Type checking** - Validates data types (string, boolean, number, etc.)
- **Format validation** - Checks URLs, dates, and other formats
- **Documentation** - Self-documenting with descriptions and examples

## Using the Schema

### Online Validation
Use our online validator: https://contentmark.org/validate

### Programmatic Validation

#### JavaScript/Node.js
```javascript
import Ajv from 'ajv';
import addFormats from 'ajv-formats';

const ajv = new Ajv();
addFormats(ajv);

// Load schema
const schema = await fetch('https://contentmark.org/schema/v1.0.0/contentmark.schema.json')
  .then(res => res.json());

const validate = ajv.compile(schema);

// Validate your ContentMark file
const valid = validate(yourContentMarkData);
if (!valid) {
  console.log(validate.errors);
}
```

#### Python
```python
import json
import jsonschema

# Load schema
with open('contentmark.schema.json', 'r') as f:
    schema = json.load(f)

# Load your ContentMark file
with open('your-contentmark.json', 'r') as f:
    data = json.load(f)

# Validate
try:
    jsonschema.validate(instance=data, schema=schema)
    print("✅ Valid ContentMark file!")
except jsonschema.exceptions.ValidationError as e:
    print(f"❌ Validation error: {e.message}")
```

#### PHP
```php
use JsonSchema\Validator;
use JsonSchema\Constraints\Constraint;

$validator = new Validator;
$data = json_decode(file_get_contents('your-contentmark.json'));
$schema = json_decode(file_get_contents('contentmark.schema.json'));

$validator->validate($data, $schema, Constraint::CHECK_MODE_VALIDATE_SCHEMA);

if ($validator->isValid()) {
    echo "✅ Valid ContentMark file!\n";
} else {
    echo "❌ Validation errors:\n";
    foreach ($validator->getErrors() as $error) {
        echo sprintf("[%s] %s\n", $error['property'], $error['message']);
    }
}
```

### IDE Integration

#### VS Code
Add this to your `.contentmark.json` files for automatic validation:

```json
{
  "$schema": "https://contentmark.org/schema/v1.0.0/contentmark.schema.json",
  "version": "1.0.0",
  // ... rest of your ContentMark file
}
```

#### IntelliJ/WebStorm
The schema will be automatically recognized when you include the `$schema` property.

## Schema Structure

### Root Object
The main ContentMark object with required and optional fields:

```json
{
  "version": "1.0.0",           // Required: Protocol version
  "siteName": "Your Site",      // Required: Site name
  "defaultUsagePolicy": { ... }, // Required: AI usage permissions
  "lastModified": "2025-07-22T12:00:00Z", // Required: Timestamp
  
  // Optional fields
  "description": "Site description",
  "feeds": [...],
  "visibility": { ... },
  "monetization": { ... },
  "access": { ... }
}
```

### Key Validation Rules

#### Required Fields
- `version` - Must follow semantic versioning (e.g., "1.0.0")
- `siteName` - 1-200 characters
- `defaultUsagePolicy` - Must include all usage permission booleans
- `lastModified` - Must be valid ISO 8601 datetime

#### String Limits
- `siteName`: 200 characters max
- `description`: 500 characters max
- `attributionTemplate`: 200 characters max
- `aiOptimizedDescription`: 500 characters max

#### Array Limits
- `preferredQueries`: 20 items max
- `expertiseAreas`: 15 items max
- `services`: 10 items max
- `callsToAction`: 5 items max

#### URL Validation
All URL fields must be absolute URLs (include protocol):
- ✅ `https://example.com/page`
- ❌ `example.com/page` (missing protocol)
- ❌ `/relative/path` (relative URL)

## Common Validation Errors

### Missing Required Fields
```json
// ❌ Error: Missing required fields
{
  "version": "1.0.0"
  // Missing: siteName, defaultUsagePolicy, lastModified
}

// ✅ Correct: All required fields present
{
  "version": "1.0.0",
  "siteName": "My Blog",
  "defaultUsagePolicy": {
    "canSummarize": true,
    "canTrain": false,
    "canQuote": true,
    "mustAttribute": true
  },
  "lastModified": "2025-07-22T12:00:00Z"
}
```

### Invalid Version Format
```json
// ❌ Error: Invalid version format
{
  "version": "v1.0" // Must be "1.0.0"
}

// ✅ Correct: Semantic versioning
{
  "version": "1.0.0"
}
```

### Invalid URL Format
```json
// ❌ Error: Invalid URLs
{
  "monetization": {
    "tipJar": "buymeacoffee.com/user" // Missing protocol
  }
}

// ✅ Correct: Absolute URL
{
  "monetization": {
    "tipJar": "https://buymeacoffee.com/user"
  }
}
```

### Array Length Violations
```json
// ❌ Error: Too many items (max 20)
{
  "visibility": {
    "preferredQueries": [
      // ... 25 items (exceeds limit)
    ]
  }
}

// ✅ Correct: Within limits
{
  "visibility": {
    "preferredQueries": [
      "web development",
      "AI consulting" // 2 items (within limit)
    ]
  }
}
```

## Testing Your Schema Changes

If you're contributing to the schema, test your changes:

### 1. Validate the Schema Itself
```bash
# Using ajv-cli
npx ajv compile -s contentmark.schema.json

# Using online validator
# Upload to https://www.jsonschemavalidator.net/
```

### 2. Test with Example Files
```bash
# Validate example files against schema
npx ajv validate -s contentmark.schema.json -d ../examples/*.json
```

### 3. Test Edge Cases
Create test files for:
- Minimum required fields only
- Maximum field lengths
- Invalid formats
- Missing required fields

## Version History

- **v1.0.0** (July 2025) - Initial schema release
  - Complete field validation
  - All monetization and visibility features
  - Comprehensive documentation

## Contributing

When modifying the schema:

1. **Maintain backward compatibility** for minor versions
2. **Update examples** to match schema changes  
3. **Test thoroughly** with validation tools
4. **Document changes** in CHANGELOG.md
5. **Update version references** in documentation

## Support

- **Schema issues**: [GitHub Issues](https://github.com/contentmark/spec/issues)
- **Validation help**: [GitHub Discussions](https://github.com/contentmark/spec/discussions)
- **Online validator**: https://contentmark.org/validate

---

**Schema URL**: https://contentmark.org/schema/v1.0.0/contentmark.schema.json

*For the latest schema documentation, visit: https://contentmark.org/schema*
