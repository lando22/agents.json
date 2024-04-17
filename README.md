# agents.json - Creating the web framework for the future of autonomous agents

## Overview
`agents.json` is a structured configuration file designed to facilitate the future framework for autonomous AI agents in navigating and interacting with specific web interfaces. It acts as a guide, providing detailed instructions for agents on interacting with different elements of a web page, enabling them to perform tasks autonomously.

## Purpose
Looking ahead, AI models are slowly showing agentic behaviour, and more work is being put into making semi-autonomous systems/fully autonomous systems. However, the current state of websites and web platforms are built for humans to interact with and not AI agents. Lots of research has been put into UI understanding with multi-modal models, but such methods can be brittle and expensive.

The purpose of `agents.json` is to provide a clear and standardized description of UI interactions on a website that autonomous agents can follow. This file is crucial for enabling AI-driven models to understand and execute web-based tasks effectively, such as searching, navigating, or processing transactions on behalf of a user. You can think of `agents.json` as similar to how `robots.txt` instructs search engines to crawl your site; `agents.json` instructs autonomous agents how to use your site!

## How It Works
The `agents.json` file outlines the interactions available on various website pages, specifying how to interact with important UI elements. This standardization helps in automating tasks that typically require human interaction.

## File Structure
The structure of `agents.json` is defined in JSON format, making it easy to read both by machines and humans. Here is an example of how the `agents.json` might be structured for a hypothetical bookstore website:

```json
{
  "apiVersion": "1.0",
  "baseUrl": "https://www.fake-bookstore.com", 
  "pages": {
    "/search": {
      "uiInteractions": {
        "searchInput": {
          "selector": "#search-box",
          "description": "Input box for search queries",
          "agent_instructions": "Enter the author's name or book title here to search for books."
        },
        "searchButton": {
          "selector": "#search-button",
          "description": "Button to submit the search query",
          "agent_instructions": "Click this button to search for the books by the entered query."
        }
      }
    },
    "/results": {
      "uiInteractions": {
        "selectBook": {
          "selector": ".book-item:first-child",
          "description": "Selects the first book in the search results",
          "agent_instructions": "Select the first book from the list that matches the search criteria."
        }
      }
    },
    "/book-detail": {
      "uiInteractions": {
        "addToCart": {
          "selector": "#add-to-cart",
          "description": "Adds the selected book to the shopping cart",
          "agent_instructions": "Use this button to add the book to the shopping cart."
        }
      }
    },
    "/cart": {
      "uiInteractions": {
        "checkout": {
          "selector": "#checkout-button",
          "description": "Proceeds to checkout",
          "agent_instructions": "Proceed to checkout after reviewing the cart."
        }
      }
    }
  }
}
```

## Key Components

- **apiVersion**: Specifies the version of the `agents.json` schema used.
- **baseUrl**: The base URL of the site where the interactions are defined.
- **pages**: A dictionary of page paths with detailed interaction specifications for each page.
- **uiInteractions**: Specifies the actionable elements within each page, including selectors, descriptions, and instructions tailored for AI agents. These should be things that an agent could interact with.

## Usage Scenario
Imagine an general purpose autonomous AI agent designed to assist users with various tasks. Say a user would like to purchase a book using their AI agent from the site `fake-bookstore.com` By accessing "fake bookstores" `agents.json` file, the agent can autonomously navigate the bookstore website, search for books based on user preferences, add books to the cart, and handle the checkout process. This allows the user to let it's AI agent to carry out the task and the bookstore business to easily prepare it's website for the future of AI agents by adding just one file to it's site.

f
## Demo with a sample agent
Building an AI agent is ultimately up to the creator and this is not a one-size-fits-all as `agents.json` can be used in lots ways with different models, web automation frameworks etc.

For this demo, we will be using:

1. GPT.3.5-Turbo-0125 on the OpenAI API for our model. This will be used to read our agents.json file and generate a step by step action plan based on a user command.
2. Selenium to operate our sample website
3. Our sample agents.json file for our website to make it accessible to agents.

## Step 1: Load the sample website
You can load the sample website with Replit here. Please note, this is not the prettiest website or very complex, it is just meant to give users an idea of how a site with agents.json could work.

## Step 2: Create our AI agent to interact with the demo site
Next, we will use the OpenAI API to define our agent to make a plan based on the users request. Our user request will ask our agent to search for a book, select it and add it to a cart.

