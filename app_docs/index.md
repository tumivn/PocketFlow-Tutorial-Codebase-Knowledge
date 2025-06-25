# Codebase Knowledge Builder Documentation

## Introduction

This documentation provides comprehensive information about the Codebase Knowledge Builder application, which analyzes codebases and generates documentation in the form of tutorials or Software Architecture Documents (SAD).

## Table of Contents

1. [Requirements](requirements.md) - Functional and non-functional requirements of the application
2. [Architecture](architecture.md) - High-level architecture and design of the application
3. [Flow](flow.md) - Detailed processing flow of the application
4. [Components](components.md) - Key components and their responsibilities
5. [Usage](usage.md) - How to install and use the application

## Overview

The Codebase Knowledge Builder is a command-line application that uses Large Language Models (LLMs) to analyze codebases and generate comprehensive documentation. The application follows a pipeline architecture, where each stage of processing is handled by a specialized component.

The application can generate two types of documentation:
- **Tutorials**: Beginner-friendly explanations of the codebase with examples and diagrams
- **Software Architecture Documents (SAD)**: Formal architectural documentation following standard practices

The application supports multiple languages for the generated documentation and can process both GitHub repositories and local directories.

## Key Features

- **Codebase Analysis**: Identifies key abstractions and their relationships in the codebase
- **Documentation Generation**: Generates comprehensive documentation with explanations, examples, and diagrams
- **Multiple Document Types**: Supports both tutorial-style documentation and formal Software Architecture Documents
- **Multi-language Support**: Generates documentation in multiple languages
- **Visualization**: Includes diagrams (using Mermaid syntax) to visualize relationships between components
- **Customization**: Supports filtering files based on include/exclude patterns and maximum file size

## Getting Started

To get started with the Codebase Knowledge Builder, see the [Usage Guide](usage.md) for installation instructions and usage examples.

## Development

For developers who want to understand or extend the application, the following documents provide detailed information:
- [Architecture Document](architecture.md) - High-level architecture and design
- [Flow Document](flow.md) - Detailed processing flow
- [Components Document](components.md) - Key components and their responsibilities

## Support

If you encounter any issues or have questions about using the Codebase Knowledge Builder, please open an issue on the GitHub repository:

https://github.com/The-Pocket/Tutorial-Codebase-Knowledge/issues