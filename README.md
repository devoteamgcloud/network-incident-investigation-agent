# Network Incident Investigation Agent

<h2 align="center"><i>AI Agent System for Network Incident Resolution and Analysis</i></h2><br>

<p align="center">
  <a href="#" target="_blank">
    <img src="https://img.shields.io/badge/Python-3.12+-blue" alt="Python Version">
  </a>
  <a href="#" target="_blank">
    <img src="https://img.shields.io/badge/Google%20ADK-1.5.0+-green" alt="Google ADK Version">
  </a>
  <a href="#" target="_blank">
    <img src="https://img.shields.io/badge/BigQuery-Integration-orange" alt="BigQuery Integration">
  </a>
  <a href="#" target="_blank">
    <img src="https://img.shields.io/badge/Jira-Integration-yellow" alt="Jira Integration">
  </a>
  <a href="#" target="_blank">
    <img src="https://img.shields.io/badge/Poetry-Dependency%20Management-purple" alt="Poetry">
  </a>
  <a href="#" target="_blank">
    <img src="https://img.shields.io/badge/MCP-Servers-red" alt="MCP Servers">
  </a>
</p>

<!-- TOC -->
* [Project Overview](#project-overview)
* [Problem Statement](#problem-statement)
* [Current Scope](#current-scope)
    * [Long-Term Vision and Objectives](#long-term-vision-and-objectives)
* [System Architecture and Implementation](#system-architecture-and-implementation)
* [Environment Setup and Usage](#environment-setup-and-usage)
    * [Prerequisites](#prerequisites)
    * [Installation](#installation)
    * [Configuration](#configuration)
    * [Running the Application](#running-the-application)
    * [Available Tasks](#available-tasks)
* [Project Structure](#project-structure)
* [Architecture & Components](#architecture--components)
* [Supported Workflows](#supported-workflows)
* [Agents Details](#agents-details)
    * [Root Agent](#root-agent)
    * [Subcategory Agent](#subcategory-agent)
    * [Deviation Agent](#deviation-agent)
* [Agent Orchestration Architecture](#agent-orchestration-architecture)
* [Development Guide](#development-guide)
    * [Adding New Agents](#adding-new-agents)
    * [Configuration Management](#configuration-management)
* [Troubleshooting](#troubleshooting)
* [Contributing](#contributing)

<!-- /TOC -->

## Project Overview

The **Network Incident Investigation Agent** is an advanced AI-powered multi-agent system designed to revolutionize network incident management and resolution. Built using Google's Agent Development Kit (ADK), this system automates the analysis, classification, and resolution of network-related incidents, significantly reducing manual effort and improving response times.

The system leverages a sophisticated multi-agent architecture that coordinates specialized agents to handle different aspects of incident management, from initial categorization to root cause analysis and deviation detection.

## Problem Statement

Network incident management in telecommunications faces numerous challenges:

* **Manual Analysis Overhead:** Time-consuming manual investigation of network incidents
* **Inconsistent Categorization:** Lack of standardized incident classification leading to routing delays
* **Data Fragmentation:** Critical information scattered across multiple systems (BigQuery, Jira, monitoring tools)
* **Reactive Approach:** Limited proactive analysis for pattern detection and trend identification
* **Knowledge Silos:** Expert knowledge not systematically captured or shared
* **Resolution Time Variability:** Inconsistent resolution times due to manual processes

## Current Scope

The system currently focuses on:

* **Automated Incident Classification:** Intelligent categorization of network incidents based on description and context
* **Multi-Source Data Integration:** Seamless integration with BigQuery for historical data analysis
* **Deviation Analysis:** Statistical analysis of incident patterns to identify anomalies and trends
* **Root Cause Investigation:** Automated investigation workflows that gather relevant information from multiple data sources
* **Structured Reporting:** Generation of comprehensive incident analysis reports

### Long-Term Vision and Objectives

The strategic roadmap includes:

* **Predictive Analytics:** Machine learning models to predict potential network issues before they occur
* **Automated Resolution:** Implementation of automated remediation for common incident types
* **Real-Time Monitoring Integration:** Direct integration with network monitoring systems for immediate incident detection
* **Knowledge Management:** Automated creation and maintenance of a knowledge base for common issues
* **Performance Optimization:** Continuous learning from resolution patterns to optimize agent performance
* **Cross-System Orchestration:** Seamless coordination across all network management tools and platforms

## System Architecture and Implementation

This project represents a production-ready evolution from proof-of-concept to a robust, scalable multi-agent system. The architecture emphasizes:

* **Modularity:** Each agent handles specific responsibilities with clear interfaces
* **Scalability:** Designed to handle increasing incident volumes and complexity
* **Extensibility:** Easy addition of new agents and integration points
* **Reliability:** Built-in error handling and fallback mechanisms
* **Observability:** Comprehensive logging and monitoring capabilities

## Environment Setup and Usage

### Prerequisites

* **Python:** 3.12 or higher
* **Poetry:** For dependency management
* **Google Cloud Project:** With appropriate permissions for Vertex AI and BigQuery

### Installation

1. **Clone the Repository:**
   ```bash
   git clone <repository-url>
   cd adk-network-issues-agent
   ```

2. **Install Dependencies:**
   ```bash
   # For the main network incident agent
   cd network_incident_agent
   poetry install
   ```

### Configuration

1. **Copy Environment Template:**
   ```bash
   cd network_incident_agent
   cp .env.example .env
   ```

2. **Configure Environment Variables:**
   Edit `.env` file with your specific values:
   ```bash
   # Vertex AI Configuration
   GOOGLE_GENAI_USE_VERTEXAI=1
   GOOGLE_CLOUD_PROJECT=your-project-id
   GOOGLE_CLOUD_LOCATION=your-location

   # BigQuery Configuration
   BQ_COMPUTE_PROJECT_ID=your-bq-project
   BQ_DATA_PROJECT_ID=your-data-project
   DATASET_ID=your_dataset
   TABLE_ID=your_table
   
   # Agent Models Configuration
   ROOT_AGENT_MODEL=gemini-2.5-flash
   ROOT_AGENT_TEMP=0.01

   # Gemini Configuration
   GEMINI_MODEL=gemini-2.5-flash
   GEMINI_TEMP=0.01
   GEMINI_TOP_P=0.95
   GEMINI_MAX_TOKENS=200
   THINKING_BUDGET=0

   # Ranking Configuration
   RERANK_LOCATION=global
   RERANK_MODEL=semantic-ranker-default@latest
   RERANK_CONFIG=default_ranking_config
   RERANK_BATCH=1000
   RERANK_RETRIEVE_TOP_N=10 

   # Final output Configuration
   OUTPUT_N_TICKETS=10
   
   ```

3. **Authenticate with Google Cloud:**
   ```bash
   gcloud auth application-default login
   gcloud config set project your-project-id
   ```

### Running the Application

The project includes several predefined tasks for easy execution:

#### Available Tasks

You can run these tasks using VS Code's task runner or directly via command line:

* **Install Dependencies:**
  ```bash
  cd network_incident_agent
  poetry install
  ```

* **Start Web Interface:**
  ```bash
  poetry run adk web
  ```

* **Run Command Line Interface:**
  ```bash
  poetry run adk run network_incident_agent
  ```

## Project Structure

```
adk-network-issues-agent/
├── network_incident_agent/                # Main agent application
│   ├── network_incident_agent/
│   │   ├── __init__.py                    # Package initialization
│   │   ├── config.py                      # Configuration management
│   │   ├── prompts.py                     # Agent prompts and instructions
│   │   ├── root_agent.py                  # Main orchestrating agent
│   │   ├── sub_agents/                    # Specialized sub-agents
│   │   │   ├── all_subcategories_agent/   # Incident categorization
│   │   │   │   ├── agent.py
│   │   │   │   ├── callback.py
│   │   │   │   ├── prompt.py
│   │   │   │   ├── utils.py
│   │   │   │   └── tools.py
│   │   │   │── filter_agent/              # Sequential Workflow 
│   │   │   │   ├── agent.py
│   │   │   │   └── prompt.py
│   │   │   │── rerank_tickets_agent/      # Relevant Incident Investigation
│   │   │   │   ├── agent.py
│   │   │   │   ├── prompt.py
│   │   │   │   ├── callback.py
│   │   │   │   ├── utils.py
│   │   │   │   └── tools.py
│   │   │   │── subcategory_agent/         # Incident categorization
│   │   │   │   ├── agent.py
│   │   │   │   ├── prompt.py
│   │   │   │   └── tools.py
│   │   │   └── deviation_agent/           # Pattern and deviation analysis
│   │   │       ├── agent.py
│   │   │       ├── callback.py
│   │   │       ├── prompt.py
│   │   │       ├── utils.py
│   │   │       └── tools.py
│   │   └── utils/                         # Common utility functions
│   │       └── utils.py
│   ├── deployment/                        # Deployment to Agent Engine
│   │   ├── __init__.py                    
│   │   ├── deploy.py                      
│   │   └── test_deployment.py            
│   ├── pyproject.toml                     # Poetry configuration
│   ├── .env.example                       # Environment template
│   └── README.md                          # Module-specific documentation. This file
├── .vscode/                               # VS Code configuration
│   └── tasks.json                         # Predefined tasks
└── get_started_with_memory_bank.ipynb     # Getting started guide
```

## Architecture & Components

### Core Components

* **Root Agent:** Central orchestrator that manages the overall incident resolution workflow
* **Sub-Agents:** Specialized agents for specific tasks (categorization, deviation analysis, retrieve and ranking)
* **Tools:** Functions used across agents
* **Utils:** Shared utilities 
* **Configuration System:** Centralized configuration management with environment-specific settings

### Data Flow

1. **Incident Input:** User provides incident details or ticket ID
2. **Classification:** All Subcategories agent and Subcategory agent analyze and categorize the incident
3. **Data Gathering:** Relevant data is collected from BigQuery and other sources
4. **Analysis:** Deviation agent performs statistical and pattern analysis
5. **Resolution:** Root agent coordinates findings and generates recommendations with the help of Rerank tickets agent.
6. **Reporting:** Structured output is provided to the user and logged to external systems

## Supported Workflows

* **Incident Triage:** Automatic classification and priority assignment
* **Historical Analysis:** Pattern recognition from historical incident data
* **Real-time Investigation:** Dynamic data gathering and analysis
* **Deviation Detection:** Statistical analysis to identify unusual patterns
* **Knowledge Extraction:** Automated extraction of insights from incident data
* **Report Generation:** Comprehensive incident analysis reports

## Agents Details

### Root Agent

The Root Agent serves as the central coordinator of the multi-agent system:

* **Responsibilities:**
  - Orchestrate workflow between sub-agents
  - Manage conversation state and context
  - Generate final analysis and recommendations
  - Handle error scenarios and fallback logic

* **Configuration:**
  - Model: Configurable via `ROOT_AGENT_MODEL` (default: gemini-2.5-flash)
  - Temperature: Controlled via `ROOT_AGENT_TEMP` (default: 0.01)
  - Thinking Budget: Set to 0 for production efficiency

### All Subcategories Agent

Specializes in retrieving all possible subcategories in a time range:

* **Responsibilities:**
  - Analyze user query to design filters to search for subcategories in database
  - Extract relevant subcategory information
  - Route incidents to appropriate handling workflows

* **Tools:**
  - `fetch_all_subcategories`: Retrieve all distinct subcategories from database based on user query filters 
  - User query analysis and subcategories retrieval

### Subcategory Agent

Specializes in incident classification and categorization:

* **Responsibilities:**
  - Analyze incident descriptions and metadata
  - Classify incidents into predefined categories
  - Extract relevant subcategory information
  - Route incidents to appropriate handling workflows

* **Tools:**
  - `get_subcategories_from_state`: Retrieve valid subcategory options from state
  - `set_top5_subcategories`: Order subcategories based on relevance with incident description and suggest top 5 subcategories
  - Subcategory validation

### Filter Agent

Set up a sequantial Workflow to execute subagents:

* **Responsibilities:**
  - Ensure that All subcategories agent and Subcategory agent are executed in order


### Deviation Agent

Focuses on pattern analysis and anomaly detection:

* **Responsibilities:**
  - Perform statistical analysis on incident patterns
  - Identify deviations from normal incident volumes
  - Calculate historical baselines and trends
  - Generate insights about incident patterns

* **Tools:**
  - `get_ticket_counts_for_deviation`: Retrieve historical ticket counts
  - Statistical analysis and trend detection algorithms

### Rerank tickets Agent

Tasked with retrieving tickets from database(after deviations are identified) and ordering them based on relevance to user query:

* **Responsibilities:**
  - Query database to retrieve all tickets related to deviations
  - Rank tickets based on relevance with user incident
  - Return most important tickets IDs related to the incident. 

* **Tools:**
  - `fetch_all_tickets_and_rerank`: Retrieve tickets related to deviations cases and rank them based on relevance to user incident

## Agent Orchestration Architecture

The system employs a hierarchical agent architecture:

```
Root Agent (Orchestrator)
├── Filter Agent (Classification)
│   ├── All subcategories agent
│   └── subcategory agent
├── Deviation Agent (Pattern Analysis)
└── Rerank tickets Agent (Retrieve and Rank)

```

### Communication Patterns

* **Synchronous Calls:** For immediate data retrieval and processing
* **Asynchronous Operations:** For long-running analyses and external API calls
* **Error Handling:** Comprehensive error recovery and fallback mechanisms
* **State Management:** Persistent context across agent interactions

## Development Guide

### Adding New Agents

1. **Create Agent Directory:**
   ```
   network_incident_agent/sub_agents/your_agent/
   ├── __init__.py
   ├── agent.py
   ├── prompt.py
   ├── utils.py
   └── tools.py
   ```

2. **Implement Agent Class:**
   ```python
   from google.adk.agents import LlmAgent
   from google.genai import types
   from google.adk.planners import BuiltInPlanner 
   
   your_agent = LlmAgent(
       name="YourAgent",
       model=YOUR_AGENT_MODEL,
       global_instruction=GLOBAL_INSTRUCTION,
       instruction=YOUR_AGENT_PROMPT,
       description="Agent description",
       tools=[your_tools],
       generate_content_config=types.GenerateContentConfig(temperature=float(YOUR_AGENT_MODEL_TEMPERATURE), ),
       planner=BuiltInPlanner(thinking_config=types.ThinkingConfig(thinking_budget=YOUR_AGENT_MODEL_THINKING_BUDGET)),
   )
   ```

3. **Register with Parent Agent:**
   Update the parent agent's `sub_agents` list to include your new agent.

### Common Issues

1. **Import Errors:**
   ```bash
   # Ensure all dependencies are installed
   poetry install
   
   # Check Python path
   poetry run python -c "import network_incident_agent"
   ```

2. **Authentication Issues:**
   ```bash
   # Re-authenticate with Google Cloud
   gcloud auth application-default login
   
   # Verify project configuration
   gcloud config list
   ```

3. **Agent Configuration Errors:**
   - Verify environment variables in `.env`
   - Check model availability in your Google Cloud project
   - Ensure proper thinking budget configuration

### Debug Mode

Enable debug logging by setting:
```bash
export LOG_LEVEL=DEBUG
```
## Contributing

1. **Fork the Repository**
2. **Create Feature Branch:** `git checkout -b feature/your-feature-name`
3. **Make Changes** following the established patterns
4. **Add Tests** for new functionality
5. **Submit Pull Request** with detailed description

### Development Standards

* Follow Python PEP 8 style guidelines
* Add comprehensive docstrings to all functions and classes
* Include unit tests for new functionality
* Update documentation for any API changes

## License
