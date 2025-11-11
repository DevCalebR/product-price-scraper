# Product Price Scraper

A flexible, production-ready web scraper for extracting product information from e-commerce websites. Built with Python, this tool allows you to scrape product names and prices using custom CSS selectors.

## Features

- ✨ **Configurable CSS Selectors**: Adapt to any website structure
- 🔄 **Automatic Retry Logic**: Handles network failures gracefully
- 📝 **Comprehensive Logging**: Track scraping progress and errors
- 🛡️ **Anti-Blocking Measures**: User-agent rotation and request delays
- 💾 **CSV Export**: Save data with timestamps and source URLs
- ⚙️ **Command-Line Interface**: Easy to use and automate
- 🧹 **Data Cleaning**: Automatically cleans prices (removes currency symbols)

## Installation

1. **Clone or download this project**

2. **Install dependencies**:
   ```bash
   pip install -r requirements.txt
   ```

## Usage

### Basic Command

```bash
python scraper.py \
  --url "https://example.com/products" \
  --product-selector ".product" \
  --name-selector ".name" \
  --price-selector ".price"
```

### Advanced Options

```bash
python scraper.py \
  --url "https://example.com/products" \
  --product-selector ".item" \
  --name-selector "h2.title" \
  --price-selector "span.cost" \
  --output "my_products.csv" \
  --max-retries 5 \
  --delay 2.0
```

### Command-Line Arguments

| Argument | Required | Default | Description |
|----------|----------|---------|-------------|
| `--url` | Yes | - | Target website URL |
| `--product-selector` | Yes | - | CSS selector for product containers |
| `--name-selector` | Yes | - | CSS selector for product names |
| `--price-selector` | Yes | - | CSS selector for product prices |
| `--output` | No | `data/products.csv` | Output CSV file path |
| `--max-retries` | No | `3` | Maximum retry attempts |
| `--delay` | No | `1.0` | Delay between requests (seconds) |

## Finding CSS Selectors

To use this scraper, you need to identify the correct CSS selectors for your target website:

### Step 1: Inspect the Website

1. Open the target website in Chrome or Firefox
2. Right-click on a product and select "Inspect" or "Inspect Element"
3. The browser's Developer Tools will open

### Step 2: Identify Selectors

Look for patterns in the HTML structure:

```html
<div class="product-card">
  <h3 class="product-title">Example Product</h3>
  <span class="product-price">$29.99</span>
</div>
```

For this structure, your selectors would be:
- `--product-selector ".product-card"`
- `--name-selector ".product-title"`
- `--price-selector ".product-price"`

### Tips for Finding Selectors

- **Classes**: Use `.classname` (most common)
- **IDs**: Use `#idname` (for unique elements)
- **Tags**: Use tag names like `h3`, `span`, `div`
- **Nested**: Combine selectors like `div.product h3.title`
- **Test**: Use browser console: `document.querySelectorAll('.product-card')`

## Output Format

The scraper generates a CSV file with the following columns:

| Column | Description |
|--------|-------------|
| `product_name` | Name of the product |
| `price` | Price (cleaned, no currency symbols) |
| `timestamp` | When the data was scraped |
| `source_url` | Source website URL |

Example output:

```csv
product_name,price,timestamp,source_url
Wireless Mouse,24.99,2025-11-11 14:30:22,https://example.com/products
Mechanical Keyboard,89.99,2025-11-11 14:30:22,https://example.com/products
USB-C Cable,12.99,2025-11-11 14:30:22,https://example.com/products
```

## Real-World Examples

### Example 1: Amazon-style Layout

```bash
python scraper.py \
  --url "https://example-store.com/electronics" \
  --product-selector "div[data-component-type='s-search-result']" \
  --name-selector "h2 span" \
  --price-selector ".a-price-whole"
```

### Example 2: Simple Grid Layout

```bash
python scraper.py \
  --url "https://shop.example.com/category/phones" \
  --product-selector ".product-grid-item" \
  --name-selector ".product-name" \
  --price-selector ".price"
```

