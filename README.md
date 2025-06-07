

## Usage
Run the script with the following command:
```bash
./google-suggest.bsh <seed_keyword> [country] [depth] [output_file]
```

## Description
This script utilizes Google’s autocomplete API to fetch search suggestions based on a seed keyword. It can recursively expand suggestions to a specified depth and save the results in a CSV format.

## Features
- Fetches Google search suggestions for a given seed keyword.
- Supports country code specification for localized suggestions (default is 'us').
- Allows setting the depth of suggestion levels to fetch.
- Outputs results to a specified CSV file, with a default naming convention based on timestamp and process ID.

## Requirements
- `xmllint`: Required for parsing XML responses from the Google API. Ensure it is installed on your system.
- `jq`: Used for URL encoding the search keyword.

## Functions
1. **usage**: Displays usage information and exits.
2. **check_xmllint**: Checks if `xmllint` is installed.
3. **url_encode**: URL-encodes a given string.
4. **fetch_suggestions**: Fetches suggestions from Google’s API for a given keyword.
5. **expand_keywords**: Recursively fetches suggestions for related keywords.
6. **process_keywords**: Initiates the keyword expansion process.

## Example
To fetch suggestions for the keyword "example" and save them to a file named "output.csv":
```bash
./google-suggest.bsh example us 2 output.csv
```

## Example Output

```bash
./google-suggest.bsh "best coffee" us 1 output.csv
best coffee,best coffee maker
best coffee,best coffee near me
best coffee,best coffee grinder
best coffee,best coffee maker 2024
best coffee,best coffee shops near me
best coffee,best coffee
best coffee,best coffee beans
best coffee,best coffee mugs
best coffee,best coffee machine for home
best coffee,best coffee maker with grinder
```

# Google Suggest

This project is a Bash script that fetches Google search suggestions for a given seed keyword and saves them to a CSV file.

## Description

This is a Bash script designed to automate the fetching and expansion of Google search suggestions. Given a "seed" keyword, it recursively fetches related suggestions from Google's autocomplete API and saves them to a CSV file. This can be useful for keyword research, content ideation, or understanding search trends.

The script leverages `curl` to interact with Google's API, `xmllint` to parse the XML response, and `jq` for robust URL encoding.

## Features

*   **Recursive Suggestion Fetching:** Expands keywords to multiple levels of suggestions.
*   **Country-Specific Results:** Get suggestions tailored to a specific Google country domain.
*   **Customizable Depth:** Control how many levels deep the script explores suggestions.
*   **CSV Output:** Stores results in a simple `parent_keyword,suggestion` CSV format.
*   **Basic Rate Limiting:** Includes a `sleep` delay between requests to reduce the risk of IP blocking.
*   **Dependency Checks:** Verifies that required tools (`curl`, `xmllint`, `jq`) are installed.
*   **Robust Argument Parsing:** Uses `getopts` for clear and flexible command-line arguments.

## Prerequisites

Before running the script, ensure you have the following commands installed on your system:

*   **`bash`**: The shell itself (usually pre-installed).
*   **`curl`**: A command-line tool for transferring data with URLs.
    *   **Installation (Debian/Ubuntu):** `sudo apt-get install curl`
    *   **Installation (Fedora/RHEL):** `sudo dnf install curl`
    *   **Installation (macOS):** `brew install curl` (if using Homebrew)
*   **`xmllint`**: A command-line XML tool, typically part of the `libxml2-utils` package.
    *   **Installation (Debian/Ubuntu):** `sudo apt-get install libxml2-utils`
    *   **Installation (Fedora/RHEL):** `sudo dnf install libxml2`
    *   **Installation (macOS):** `brew install libxml2` (if using Homebrew)
*   **`jq`**: A lightweight and flexible command-line JSON processor, used here for URL encoding.
    *   **Installation (Debian/Ubuntu):** `sudo apt-get install jq`
    *   **Installation (Fedora/RHEL):** `sudo dnf install jq`
    *   **Installation (macOS):** `brew install jq` (if using Homebrew)

