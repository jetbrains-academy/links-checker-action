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
