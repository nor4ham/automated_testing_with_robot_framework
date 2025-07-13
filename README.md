# Automated Testing with Robot Framework

This repository contains everything you need to set up and run automated tests for both web and mobile applications using Robot Framework.

## Table of Contents

1. [Prerequisites](#prerequisites)
2. [Repository Structure](#repository-structure)
3. [Setup & Installation](#setup--installation)
4. [Web Testing](#web-testing)
5. [Mobile Testing](#mobile-testing)
6. [Running Tests](#running-tests)
7. [Results & Reports](#results--reports)
8. [CI Integration](#ci-integration)
9. [Contributing](#contributing)

---

## Prerequisites

- **Git** installed and configured
- **Python 3.8+**
- **Node.js & npm** (for Appium)
- **Docker** (optional, if using containers)
- **Browser drivers** (ChromeDriver, geckodriver) on your PATH for web tests
- **Android SDK & Emulator** or **iOS Simulator** for mobile tests

## Repository Structure

```text
my-tests/
├── .gitignore
├── README.md                # Project documentation (this file)
├── requirements.txt         # Python dependencies
├── docker-compose.yml       # (optional) for containerized setup
├── tests/                   # Robot Framework test cases
│   ├── web_login.robot
│   └── mobile_login.robot
├── resources/               # Shared keywords and variables
│   └── Common.resource
├── results/                 # Output reports and logs
└── .github/                 # CI workflows
    └── workflows/           # e.g., robot-ci.yml
```

## Setup & Installation

1. **Clone the repository**:

   ```bash
   git clone https://github.com/<your-org>/my-tests.git
   cd my-tests
   ```

2. **Create a virtual environment**:

   ```bash
   python3 -m venv rf-env
   source rf-env/bin/activate   # macOS/Linux
   .\rf-env\Scripts\activate  # Windows
   ```

3. **Install Python dependencies**:

   ```bash
   pip install -r requirements.txt
   ```

4. **Install Appium server**:

   ```bash
   npm install -g appium
   ```

5. **Ensure browser drivers** are on your PATH and mobile emulators/devices are available.

## Web Testing

We use **SeleniumLibrary** for browser automation.

- Test file: `tests/web_login.robot`
- Launch with Chrome by default; change browser in test or via CLI.

## Mobile Testing

We use **AppiumLibrary** for mobile automation.

- Test file: `tests/mobile_login.robot`
- Make sure Appium server is running:
  ```bash
  appium
  ```

## Running Tests

- **All tests**:

  ```bash
  robot --outputdir results tests
  ```

- **Only web tests**:

  ```bash
  robot -i web --outputdir results tests/web_login.robot
  ```

- **Only mobile tests**:

  ```bash
  robot -i mobile --outputdir results tests/mobile_login.robot
  ```

## Results & Reports

After running, view the HTML report:

```bash
open results/report.html  # macOS
xdg-open results/report.html  # Linux
```

- `log.html`, `report.html`, and `output.xml` are generated in `results/`.

## CI Integration

Add a workflow in `.github/workflows/robot-ci.yml` to install dependencies, start Appium, and run tests. Example:

```yaml
name: Robot Framework CI
on: [push, pull_request]
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Set up Python
        uses: actions/setup-python@v2
        with:
          python-version: '3.9'
      - name: Install dependencies
        run: |
          python -m venv rf-env
          source rf-env/bin/activate
          pip install -r requirements.txt
          npm install -g appium
      - name: Start Appium
        run: appium &
      - name: Run Robot tests
        run: robot --outputdir results tests
      - name: Upload Results
        uses: actions/upload-artifact@v2
        with:
          name: robot-results
          path: results
```

## Contributing

Contributions are welcome! Please:

1. Fork the repo
2. Create a feature branch
3. Submit a pull request

---

> *Documentation generated on* \${CURRENT\_DATE}