## Installation

1.  **Clone the repository (or download the script):**
    ```bash
    git clone https://github.com/your-username/google-suggestion-scraper.git
    cd google-suggestion-scraper
    ```
    (Replace `your-username/google-suggestion-scraper.git` with the actual repository URL if you host it)

2.  **Make the script executable:**
    ```bash
    chmod +x singlekeyword.bsh
    ```

## Usage

```
./singlekeyword.bsh -k <seed_keyword> [-c <country>] [-d <depth>] [-o <output_file>]
```

**Options:**

*   `-k <seed_keyword>`: **(Mandatory)** The initial keyword to start the search suggestion process.
*   `-c <country>`: The 2-letter country code (e.g., `us`, `gb`, `de`). Defaults to `us`.
*   `-d <depth>`: The number of suggestion levels to fetch. A depth of `1` fetches direct suggestions for the seed keyword. A depth of `2` fetches direct suggestions, then suggestions for each of those suggestions, and so on. Defaults to `1`.
*   `-o <output_file>`: The name of the output CSV file. Defaults to `keywords.<timestamp>.<pid>.csv` (e.g., `keywords.20231120.143000.12345.csv`).
*   `-h`: Display the usage help message.

### Examples

1.  **Fetch direct suggestions for "marketing tools" in the US:**
    ```bash
    ./singlekeyword.bsh -k "marketing tools"
    ```

2.  **Fetch direct suggestions for "buy shoes" in Germany:**
    ```bash
    ./singlekeyword.bsh -k "buy shoes" -c de
    ```

3.  **Fetch suggestions for "best coffee" up to 2 levels deep:**
    ```bash
    ./singlekeyword.bsh -k "best coffee" -d 2
    ```

4.  **Fetch suggestions for "travel planning" up to 3 levels deep, save to a custom file:**
    ```bash
    ./singlekeyword.bsh -k "travel planning" -c us -d 3 -o travel_keywords.csv
    ```

5.  **Display help:**
    ```bash
    ./singlekeyword.bsh -h
    ```

## Output Format

The script generates a CSV file where each line contains:
`parent_keyword,"extracted_suggestion"`

The `extracted_suggestion` is quoted to handle commas or special characters within the suggestion itself.

**Example `output.csv` content (for `-k "coffee" -d 2`):**

```csv
coffee,"coffee near me"
coffee,"coffee maker"
coffee,"coffee shop"
coffee,"coffee beans"
coffee,"coffee table"
coffee near me,"coffee near me open now"
coffee near me,"coffee near me starbucks"
coffee near me,"coffee near me drive thru"
coffee maker,"coffee maker with grinder"
coffee maker,"coffee maker single serve"
# ... and so on for other suggestions
```

## Limitations and Warnings

*   **Unofficial API:** This script uses Google's internal autocomplete suggestion API (`suggestqueries.google.com`). This API is not officially supported for public use, its structure or existence may change at any time without notice, potentially breaking the script.
*   **IP Blocking/CAPTCHAs:** Frequent or heavy usage of this script can lead to your IP address being temporarily blocked by Google or trigger CAPTCHA challenges, as it mimics automated behavior. The built-in `sleep 2` helps mitigate this but is not foolproof.
*   **Terms of Service:** Automated querying of Google's services may violate their Terms of Service. Use this script responsibly and at your own risk.
*   **Fixed User-Agent:** The script uses a fixed User-Agent string. While it rotates through a small list, sophisticated bot detection systems might still identify this.
*   **Network Dependence:** The script relies on an active internet connection to reach Google's servers.

## Contributing

Feel free to fork the repository and submit pull requests if you have improvements or additional detection methods to contribute.

### Steps to Contribute

1. Fork the repository.
2. Create a new branch for your feature or bug fix.
3. Make your changes.
4. Commit your changes and push to your fork.
5. Submit a pull request.

## License

This project is licensed under the **GNU General Public License v3.0**. See the [LICENSE](LICENSE) file for details.
