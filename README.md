# ContentMark Protocol Specification

**The Content Access Protocol for Ethical AI-Content Interaction**

ContentMark is an open standard that gives content creators control over how AI agents use their work. It provides structured permissions, monetization options, and AI visibility optimization in a simple, adoptable format.

## Quick Start

Add a `.contentmark.json` file to your website's `/.well-known/` directory:

```json
{
  "version": "1.0.0",
  "siteName": "Your Website",
  "defaultUsagePolicy": {
    "canSummarize": true,
    "canTrain": false,
    "canQuote": true,
    "mustAttribute": true
  },
  "lastModified": "2025-07-22T12:00:00Z"
}
```

That's it! AI agents can now discover and respect your content policies.

## What ContentMark Enables

### 🔐 **Control AI Usage**
- Define what AI can do with your content
- Require attribution and proper crediting
- Block or allow specific types of AI processing

### 💰 **Monetize AI Traffic**
- Include tip jars and donation links
- Add sponsorship and consultation information  
- Enable subscription and premium content flows

### 🚀 **Optimize AI Visibility**
- Boost discoverability by AI agents
- Define expertise areas and preferred queries
- Guide how AI should present your content

### 🤝 **Enable Ethical AI**
- Move beyond adversarial blocking to collaboration
- Create sustainable content economy
- Respect creator rights while enabling AI innovation

## Core Features

- **Simple JSON format** - Easy to create and validate
- **Extensible schema** - Adapt to your specific needs  
- **Built on web standards** - Uses `.well-known/` URIs and JSON-LD
- **CMS integration** - Works with WordPress, Ghost, static sites
- **Open source** - Community-driven development

## Why ContentMark?

Current AI-content interaction is broken:
- **Creators** lack control over how AI uses their work
- **AI platforms** scrape without clear permissions
- **No monetization** path for creators in AI era
- **Binary solutions** - block everything or allow everything

ContentMark fixes this by providing:
- **Granular permissions** instead of all-or-nothing
- **Built-in monetization** instead of value extraction
- **AI optimization** instead of just blocking
- **Collaborative framework** instead of adversarial relationship

## Documentation

- [📖 **Full Specification**](./SPECIFICATION.md) - Complete protocol documentation
- [🔧 **Schema Reference**](./schema/) - JSON Schema definitions  
- [📋 **Examples**](./examples/) - Sample implementations
- [🚀 **Getting Started**](./docs/getting-started.md) - Implementation guide
- [🤝 **Contributing**](./CONTRIBUTING.md) - How to contribute

## Examples

### Basic Blog
```json
{
  "version": "1.0.0",
  "siteName": "Tech Blog",
  "defaultUsagePolicy": {
    "canSummarize": true,
    "canTrain": false,
    "canQuote": true,
    "mustAttribute": true,
    "attributionTemplate": "From {siteName} - {url}"
  },
  "monetization": {
    "tipJar": "https://buymeacoffee.com/techblog"
  }
}
```

### Business with AI Optimization
```json
{
  "version": "1.0.0",
  "siteName": "AI Consulting Experts",
  "defaultUsagePolicy": {
    "canSummarize": true,
    "canTrain": false,
    "canQuote": true,
    "mustAttribute": true
  },
  "visibility": {
    "aiDiscovery": "enhanced",
    "preferredQueries": ["AI consulting", "machine learning experts"],
    "expertiseAreas": ["artificial intelligence", "ML strategy"]
  },
  "monetization": {
    "consultation": {
      "available": true,
      "bookingUrl": "https://calendly.com/ai-experts"
    }
  }
}
```

### Premium Content
```json
{
  "version": "1.0.0",
  "siteName": "Premium Research",
  "defaultUsagePolicy": {
    "canSummarize": false,
    "canTrain": false,
    "canQuote": true,
    "mustAttribute": true
  },
  "access": {
    "type": "paywall",
    "previewAvailable": true,
    "subscriptionUrl": "https://premium-research.com/subscribe"
  }
}
```

## Tools & Resources

- [**Validator**](https://github.com/contentmark/cli) - Validate your ContentMark files
- [**CLI Tools**](https://github.com/contentmark/cli) - Command-line utilities with 7 templates, batch processing, and multiple output formats
- [**Website**](https://contentmark.org) - Official ContentMark website
- [**Community**](https://github.com/contentmark/spec/discussions) - Join the discussion

## Status

ContentMark is currently in **active development**. The core specification is stabilizing and we welcome feedback from early adopters.

- ✅ Core schema defined
- ✅ Basic validation tools
- 🔄 Community feedback integration
- ⏳ CMS plugins in development
- ⏳ AI platform partnerships

## Contributing

ContentMark is an open standard developed by the community. We welcome:

- 📝 **Specification feedback** - Review the protocol design
- 🐛 **Issue reports** - Found a problem? Let us know
- 💡 **Feature suggestions** - Ideas for improvements
- 🔧 **Implementation examples** - Show us how you're using it
- 📖 **Documentation** - Help improve our guides

See [CONTRIBUTING.md](./CONTRIBUTING.md) for details.

## License

The ContentMark specification is licensed under [MIT License](./LICENSE). You are free to implement, extend, and build upon this standard.

## Get Started

1. **Add ContentMark to your site**: Create `.well-known/contentmark.json`
2. **Validate your implementation**: Use our [online validator](https://contentmark.org/validate)
3. **Join the community**: [GitHub Discussions](https://github.com/contentmark/spec/discussions)
4. **Follow updates**: Star this repository for updates

---

**Building the ethical foundation for AI-content interaction.**

*ContentMark is a community-driven open standard for the AI era.*
