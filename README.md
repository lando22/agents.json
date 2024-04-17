# agents.json - Creating the web framework for the future of autonomous agents

## Overview
`agents.json` is a structured configuration file designed to facilitate the future framework for autonomous AI agents in navigating and interacting with specific web interfaces. It acts as a guide, providing detailed instructions for agents on how to interact with different elements of a web page, enabling them to perform tasks autonomously.

## Purpose
Looking ahead, AI models are slowly showing agentic behaviour, and more work is being put into making semi-autonomous systems/fully autonomous systems. However, the current state of websites and web platforms are built for humans to interact with and not AI agents. Lots of research has been put into UI understanding with multi-modal models, but such methods can be brittle and expensive.

The purpose of `agents.json` is to provide a clear and standardized description of UI interactions on a website that autonomous agents can follow. This file is crucial for enabling AI-driven models to understand and execute web-based tasks effectively, such as searching, navigating, or processing transactions on behalf of a user. You can think of `agents.json` similar to how `robots.txt` instructs search engines to crawl your site; `agents.json` instructs autonomous agents how to use your site!

## How It Works
The `agents.json` file outlines the interactions available on various pages of a website, specifying how to interact with important UI elements. This standardization helps in automating tasks that typically require human interaction.

## File Structure
The structure of `agents.json` is defined in JSON format, making it easy to read both by machines and humans. Here is an example of how the `agents.json` might be structured for a hypothetical book store website:

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

## Future Scope
While `agents.json` is initially designed for web interactions, its framework could be extended to other platforms and applications, such as mobile apps or even desktop software, wherever standardized interaction instructions can enhance automation. This framework could be extended to backend databases and APIs.

## Why use UI elements? Why not just use backend APIs?
Overall, plugging directly into backend APIs would be ideal and likely easier. Eventually, this would be a second addition to this framework. However, most websites today do not offer public facing APIs, even more website creators do not have the resources to build a fully managed API for agents.

`agents.json` works with websites in their current form today with minimal edits. This makes the framework easier to adopt and to understand.

## Conclusion
`agents.json` represents an innovative approach to enabling sophisticated interactions by AI agents across web platforms. It serves as a bridge between human-like understanding and machine execution, offering a scalable solution for automating complex web navigation and operations.
