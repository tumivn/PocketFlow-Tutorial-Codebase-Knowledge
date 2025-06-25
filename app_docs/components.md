# Components Document for Codebase Knowledge Builder

## 1. Introduction

This document describes the key components of the Codebase Knowledge Builder application, their responsibilities, and how they interact with each other. The application is built using the PocketFlow framework, which provides a way to define and execute workflows.

## 2. Core Components

The Codebase Knowledge Builder consists of the following core components:

### 2.1 Main Module (main.py)

**Responsibility**: Serves as the entry point for the application, parsing command-line arguments and initializing the workflow.

**Key Functions**:
- Parse command-line arguments using `argparse`
- Initialize the shared dictionary with inputs
- Create and run the tutorial flow

**Interactions**:
- Calls the `create_tutorial_flow` function from flow.py
- Passes the shared dictionary to the flow for execution

### 2.2 Flow Module (flow.py)

**Responsibility**: Defines the workflow of the application using the PocketFlow framework.

**Key Functions**:
- Create and connect the nodes in the workflow
- Return the flow instance for execution

**Interactions**:
- Imports node classes from nodes.py
- Creates instances of the nodes
- Connects the nodes in sequence
- Returns the flow instance to main.py

### 2.3 Nodes Module (nodes.py)

**Responsibility**: Implements the processing logic for each step in the workflow.

**Key Classes**:
- `FetchRepo`: Fetches the repository or scans the local directory
- `IdentifyAbstractions`: Identifies key abstractions in the codebase
- `AnalyzeRelationships`: Analyzes relationships between the identified abstractions
- `OrderChapters`: Determines the order of chapters for the tutorial or SAD
- `WriteChapters`: Writes the content for each chapter
- `CombineTutorial`: Combines the chapters into a final output

**Interactions**:
- Each node interacts with the shared dictionary to read inputs and write outputs
- Nodes call utility functions from the utils package
- Nodes may call external services (e.g., LLMs) for processing

### 2.4 Utils Package (utils/)

**Responsibility**: Provides utility functions for the nodes.

**Key Modules**:
- `crawl_github_files.py`: Functions for crawling GitHub repositories
- `crawl_local_files.py`: Functions for crawling local directories
- `call_llm.py`: Functions for calling LLM services

**Interactions**:
- Called by the nodes to perform specific tasks
- May interact with external services (e.g., GitHub API, LLM services)

## 3. Node Components

### 3.1 FetchRepo

**Responsibility**: Fetch the repository or scan the local directory, collecting file contents.

**Methods**:
- `prep(shared)`: Prepares for execution by determining the source type (repo or directory) and setting up parameters.
- `exec(prep_res)`: Executes the fetching process, either by cloning and crawling a GitHub repository or by crawling a local directory.
- `post(shared, prep_res, exec_res)`: Updates the shared dictionary with the fetched files.

**Inputs**:
- Repository URL or local directory path
- Include/exclude file patterns
- Maximum file size

**Outputs**:
- List of files with their contents

### 3.2 IdentifyAbstractions

**Responsibility**: Analyze the codebase to identify key abstractions (components, modules, concepts).

**Methods**:
- `prep(shared)`: Prepares for execution by creating a context string from the file contents.
- `exec(prep_res)`: Calls the LLM to identify abstractions in the codebase and parses the response.
- `post(shared, prep_res, exec_res)`: Updates the shared dictionary with the identified abstractions.

**Inputs**:
- List of files with their contents

**Outputs**:
- List of abstractions (name, description)

### 3.3 AnalyzeRelationships

**Responsibility**: Determine relationships between the identified abstractions.

**Methods**:
- `prep(shared)`: Prepares for execution by creating a context string from the file contents and abstractions.
- `exec(prep_res)`: Calls the LLM to analyze relationships between abstractions and parses the response.
- `post(shared, prep_res, exec_res)`: Updates the shared dictionary with the relationships and summary.

**Inputs**:
- List of files with their contents
- List of abstractions

**Outputs**:
- Relationships between abstractions
- Project summary

### 3.4 OrderChapters

**Responsibility**: Determine the logical order for presenting the abstractions in the documentation.

**Methods**:
- `prep(shared)`: Prepares for execution by creating a context string from the abstractions and relationships.
- `exec(prep_res)`: Calls the LLM to determine the optimal order for presenting the abstractions and parses the response.
- `post(shared, prep_res, exec_res)`: Updates the shared dictionary with the chapter order.