Here is the OpenAI API code in Python to create a step by step plan:


```python
import requests
from openai import OpenAI
client = OpenAI()

# define our system prompt. Please note: this is a sample agent, developers can use different models or a different system prompt
system_prompt = """You are an autonomous AI agent tasked with interpreting user requests and using a provided `agents.json` file to plan and execute web interactions on an electric bike shop website. Based on the user's query and the defined interactions in `agents.json`, generate a JSON object detailing the steps that should be taken on the website to complete the task.

User Query: "Order the best-rated electric bike."

agents.json content:
{
  "apiVersion": "1.0",
  "baseUrl": "https://www.electricbikeshop.com",
  "pages": {
    "/products": {
      "uiInteractions": {
        "sortOptions": {
          "selector": "#sort-by",
          "description": "Dropdown to sort products, e.g., by rating",
          "agent_instructions": "Sort the products by rating to find the best-rated bikes."
        },
        "selectProduct": {
          "selector": ".product-item:first-child",
          "description": "Selects the highest rated product",
          "agent_instructions": "Click on the first product after sorting by highest rating."
        }
      }
    },
    "/product-detail": {
      "uiInteractions": {
        "addToCart": {
          "selector": "#add-to-cart",
          "description": "Adds the selected bike to the shopping cart",
          "agent_instructions": "Use this button to add the chosen electric bike to the cart."
        }
      }
    },
    "/cart": {
      "uiInteractions": {
        "checkout": {
          "selector": "#checkout-button",
          "description": "Proceeds to checkout",
          "agent_instructions": "Proceed to checkout after reviewing the cart."
        }
      }
    }
  }
}

Expected JSON output format:

{
  "task": "Order the best-rated electric bike",
  "steps": [
    {
      "action": "navigate",
      "url": "https://www.electricbikeshop.com/products",
      "description": "Navigate to the products page to view all bikes."
    },
    {
      "action": "select",
      "selector": "#sort-by",
      "value": "Highest Rating",
      "description": "Sort the bike listings by highest rating to find the best-rated bikes."
    },
    {
      "action": "click",
      "selector": ".product-item:first-child",
      "description": "Select the first bike in the list, which is the highest-rated based on the sort."
    },
    {
      "action": "click",
      "selector": "#add-to-cart",
      "description": "Add the selected highest-rated bike to the shopping cart."
    },
    {
      "action": "navigate",
      "url": "https://www.electricbikeshop.com/cart",
      "description": "Go to the cart page to review items before checkout."
    },
    {
      "action": "click",
      "selector": "#checkout-button",
      "description": "Click on the checkout button to proceed with the purchase."
    }
  ]
}

Generate a JSON plan detailing the sequence of actions that an automation script should execute to fulfill the user's request based on the agents.json file. The plan should include detailed steps such as navigating to URLs, clicking on specific selectors, entering text, and any necessary waits or checks. Ensure the output is structured and clear for automation."""


# download the agent.json file from our sample website
# TODO: replace with your url from replit or other source with agents.json file
agents_file = requests.get("https://www.<url>/agent.json")

# download agents.json
with open("agents.json", "w") as f:
  f.write(agents_file.json())


# use OpenAI agent to create a plan to carry out the user request and read the sites agents.json, which inlcudes instructions for how to navigate the site
response = client.chat.completions.create(
  model="gpt-3.5-turbo-0125",
  messages=[
    {
      "role": "system",
      "content": system_prompt
    },
    {
      "role": "user",
      "content": "User query: Find and purchase the latest thriller novel by Author X from the Book World website.\n\nagents.json content:\n\n" + json.dumps((open("agents.json", "r")).read(), indent=4) # sample query with our sites agents.json
    }
  ],
  temperature=0.35,
  max_tokens=1024,
  top_p=1,
  frequency_penalty=0,
  presence_penalty=0,
  response_format={"type": "json_object" }
)

# get the plan and write the actions json file, which contains a step-by-step plan from our AI agent
with open("actions.json", "w") as actions_file:
  actions_file.write(response.choices[0].message.content)
```

Which will output something along the lines of:

