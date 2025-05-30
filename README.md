# EspressoNow ☕

Python CLI tool to find specialty coffee shops near your current location.

## Features

- 🌍 **Auto-location detection** - Automatically detects your current location
- 📍 **Custom location search** - Search near any address or coordinates
- ☕ **Specialty coffee focus** - Finds high-quality, specialty coffee shops
- 🎨 **Beautiful output** - Rich, colorful CLI interface with tables and progress bars
- 🔍 **Flexible search** - Customizable search radius and result limits
- 🗺️ **Google Places API integration** - Real-time coffee shop data from Google's database

## Installation

### From Source

```bash
git clone https://github.com/ethanqcarter/EspressoNow.git
cd EspressoNow
pip install -e .
```

### Dependencies

```bash
pip install -r requirements.txt
```

## Quick Start

### Configuration

**Required:** You'll need a Google Places API key to search for coffee shops:

1. Get an API key at [Google Places API](https://developers.google.com/places/web-service/get-api-key)
2. Set it as an environment variable:
   ```bash
   export GOOGLE_PLACES_API_KEY=your_key_here
   ```
3. Or create a `.env` file:
   ```bash
   cp env.example .env
   # Edit .env and add your API key
   ```

### Basic Usage

```bash
# Find coffee shops near your current location
espresso search

# Search with custom radius
espresso search --radius 5

# Search near a specific address
espresso search --location "123 Main St, San Francisco, CA"

# Search near coordinates
espresso search --location "37.7749,-122.4194"
```

## Commands

### `search` - Find coffee shops

```bash
espresso search [OPTIONS]

Options:
  -r, --radius FLOAT         Search radius (default: 3.0)
  -n, --max-results INTEGER  Maximum number of results (default: 10)
  -l, --location TEXT        Search location (address or "lat,lng")
  --api-key TEXT             Google Places API key
  --min-rating FLOAT         Minimum rating (e.g., 4.0 for >4 stars)
  --exclude-chains           Exclude chain coffee shops (Starbucks, Dunkin, etc.)
  --specialty-only           Show only specialty coffee (4+ stars, no chains) - DEFAULT
  --include-all              Include all coffee shops (disables specialty-only default)
  --miles                    Use miles instead of kilometers (DEFAULT)
  --km                       Use kilometers instead of miles
  --open                     Show only currently open coffee shops
  --export-yaml TEXT         Export results to YAML file (e.g., --export-yaml results.yaml)
  --help                     Show help message
```

### `config` - Show configuration

```bash
espresso config
```

Shows current configuration status and setup instructions.

## Examples

### Find coffee shops near current location
```bash
espresso search
```

### Search with 5-mile radius, max 20 results
```bash
espresso search --radius 5 --max-results 20
```

### Search near specific coordinates (San Francisco)
```bash
espresso search --location "37.7749,-122.4194"
```

### Search near coordinates (New York City)
```bash
espresso search --location "40.7128,-74.0060"
```

### Find only specialty coffee (4+ stars, no chains)
```bash
espresso search --specialty-only
```

### Show only currently open coffee shops
```bash
espresso search --open
```

### Exclude chain coffee shops
```bash
espresso search --exclude-chains
```

### Use kilometers instead of miles
```bash
espresso search --km
```

### Use specific API key
```bash
espresso search --api-key your_google_places_api_key
```

### Export results to YAML file
```bash
espresso search --export-yaml coffee_shops.yaml
```

### Export open coffee shops to YAML
```bash
espresso search --open --export-yaml open_shops.yaml
```

## Example Output

Here's what you'll see when searching for specialty coffee in San Francisco:

```bash
$ espresso search --location "37.7749,-122.4194" --radius 2 --max-results 10
```

## Output

EspressoNow displays results in a beautiful table format showing:

- ☕ **Name** - Coffee shop name
- 📍 **Address** - Street address
- ⭐ **Rating** - Star rating (1-5)
- 💰 **Price** - Price level ($-$$$$)
- 📏 **Distance** - Distance from search location
- 📞 **Phone** - Contact number

## YAML Export

EspressoNow can export search results to YAML files using the `--export-yaml` flag:

```bash
espresso search --location "San Francisco, CA" --export-yaml sf_coffee.yaml
```

### YAML Structure

The exported YAML file includes:

```yaml
%YAML 1.2
---
search_info:
  location:
    latitude: 37.7749
    longitude: -122.4194
  search_radius: 2.0
  radius_unit: miles
  total_results: 3
  exported_at: '2025-05-30T16:03:25.525805'
coffee_shops:
- name: Social Cafe
  address: 804 Bryant St, San Francisco, CA 94103, USA
  location:
    latitude: 37.775997
    longitude: -122.402995
  rating: 5.0
  price_level: 1
  phone: (415) 829-3259
  opening_hours:
  - 'Monday: 7:30 AM – 4:00 PM'
  - 'Tuesday: 7:30 AM – 3:00 PM'
  # ... more days
  distance: 1.45
  distance_unit: miles
  place_id: ChIJJTqnmmx_j4ARY4Gi004ITPc
# ... more coffee shops
...
```

**Features:**
- 📊 **Complete data export** - All coffee shop details and metadata
- 🎯 **Structured format** - Easy to parse and process programmatically  
- 🕐 **Timestamp included** - When the search was performed
- 📍 **Location context** - Search coordinates and radius
- ✨ **Clean formatting** - Formatted with `yaypp` for readable YAML structure

## API Integration

### Google Places API (New) with Pagination

EspressoNow uses the latest Google Places API (New) with intelligent pagination to find comprehensive coffee shop results:
- **Text Search API** with location bias for better coverage
- **Automatic pagination** using `nextPageToken` to get up to 60 results
- **Deduplication** to ensure no duplicate coffee shops
- **Real-time data** from Google's comprehensive database
- **Detailed place information** including ratings, prices, hours, and contact details
- **Supports up to 50km search radius**

**Note:** A Google Places API key is required for the application to function.

## Development

### Project Structure

```
espressonow/
├── __init__.py          # Package initialization
├── cli.py              # Command-line interface
├── core.py             # Core coffee shop finding logic
├── location.py         # Location detection and services
└── models.py           # Data models (Pydantic)
```

### Running Tests

```bash
# Install in development mode
pip install -e .

# Test the CLI
espresso search --help
espresso config

# Test with API key
export GOOGLE_PLACES_API_KEY=your_key_here
espresso search --location "San Francisco, CA"
```

## Changelog

### v0.3.0 (Latest)
- **🚀 Major Performance Improvement**: Implemented intelligent pagination using Google Places Text Search API
- **📈 10x Better Coverage**: Now finds 40+ coffee shops in 2km radius vs 3-4 previously
- **🔄 Comprehensive Results**: Uses `nextPageToken` to get up to 60 results with automatic deduplication
- **✅ Consistent Results**: Smaller radius results are always included in larger radius searches
- **⚡ Optimized API Usage**: Respectful delays between paginated requests
- **🎯 Better Search Strategy**: Switched from Nearby Search to Text Search API for superior coverage

### v0.2.0
- Google Places API (New) integration
- Real-time coffee shop data
- Beautiful CLI interface with Rich library
- Auto-location detection
- Custom location search
- Chain coffee shop filtering

### v0.1.0
- Initial release
- Basic coffee shop search functionality

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Add tests if applicable
5. Submit a pull request

## License

MIT License - see LICENSE file for details.

## Support

- 🐛 **Issues**: [GitHub Issues](https://github.com/ethanqcarter/EspressoNow/issues)

---

Made with ☕ and ❤️ by coffee enthusiasts, for coffee enthusiasts.
