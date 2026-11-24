# Email Finder

Command-line tool that collects publicly listed email addresses for a company domain from web search results. It runs a set of targeted queries through a self-hosted SearXNG instance, extracts addresses with a regular expression, and filters out unrelated or generic-provider addresses.

## Problem

Public contact addresses for a company are spread across search snippets. This tool gathers them into one deduplicated list, so the results can be reviewed in one place.

## How it works

1. Takes a company name or domain, plus an optional contact name. If the input is a domain or URL it is sanitized to a bare domain.
2. Builds queries from templates that depend on what is known: company name only, domain only, domain plus contact name. With a contact name and a domain it also searches for common address patterns such as `first.last@domain`.
3. Runs each query through SearXNG and concatenates the titles and snippets.
4. Extracts addresses with a regex and lowercases them.
5. Filters: removes addresses on a blacklist of common providers and email-tool sites, and, when a domain is known, keeps only addresses on that domain.
6. Outputs a sorted, unique list as JSON (with the query details) or as plain text.

The tool does not contact mail servers and does not verify that an address exists. Pattern queries only show which guesses appear in public search results.

## Stack

- Python 3, `requests`
- SearXNG for search (self-hosted)

## Usage

```bash
pip install -r requirements.txt

python standalone_lead_email_finder.py example.com \
  --searx-url http://localhost:8080 \
  --ceo-name "Jane Doe" \
  --output-file emails.json
```

Options: `-s/--searx-url` (required), `-c/--ceo-name`, `-o/--output-file`, `-f/--output-format {json,txt}`, `-l/--log-level`.

## Limits

- Only finds addresses that already appear in search results.
- One company per run.

## Responsible use

Use the output only in ways that respect the privacy and anti-spam laws that apply to you, and the terms of your search provider. Do not use it for unsolicited bulk mailing.

## License

MIT. See `LICENSE`.
