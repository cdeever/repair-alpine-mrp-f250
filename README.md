# Alpine MRP-F250 Repair Guide

A comprehensive repair guide for the Alpine MRP-F250 4-Channel Power Amplifier, built as a Hugo static site.

## Live Site

View the documentation at: https://cdeever.github.io/repair-alpine-mrp-f250/

## About

This guide was developed while diagnosing and repairing an MRP-F250 with DC/DC converter failures. It includes:

- **Circuit Theory** - How the amplifier works at block and component level
- **Diagnostic Procedures** - Systematic test plans with expected values
- **Repair Procedures** - Common failure modes and repair instructions
- **Reference Data** - Specifications, terminal voltages, and parts lists

## Building Locally

### Prerequisites

- [Hugo Extended](https://gohugo.io/installation/) (v0.128.0 or later)
- Git

### Clone and Build

```bash
# Clone with submodules
git clone --recurse-submodules https://github.com/cdeever/repair-alpine-mrp-f250.git
cd repair-alpine-mrp-f250

# Start development server
hugo server -D

# Build for production
hugo --gc --minify
```

## Source Material

Based on Alpine Service Manual **68E39196S01** (January 2006 revision).

## Contributing

Contributions welcome! If you have additional repair information, corrections, or improvements:

1. Fork the repository
2. Create a feature branch
3. Submit a pull request

## License

Documentation content is provided for educational and repair purposes.
