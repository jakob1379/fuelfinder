# Fuelfinder

<p align="center">
  <img src=".github/assets/fuelfinder-flow.svg" width="100%" alt="Diagram of how fuelfinder works: it reads the price table on fuelfinder.dk, with a row per fuel company and a column per fuel type, stores it in a cache under ~/.cache/fuelfinder_cache for five minutes, and prints it as JSON keyed by fuel type and then company.">
</p>

Fuelfinder is a small command-line tool that reads the current Danish fuel prices from [fuelfinder.dk](https://www.fuelfinder.dk) and prints them as JSON. Results are cached on disk for five minutes, so running it repeatedly does not hammer the site.

```sh
uvx fuelfinder
```

## Features

- **Scrape fuel prices**: reads the price table from `fuelfinder.dk/listprices.php`, one row per fuel company and one column per fuel type.
- **Rate limiting**: requests go to the server at most once every 5 minutes, using a persistent cache.
- **Command-line interface**: built with Typer.
- **Debug logging**: optional, using Python's built-in logging.
- **Pre-commit setup**: `ruff` keeps the code consistent.

## Installation

Run it without installing, using [uv](https://github.com/astral-sh/uv):

```sh
uvx fuelfinder
```

Or clone the repository and let uv manage the dependencies:

```sh
git clone https://github.com/jakob1379/fuelfinder.git
cd fuelfinder
uv sync
uv run fuelfinder
```

Fuelfinder requires Python 3.12 or newer.

## Usage

```sh
uvx fuelfinder --debug
```

The output is the price table as a JSON object keyed by column (fuel type) and then by row (fuel company).

| Option    | Description                                                                         |
| --------- | ----------------------------------------------------------------------------------- |
| `--url`   | The URL to fetch fuel prices from. Can also be set with the `FUELLIST_URL` environment variable. Defaults to `https://www.fuelfinder.dk/listprices.php`. |
| `--debug` | Enable debug logging for more detailed output.                                      |

## Configuration

Caching uses `diskcache` and is stored in `~/.cache/fuelfinder_cache`, so it persists across invocations. A cached result is reused for 5 minutes before the page is fetched again.

## Development

Contributions are welcome as pull requests. Please make sure your code passes all pre-commit hooks and document any new functionality.

This project uses [pre-commit](https://pre-commit.com/) with:

- **Ruff**: linting and formatting.
- **pre-commit-hooks**: trailing whitespace, end-of-file, and YAML/JSON checks.

```sh
pre-commit install          # set up the hooks
pre-commit run --all-files  # run them on every file
```

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

## Acknowledgments

- [fuelfinder.dk](https://www.fuelfinder.dk) for providing the data source.
- The authors of `ruff`, `typer`, `diskcache`, and the other open-source projects used in this tool.