### Example 3: Complex Nested Structure

```bash
python scraper.py \
  --url "https://marketplace.example.com/deals" \
  --product-selector "article.listing" \
  --name-selector "div.info h2 a" \
  --price-selector "div.pricing span.amount"
```

## Troubleshooting

### No Products Found

**Problem**: Scraper runs but finds 0 products.

**Solutions**:
1. Verify your CSS selectors using browser Developer Tools
2. Check if the website loads content dynamically with JavaScript (this scraper only works with static HTML)
3. Test your selector in the browser console: `document.querySelectorAll('your-selector')`

### Connection Errors

**Problem**: Scraper fails with connection timeout or 403/429 errors.

**Solutions**:
1. Increase the `--delay` parameter (e.g., `--delay 3.0`)
2. Increase `--max-retries` (e.g., `--max-retries 5`)
3. Check if the website blocks automated access (requires more advanced techniques)
4. Verify the URL is correct and accessible in your browser

### Incorrect Data Extracted

**Problem**: Wrong data appears in the CSV.

**Solutions**:
1. Double-check your selectors are specific enough
2. Inspect the HTML structure carefully - child selectors are relative to the product container
3. Use more specific selectors (e.g., `span.price` instead of just `.price`)

### Dynamic Content Not Loading

**Problem**: Website uses JavaScript to load products.

**Solutions**:
This scraper only works with static HTML. For JavaScript-heavy sites, consider:
- Using Selenium or Playwright instead
- Finding an API endpoint the website uses (check Network tab in Developer Tools)
- Checking if the site has an official API

## Legal and Ethical Considerations

⚠️ **Important**: Always respect website terms of service and robots.txt

- Check the website's `robots.txt` file (e.g., `https://example.com/robots.txt`)
- Review the website's Terms of Service for scraping policies
- Use appropriate delays between requests (`--delay` parameter)
- Don't overload servers with too many requests
- Respect copyright and data usage rights
- Some websites explicitly prohibit scraping

## Best Practices

1. **Start Small**: Test with a single page before scraping large amounts of data
2. **Be Respectful**: Use delays of 1-3 seconds between requests
3. **Monitor Logs**: Check the console output for errors and warnings
4. **Regular Testing**: Website structures change; test your selectors periodically
5. **Error Handling**: The scraper logs all errors - review them to improve your selectors

## Advanced Usage

### Using as a Python Module

You can also import and use the scraper in your own Python scripts:

```python
from scraper import ProductScraper

# Create scraper instance
scraper = ProductScraper(
    url="https://example.com/products",
    product_selector=".product",
    name_selector=".name",
    price_selector=".price",
    max_retries=3,
    delay=1.0
)

# Run scraper
success = scraper.scrape(output_path="custom_output.csv")

if success:
    print("Scraping completed successfully!")
else:
    print("Scraping failed!")
```

## Project Structure

```
product_scraper/
├── scraper.py          # Main scraper script
├── requirements.txt    # Python dependencies
├── README.md          # This file
└── data/              # Output directory (auto-created)
    └── products.csv   # Scraped data
```

## Dependencies

- **requests**: HTTP library for fetching web pages
- **beautifulsoup4**: HTML parsing and CSS selector support
- **lxml**: Fast XML/HTML parser (used by BeautifulSoup)

## Contributing

Feel free to modify and extend this scraper for your needs. Some ideas:

- Add support for pagination
- Export to JSON or database
- Add proxy support
- Implement concurrent scraping for multiple pages
- Add price comparison features
- Create a web UI

## License

This is a starter project for educational purposes. Use responsibly and in accordance with applicable laws and website terms of service.

## Support

If you encounter issues:

1. Check the troubleshooting section above
2. Review the logs for specific error messages
3. Verify your CSS selectors in the browser
4. Ensure the website is accessible and doesn't require authentication

---

**Happy Scraping! 🚀**