**Inputs**:
- List of abstractions
- Relationships between abstractions

**Outputs**:
- Chapter order (list of indices into the abstractions list)

### 3.5 WriteChapters

**Responsibility**: Generate the content for each chapter, using the LLM to create explanations, examples, and diagrams.

**Methods**:
- `prep(shared)`: Prepares for execution by creating a list of items to process, one for each chapter.
- `exec(item)`: Calls the LLM to generate the content for a single chapter and returns the content.
- `post(shared, prep_res, exec_res_list)`: Updates the shared dictionary with the chapter contents.

**Inputs**:
- List of abstractions
- Relationships between abstractions
- Chapter order
- List of files with their contents

**Outputs**:
- Chapter contents

### 3.6 CombineTutorial

**Responsibility**: Combine the chapters into a final output, creating an index file and organizing the chapters in the specified output directory.

**Methods**:
- `prep(shared)`: Prepares for execution by gathering all the necessary data from the shared dictionary.
- `exec(prep_res)`: Creates the output directory, generates the index file, and writes each chapter to a separate file.
- `post(shared, prep_res, exec_res)`: Updates the shared dictionary with the output directory path.

**Inputs**:
- Chapter contents
- Chapter order
- List of abstractions
- Relationships between abstractions
- Project summary

**Outputs**:
- Output directory path with generated documentation files

## 4. Utility Components

### 4.1 crawl_github_files.py

**Responsibility**: Provide functions for crawling GitHub repositories.

**Key Functions**:
- `crawl_github_files(repo_url, token, include_patterns, exclude_patterns, max_file_size)`: Clones a GitHub repository and crawls it to collect file contents.

**Inputs**:
- Repository URL
- GitHub token
- Include/exclude patterns
- Maximum file size

**Outputs**:
- List of files with their contents

### 4.2 crawl_local_files.py

**Responsibility**: Provide functions for crawling local directories.

**Key Functions**:
- `crawl_local_files(directory, include_patterns, exclude_patterns, max_file_size)`: Crawls a local directory to collect file contents.

**Inputs**:
- Directory path
- Include/exclude patterns
- Maximum file size

**Outputs**:
- List of files with their contents

### 4.3 call_llm.py

**Responsibility**: Provide functions for calling LLM services.

**Key Functions**:
- `call_llm(prompt, temperature, max_tokens)`: Calls an LLM service with the given prompt and parameters.

**Inputs**:
- Prompt
- Temperature
- Maximum tokens

**Outputs**:
- LLM response

## 5. Component Interactions

The components interact with each other through the shared dictionary, which is passed between the nodes in the workflow. The shared dictionary contains both inputs (e.g., repository URL, include/exclude patterns) and outputs (e.g., files, abstractions, relationships, chapter order, chapter contents).

The flow of data through the components is as follows:

1. **Main Module** initializes the shared dictionary with inputs from command-line arguments.
2. **Flow Module** defines the workflow by connecting the nodes in sequence.
3. **FetchRepo** fetches the repository or scans the local directory, adding the files to the shared dictionary.
4. **IdentifyAbstractions** identifies key abstractions in the codebase, adding them to the shared dictionary.
5. **AnalyzeRelationships** analyzes relationships between the identified abstractions, adding them to the shared dictionary.
6. **OrderChapters** determines the order of chapters, adding it to the shared dictionary.
7. **WriteChapters** generates the content for each chapter, adding it to the shared dictionary.
8. **CombineTutorial** combines the chapters into a final output, adding the output directory path to the shared dictionary.
9. **Main Module** displays a completion message with the output directory path.

## 6. Component Dependencies

The components have the following dependencies:

- **Main Module** depends on the Flow Module.
- **Flow Module** depends on the Nodes Module.
- **Nodes Module** depends on the Utils Package.
- **Utils Package** may depend on external libraries and services.

The application follows a layered architecture, with the Main Module at the top, followed by the Flow Module, Nodes Module, and Utils Package. This layered approach allows for separation of concerns and modularity.

## 7. Component Configuration

The components are configured through command-line arguments and environment variables:

- Command-line arguments are parsed by the Main Module and passed to the nodes through the shared dictionary.
- Environment variables (e.g., GITHUB_TOKEN, OPENAI_API_KEY) are used by the Utils Package to authenticate with external services.

This configuration approach allows for flexibility in how the application is used and deployed.