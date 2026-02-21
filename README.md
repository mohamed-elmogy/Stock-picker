project:
  name: Agentic AI Stock Picker Crew
  description: >
    A multi-agent AI system built with CrewAI that discovers trending companies
    in a given sector, performs deep financial research, selects the best
    investment opportunity, and sends a push notification with the final decision.

  overview:
    workflow:
      - Find trending companies in a sector using latest news
      - Perform deep financial research on each company
      - Analyze research and select best investment
      - Send push notification with 1-line rationale
      - Output full investment report

  architecture:
    framework: CrewAI
    llm: openai/gpt-4o-mini
    language: Python
    tools:
      - Pushover API (push notifications)
      - Requests
      - Pydantic

  agents:
    - name: trending_company_finder
      role: Financial News Analyst
      goal: >
        Read the latest news and find 2-3 trending companies
        in the selected sector. Always choose new companies.
      llm: openai/gpt-4o-mini

    - name: financial_researcher
      role: Senior Financial Researcher
      goal: >
        Provide comprehensive financial and strategic analysis
        for each trending company in a structured report.
      llm: openai/gpt-4o-mini

    - name: stock_picker
      role: Investment Decision Analyst
      goal: >
        Analyze research findings, select the best company,
        send a push notification with the decision,
        and produce a detailed investment report.
      llm: openai/gpt-4o-mini

    - name: manager
      role: Project Manager
      goal: >
        Delegate tasks between agents to achieve the final goal
        of selecting the best company for investment.

  tasks:
    - name: find_trending_companies
      description: >
        Search latest news in {sector} and identify top trending companies.
      output_file: output/trending_companies.json

    - name: research_trending_companies
      description: >
        Generate detailed financial research reports
        for the trending companies.
      output_file: output/research_report.json

    - name: pick_best_company
      description: >
        Analyze research reports, select the best company,
        notify the user, and provide final justification.
      output_file: output/decision.md

  project_structure:
    stock_picker:
      - config/
          - agents.yaml
          - tasks.yaml
      - tools/
          - push_notification_tool.py
      - crew.py
      - main.py
      - __init__.py

  installation:
    steps:
      - git clone https://github.com/yourusername/agentic-stock-picker.git
      - cd agentic-stock-picker
      - python -m venv venv
      - source venv/bin/activate  # Mac/Linux
      - venv\Scripts\activate     # Windows
      - pip install crewai openai pydantic requests

  environment_variables:
    required:
      - OPENAI_API_KEY
      - PUSHOVER_USER
      - PUSHOVER_TOKEN
    example_mac_linux: |
      export OPENAI_API_KEY="your_key"
      export PUSHOVER_USER="your_user"
      export PUSHOVER_TOKEN="your_token"
    example_windows: |
      setx OPENAI_API_KEY "your_key"
      setx PUSHOVER_USER "your_user"
      setx PUSHOVER_TOKEN "your_token"

  execution:
    command: python main.py
    default_sector: Technology
    behavior: >
      Runs the full multi-agent workflow,
      prints final decision,
      sends push notification,
      and saves structured outputs.

  outputs:
    - output/trending_companies.json
    - output/research_report.json
    - output/decision.md

  customization:
    change_sector_in: main.py
    example: |
      inputs = {
          "sector": "Healthcare",
          "current_date": str(datetime.now())
      }

  example_workflow:
    - Trending Finder → Finds NVIDIA, Palantir, Snowflake
    - Researcher → Builds detailed financial analysis
    - Stock Picker → Selects best risk/reward opportunity
    - Notification → Sends investment decision

  use_cases:
    - AI agent orchestration demos
    - FinTech automation
    - Quant research experiments
    - Portfolio projects
    - AI engineering showcases

  license: MIT

  contributions: >
    Pull requests and forks are welcome.
    Extend with additional agents, scoring models,
    real financial APIs, or deployment automation.

  tags:
    - AI
    - Multi-Agent Systems
    - CrewAI
    - FinTech
    - Automation
    - OpenAI
