# Links checker action
Action for correctness check of links in courses

## Usage example
Add this action to the course repository:
```yaml
name: LinksChecker

on:
  # Triggers the workflow on push or pull request events but only for the "master" branch
  push:
    branches: [ "main" ]
  pull_request:
    branches: [ "main" ]

  # Allows you to run this workflow manually from the Actions tab
  workflow_dispatch:

jobs:
  links-checker-job:
    runs-on: ubuntu-latest
    steps:
      - name: Check out repository code
        uses: actions/checkout@v4

      - name: Call action jetbrains-academy/links-checker-action
        uses: jetbrains-academy/links-checker-action@main
        with:
          dir: ./
```

## Exceptions

Some sites block automated requests (for example, due to bot protection) and may appear as broken links. The checker supports exception rules so such cases are treated as warnings instead of errors.

- The action loads rules from `/LinksChecker/exceptions.yaml`.
- Rule format:
  - `domain`: **substring** to match against the URL host
  - `status_codes`: list of HTTP codes that should not fail the check
  - `message` (optional): note shown in the warning

Example:
```yaml
exceptions:
  - domain: "npmjs.com"
    status_codes: [403]
    message: "npmjs.com has bot protection"
```

Behavior:
- If a link matches a rule and returns one of the listed codes, the result is a WARNING with the rule message. **The check will then be considered successful.**
- Other 4xx/5xx responses are reported as ERRORs.
