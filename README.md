# Google Suggest

This project is a Bash script that fetches Google search suggestions for a given seed keyword and saves them to a CSV file.

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

