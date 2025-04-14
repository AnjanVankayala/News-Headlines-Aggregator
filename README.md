# News-Headlines-Aggregator

A Python-based command-line tool and Jupyter/Colab notebook to fetch and aggregate the latest news headlines from multiple configurable online sources.

## Purpose

In today's fast-paced world, staying updated with the news often requires visiting multiple websites. This project aims to simplify that process by providing a single, consolidated view of recent headlines directly in your terminal or notebook output. It fetches headlines and their corresponding links from various news sources that you configure.

This tool is intended for:
* Users who want a quick, text-based overview of the latest news without browser clutter.
* Python learners interested in practicing web scraping, API interaction (implicitly via web requests), data extraction, and working with external configuration files.
* Anyone looking for a simple, customizable news aggregation solution.

## Features

* **Multi-Source Aggregation:** Fetches headlines from a list of news websites defined by the user.
* **External Configuration:** News sources, URLs, and crucial CSS selectors are managed in an external `config.json` file, making it easy to add, remove, or modify sources without changing the core Python code.
* **Source Selection:** Allows users to specify which configured sources they want to fetch headlines from during execution.
* **Keyword Filtering:** Option to filter the aggregated headlines to show only those containing specific keywords (case-insensitive).
* **Link Inclusion:** Displays the direct URL for each headline, allowing easy access to the full article.
* **Formatted Output:** Presents headlines clearly in the terminal/output, grouped by source and numbered sequentially.
* **Error Handling:** Includes basic error handling for common issues like network connection problems or website access errors (e.g., 4xx/5xx status codes).
* **Notebook & Colab Ready:** Provided as an `.ipynb` notebook file, runnable locally (with Jupyter) or directly via the included Google Colab link for easy setup and execution.

## Installation Instructions

**Prerequisites:**
* Python 3.x installed on your system.
* `pip` (Python package installer).

**Steps:**

1.  **Clone the Repository:**
    ```bash
    git clone https://github.com/AnjanVankayala/News-Headlines-Aggregator.git
    cd news-headlines-aggregator
    ```

2.  **Set up a Virtual Environment (Recommended):**
    ```bash
    python -m venv venv
    # Activate the virtual environment
    # On Windows:
    # venv\Scripts\activate
    # On macOS/Linux:
    # source venv/bin/activate
    ```

3.  **Install Dependencies:**
    This project relies on the `requests` and `beautifulsoup4` libraries. Install them using pip:
    ```bash
    pip install requests beautifulsoup4
    ```

## Usage Guidelines

**1. Configure News Sources (`config.json`)**

This is the **most crucial step** for the aggregator to work correctly.

* Before running the script/notebook for the first time, you need to create or verify the `config.json` file in the project's root directory.
* The notebook (`News_Headlines_Aggregator.ipynb`) includes a cell that defines sample configuration data (`config_data`) and writes it to `config.json`. **You must run this cell.**
* **IMPORTANT:** The CSS selectors (`selector` field in the JSON) are highly dependent on the target website's HTML structure and **will break** when websites update their design. You **must** manually inspect the HTML of each news source you want to use (using your browser's developer tools - Right Click -> Inspect Element on a headline) and update the `selector` value accordingly.

    **Example `config.json` Structure:**
    ```json
    {
        "source_id_1": {
            "url": "[https://www.news-website-1.com/news](https://www.news-website-1.com/news)",
            "base_url": "[https://www.news-website-1.com](https://www.news-website-1.com)",
            "selector": "h2.headline-class a" // <-- EXAMPLE: Replace with actual selector found via inspection
        },
        "source_id_2": {
            "url": "[https://news.another-site.org/latest](https://news.another-site.org/latest)",
            "base_url": "[https://news.another-site.org](https://news.another-site.org)",
            "selector": "article.news-item > a" // <-- EXAMPLE: Replace with actual selector
        }
        // Add more sources here
    }
    ```
    * `source_id`: A unique lowercase identifier (e.g., `bbc`, `times_of_india`).
    * `url`: The direct URL to the news page you want to scrape.
    * `base_url`: The main domain URL, used to construct full links from relative paths found in `href` attributes.
    * `selector`: The CSS selector that targets the HTML elements containing the headlines (often `<a>` tags or elements containing them like `<h2>`, `<h3>`, `<p>`).

**2. Running the Aggregator (Using the Notebook)**

* **IPython Notebook (`News_Headlines_Aggregator.ipynb`):**
    * Open the `.ipynb` file using Jupyter Notebook, JupyterLab, or VS Code with Python extensions.
    * Run the cells sequentially:
        1.  Install dependencies (`!pip install...`).
        2.  Define/update `config_data` and create `config.json`. **Remember to update selectors here first!**
        3.  Run the cells containing the Python function definitions (`load_config`, `scrape_source`, `display_headlines`, `main`).
        4.  Execute the `main()` function calls in the final cells to run the aggregator. Modify the parameters passed to `main()` as needed.

* **Google Colab:**
    * Click the "Open in Colab" badge/link provided in the repository (if available) or upload the `.ipynb` file to Google Colab.
    * Run the cells sequentially, similar to the local notebook instructions. You will need to run the configuration cell to create `config.json` within the Colab environment each time the runtime restarts. **Ensure selectors are up-to-date within the notebook cell before running.**

* **Calling the `main` function:**
    The `main` function in the notebook/script is designed to be called directly with parameters:
    ```python
    # Defined within the notebook or a .py file

    # Run with all sources from config.json, no filter
    main()

    # Run only with 'bbc' and 'ndtv', no filter
    main(sources_str='bbc,ndtv')

    # Run with all sources, filter for headlines containing 'technology'
    main(filter_keyword='technology')

    # Run only with 'times_of_india', filter for 'business'
    main(sources_str='times_of_india', filter_keyword='business')
    ```

## Key Components

* **`News_Aggregator.ipynb` (or `.py` script):** The core application file containing all Python code.
    * **`load_config()`:** Reads and parses the `config.json` file.
    * **`scrape_source()`:** Fetches HTML content from a single specified source URL, parses it using BeautifulSoup, extracts headlines and links based on the configured selector, and handles basic errors.
    * **`display_headlines()`:** Takes the aggregated headlines, applies the keyword filter (if any), and prints the formatted output to the console/notebook output.
    * **`main(sources_str, filter_keyword)`:** Orchestrates the workflow - loads config, determines which sources to scrape based on parameters, calls `scrape_source` for each, and then calls `display_headlines`.
* **`config.json`:** An external JSON file storing the list of news sources, their URLs, base URLs, and crucial CSS selectors. This allows configuration without modifying the Python code itself (though selectors need frequent updates).
* **`requests` (Library):** Used for making HTTP GET requests to download the HTML content of the news websites.
* **`BeautifulSoup4` (Library):** Used for parsing the downloaded HTML content and navigating the document structure to find elements using CSS selectors.

## Objectives

* **Functionality:** To create a working command-line/notebook tool that successfully aggregates news headlines from user-defined sources.
* **Learning:** To provide a practical project for learning and applying fundamental web scraping concepts in Python.
* **Configuration:** To demonstrate the use of external configuration files (JSON) for making applications more flexible.
* **Modularity:** To structure the code into logical functions for better readability and maintainability.
* **Usability:** To offer a simple, text-based alternative for quick news consumption.

---

**Disclaimer:** Web scraping effectiveness depends entirely on the target website's structure. Selectors in `config.json` **will** require regular updates as websites change. Please use this tool responsibly and respect the terms of service and `robots.txt` files of the websites you are scraping. Avoid making excessive requests.
