# Example Repo to Run GHA locally

This repo demonstrates how to set up and run GitHub Actions locally using Act. It includes a simple Node.js project with a basic test, and a GitHub Actions workflow to run the test.

## Prerequisites

- **Docker**: [Install Docker](https://docs.docker.com/get-docker/)
- **Act**: [Install Act](https://github.com/nektos/act)

### Installing Act

**Using Homebrew (macOS/Linux):**

```sh
brew install act
```

**Using Chocolatey (Windows):**

```sh
choco install act-cli
```

**Direct Download**: Visit the [Act releases page](https://github.com/nektos/act/releases) and download the binary for your OS.

## Setup

### Step 1: Clone the Repository

```sh
git clone https://github.com/rukywe/running-gha-locally.git
cd running-gha-locally
```

### Step 2: Install Dependencies

```sh
npm install
```

### Step 3: Run Tests Locally

```sh
npm test
```

### Step 4: Run GitHub Actions Locally

```sh
act
```

For Apple M-series chips:

```sh
act --container-architecture linux/amd64
```

## Troubleshooting

### Docker Login and Manual Image Pull

**Login to Docker:**

```sh
docker login
```

**Manually Pull the Docker Image:**

```sh
docker pull ghcr.io/catthehacker/ubuntu:act-latest
```

### Running Specific Jobs or Viewing the Execution Graph

**Run Specific Jobs:**

```sh
act -j build
```

**View Execution Graph:**

```sh
act -l
```

## Project Structure

- **sum.js**: A module that exports a function to add two numbers.
- **sum.test.js**: A test file for `sum.js` using Jest.
- **.github/workflows/ci.yml**: GitHub Actions workflow file to run the tests.

## Example Files

**`sum.js`:**

```js
function sum(a, b) {
  return a + b;
}
module.exports = sum;
```

**`sum.test.js`:**

```js
const sum = require('./sum');

test('adds 1 + 2 to equal 3', () => {
  expect(sum(1, 2)).toBe(3);
});
```

**`package.json`:**

```json
{
  "name": "running-gha-locally",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "jest"
  },
  "dependencies": {},
  "devDependencies": {
    "jest": "^27.0.0"
  }
}
```

**`.github/workflows/ci.yml`:**

```yaml
name: CI

on: [push, pull_request]

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v2
      - name: Set up Node.js
        uses: actions/setup-node@v2
        with:
          node-version: '20'
      - run: npm install
      - run: npm test
```