```json
{
    "baseUrl": "https://<your url>",
    "actions": [
      {
        "description": "Navigate to the search page",
        "action": "navigate",
        "url": "https://<your url>.replit.dev"
      },
      {
        "description": "Enter 'Shakespeare' into the search box",
        "action": "enter_text",
        "selector": "#search-box",
        "text": "Author X Thriller Novel"
      },
      {
        "description": "Click the search button",
        "action": "click",
        "selector": "#search-button"
      },
      {
        "description": "Click on the first book item from the search results",
        "action": "click",
        "selector": ".book-item:first-child a"
      },
      {
        "description": "Add the book to the cart",
        "action": "click",
        "selector": "#add-to-cart"
      },
      {
        "description": "Go to the cart page",
        "action": "navigate",
        "url": "https://your-url/cart"
      },
      {
        "description": "Proceed to checkout",
        "action": "click",
        "selector": "#checkout-button"
      }
    ]
  }
```

## Step 3: Carry out site interaction with Selenium
To carry out the task fully autonomously, you can use the following script:

```python
from selenium import webdriver
from selenium.webdriver.chrome.service import Service
from selenium.webdriver.common.by import By
from selenium.webdriver.common.keys import Keys
import json
import time

# Load the JSON file with action definitions
with open('actions.json', 'r') as file:
    agents = json.load(file)

# Set up Chrome options
options = webdriver.ChromeOptions()
# Specify the path to ChromeDriver using Service
s = Service('/path/to/chromedriver')  # Replace '/path/to/chromedriver' with the actual path if not in PATH

driver = webdriver.Chrome(service=s, options=options)

# Function to perform actions
def perform_action(action):
    if action['action'] == 'navigate':
        driver.get(action['url'])
    elif action['action'] == 'click':
        driver.find_element(By.CSS_SELECTOR, action['selector']).click()
    elif action['action'] == 'enter_text':
        element = driver.find_element(By.CSS_SELECTOR, action['selector'])
        element.clear()
        element.send_keys(action['text'])

# Execute each action from the JSON
try:
    for action in agents['actions']:
        # slow down the interactions on purpose so you can view the actions
        time.sleep(3)
        perform_action(action)
finally:
    # Output the result or finalize the test
    print(driver.page_source)  # You can modify this to suit what output you need, such as checking for specific elements.
    driver.quit()
```

If the plan generated by our agent was correct, this should get us to the checkout page. If the model is not outputting good steps, here is a verified set of steps for our sample site:

```json
{
  "baseUrl": "https://<url>",
  "actions": [
    {
      "description": "Navigate to the search page",
      "action": "navigate",
      "url": "https://<url>"
    },
    {
      "description": "Enter 'Shakespeare' into the search box",
      "action": "enter_text",
      "selector": "#search-box",
      "text": "Author X Thriller Novel"
    },
    {
      "description": "Click the search button",
      "action": "click",
      "selector": "#search-button"
    },
    {
      "description": "Click on the first book item from the search results",
      "action": "click",
      "selector": ".book-item:first-child a"
    },
    {
      "description": "Add the book to the cart",
      "action": "click",
      "selector": "#add-to-cart"
    },
    {
      "description": "Go to the cart page",
      "action": "navigate",
      "url": "<url>/cart"
    },
    {
      "description": "Proceed to checkout",
      "action": "click",
      "selector": "#checkout-button"
    }
  ]
}
```

**Please remember**: The actual agent we built may be faulty at times or not robust, and our sample site is incredibly basic. Remember that building the actual agent is not the purpose of `agents.json` nor is the website, the purpose was the show that an agents.json file could serve as a viable way to get models to interact with web UIs.

## Future Scope
While `agents.json` is initially designed for web interactions, its framework could be extended to other platforms and applications, such as mobile apps or even desktop software, wherever standardized interaction instructions can enhance automation. This framework could be extended to backend databases and APIs.

## Why use UI elements? Why not just use backend APIs?
Overall, plugging directly into backend APIs would be ideal and likely easier. Eventually, this would be a second addition to this framework. However, most websites today do not offer public-facing APIs, and even more website creators do not have the resources to build a fully managed API for agents.

`agents.json` works with websites in their current form today with minimal edits. This makes the framework easier to adopt and to understand.

## Conclusion
`agents.json` represents an innovative approach to enabling sophisticated interactions by AI agents across web platforms. It bridges human-like understanding and machine execution, offering a scalable solution for automating complex web navigation and operations.
