# Multi AI Agent Systems with crewAI

[![DeepLearning.AI](https://img.shields.io/badge/DeepLearning.AI-Course-blue)](https://www.deeplearning.ai/)
[![crewAI](https://img.shields.io/badge/crewAI-Framework-orange)](https://www.crewai.com/)
[![Python](https://img.shields.io/badge/Python-3.10+-yellow)](https://www.python.org/)

## Introduction

Welcome to the repository for the course **[Multi AI Agent Systems with crewAI](https://www.deeplearning.ai/short-courses/multi-ai-agent-systems-with-crewai/)**, offered by **[DeepLearning.AI](https://www.deeplearning.ai/)** in collaboration with **[crewAI](https://www.crewai.com/)**.

This course explores the cutting-edge domain of Agentic AI, moving beyond simple prompt engineering to creating orchestrated teams of AI agents capable of performing complex, multi-step tasks. By leveraging the **crewAI** open-source framework, this course provides a hands-on approach to automating real-world business processes.

### Core Concepts
The curriculum focuses on designing effective agents by mastering these key components:
* **Role-playing:** Assigning specialized personas to agents to increase performance and focus.
* **Memory:** Implementing short-term, long-term, and shared memory to maintain context across tasks.
* **Tools:** Equipping agents with capabilities to interact with the outside world (e.g., web search, API integration).
* **Focus:** Breaking down complex goals into granular tasks assigned to specific agents.
* **Guardrails:** Implementing strategies to handle errors, prevent hallucinations, and avoid infinite loops.
* **Cooperation:** Orchestrating task execution in series, parallel, or hierarchically.

### Practical Applications
Throughout the modules, we build agent crews to automate six common business processes:
1.  **Job Search:** Tailoring resumes and interview preparation.
2.  **Content Creation:** Researching, writing, and editing technical articles.
3.  **Customer Support:** Automating inquiry classification and responses.
4.  **Outreach:** Conducting targeted customer outreach campaigns.
5.  **Event Management:** Planning and executing logistics for events.
6.  **Finance:** Performing data-driven financial analysis.

---

## Course Topics

Below is a detailed breakdown of every module covered in this course. Click on the arrows to expand the detailed notes for each topic.

<details>
<summary><strong>1. Overview</strong></summary>

### The Shift to Agentic Workflows

The course begins by establishing the fundamental shift occurring in the world of Artificial Intelligence: the transition from direct "Zero-Shot" prompting to **Agentic Workflows**. While traditional Large Language Model (LLM) interactions involve a human sending a prompt and receiving a single response, an agentic system involves an AI entity that can reason, plan, and execute multiple steps to achieve a higher-level goal.

In this overview, we explore why single LLMs often struggle with complex tasks. When asked to "write a researched article," a single model might hallucinate facts or lose coherence over long contexts. However, by breaking this request into an agentic workflow—where one agent researches and another writes—we significantly improve reliability and quality.

We introduce **crewAI**, the framework used throughout this course. crewAI is designed to make building these multi-agent systems accessible and modular. It operates on the premise that AI agents should function like a well-structured human team. Just as a software engineering team has a Product Manager, a Developer, and a QA Engineer, a crewAI system allows you to define specific roles, goals, and backstories for digital workers.

The overview also touches on the modularity of the framework. crewAI is agnostic to the underlying LLM; while we may use OpenAI's GPT-4 or localized models via Ollama, the orchestration logic remains the same. This flexibility is crucial for enterprise adoption where data privacy or cost constraints might dictate the choice of model. By the end of the overview, the learner understands that they are not just learning a Python library, but a new paradigm of computing where software is no longer just a tool, but a proactive collaborator.

</details>

<details>
<summary><strong>2. AI Agents</strong></summary>

### Defining the Digital Worker

What exactly constitutes an AI Agent? This module demystifies the buzzword. In the context of crewAI, an agent is a wrapper around a Large Language Model that is given three specific attributes: a **Role**, a **Goal**, and a **Backstory**.

1.  **Role:** This defines the agent's function within the crew. For example, "Senior Researcher" or "Customer Support Representative." The role primes the LLM to adopt a specific persona, narrowing its focus and improving the relevance of its outputs.
2.  **Goal:** This is the objective the agent is trying to achieve. A clear goal helps the agent prioritize its actions. For a researcher, the goal might be "Uncover groundbreaking developments in AI for 2024."
3.  **Backstory:** This provides context and personality. It explains *why* the agent is doing what it is doing. A backstory might read, "You are a veteran tech journalist with a keen eye for detail and a skepticism of hype." This nuance helps guide the tone and style of the agent's output.

This section explains the technical architecture of an agent. When an agent is initialized in crewAI, it isn't just a text generator; it is an entity capable of looping. It can Assess -> Think -> Act -> Observe. We discuss the "ReAct" (Reasoning and Acting) prompting strategy that underpins this behavior. The agent receives a task, reasons about how to solve it, selects a tool (if available), observes the output of that tool, and then updates its reasoning.

We also cover the parameters available for fine-tuning agents, such as `allow_delegation` (can this agent ask others for help?) and `verbose` (do we want to see the agent's internal thought process in the console?). This foundational knowledge is critical because a multi-agent system is only as strong as the individual agents comprising it. If the agents are poorly defined, the collective output will be disjointed.

</details>

<details>
<summary><strong>3. Create agents to research and write an article</strong></summary>

### Building Your First Crew

In this hands-on module, we move from theory to code by building a classic content creation crew. The objective is to produce a high-quality technical article. To do this, we instantiate two distinct agents: a **Tech Researcher** and a **Content Writer**.

**The Tech Researcher:**
We define this agent with a role focused on information gathering. Its goal is to find the latest trends on a specific topic. We realize here that an LLM by itself has a training cutoff; it doesn't know the news from today. This introduces the necessity of **Tools** (specifically search tools), which allows the agent to fetch real-time data. We observe how the agent takes a broad topic, breaks it down into search queries, and synthesizes the results.

**The Content Writer:**
The second agent is the Writer. Its backstory emphasizes engaging, accessible writing. Crucially, this agent does not need search tools. Its input is the output from the Researcher. This demonstrates the concept of **Sequential Processes**. The Researcher passes its findings to the Writer, who transforms raw data into a polished narrative.

We explore the code structure for defining `Task` objects. A task in crewAI requires a description and an `expected_output`. We assign the research task to the Researcher and the writing task to the Writer. Finally, we instantiate the `Crew` object, passing in the agents and tasks, and select the process type (Sequential).

When we run this crew, we witness the power of specialization. The Writer doesn't hallucinate facts because it is grounded by the Researcher's data. The Researcher doesn't produce dry, bulleted lists because the Writer acts as a stylistic filter. This module proves that two specialized agents outperform a single "do-it-all" prompt.

</details>

<details>
<summary><strong>4. Key elements of AI agents</strong></summary>

### The Anatomy of Performance

This module provides a deep dive into the nuances that make an agent effective versus one that gets stuck in loops. We analyze the "Hyperparameters" of agent creation.

**Role-Playing and Prompt Engineering:**
We discuss how crewAI leverages the "System Prompt" of the underlying LLM. By defining a rich backstory, we are effectively performing advanced prompt engineering behind the scenes. We explore examples of weak vs. strong backstories. A weak backstory is "You are a coder." A strong backstory is "You are a Principal Software Engineer at Google with 10 years of experience in Python, known for writing clean, documented, and efficient code." The latter elicits significantly better code generation.

**Inter-Agent Communication (Delegation):**
We explore the `allow_delegation` flag in depth. When set to `True`, an agent can pass tasks to other agents if it feels it cannot complete them alone. This mimics a real-world manager-employee relationship. We discuss scenarios where you *should* and *should not* use this. For strictly defined pipelines (like our article writer), delegation might be disabled to ensure a predictable flow. for open-ended research, delegation allows for dynamic problem solving.

**Memory and Context Window:**
Agents have limits. We discuss how crewAI handles the context window (the amount of text an AI can process at once). We introduce the concept of **Short-term Memory** (what happened in this execution) vs. **Long-term Memory** (stored in a vector database for future retrieval). This allows agents to "learn" from past executions, preventing them from repeating mistakes. We also touch on the technical implementation of how tasks are passed between agents—ensuring that the context isn't lost but also doesn't overflow the token limit.

</details>

<details>
<summary><strong>5. Multi agent customer support automation</strong></summary>

### Empathy and Logic at Scale

Customer support is one of the highest-value use cases for AI agents. In this module, we design a crew capable of handling incoming support tickets. This adds a layer of complexity: **Conditional Logic** and **Tone Assurance**.

We create agents such as a **Senior Support Analyst** and a **QA Specialist**.
The Support Analyst's role is to diagnose the issue. However, unlike the Researcher in the previous module, this agent requires access to internal documentation (simulated via tools). We teach the agent to differentiate between a "Bug," a "Feature Request," and a "General Inquiry."

**The Quality Assurance (QA) Agent:**
This is a vital concept in agentic workflows—the "Human in the Loop" surrogate. Instead of sending the Support Analyst's draft directly to the customer, it passes through a QA Agent. The QA Agent's instructions are to review the response for tone, accuracy, and empathy. If the response is too robotic or technically incorrect, the QA agent can send it back (feedback loop) or rewrite it.

We also explore how to hook this crew into a real data stream. While we simulate inputs in the course, we discuss how this architecture would connect to an API like Zendesk or Intercom. The "Input" for the crew becomes the content of the support ticket. We emphasize the importance of **Guardrails** here—ensuring the agent never promises refunds it can't authorize or shares private data. This module highlights that multi-agent systems are excellent for balancing the *efficiency* of automation with the *quality control* required for client-facing interactions.

</details>

<details>
<summary><strong>6. Mental framework for agent creation</strong></summary>

### Thinking Like a Manager

This module shifts from coding to **Systems Design**. Building a crew is less about writing Python and more about organizational management. We introduce a mental framework for designing crews: **Identify -> Decompose -> Assign**.

**1. Identify the Goal:**
What is the final deliverable? Is it a report? A piece of code? A JSON file? Being vague here leads to vague agents. We learn to define the "Definition of Done."

**2. Decompose the Process:**
How would a human team do this? If you were hiring humans to do this task, who would you hire? You wouldn't hire one person to be the CEO, CTO, and Intern simultaneously. You would break the process into steps.
* Step 1: Gather data.
* Step 2: Analyze data.
* Step 3: Format report.

**3. Assign Roles and Tools:**
Once the process is broken down, we map agents to these steps. We ask: What tools does the "Step 1" agent need? Does the "Step 2" agent need the raw data or just the summary?

We also discuss the concept of **Agent Granularity**. When should a task be one agent, and when should it be two? The rule of thumb provided is: if the prompt for one agent becomes too long or complex (e.g., "Do X, then Y, but if Z happens do A, and ensure B"), it is time to split that agent into two. This framework helps prevent "Agent Confusion," where an LLM forgets part of its instructions because it is overloaded with conflicting constraints.

</details>

<details>
<summary><strong>7. Key elements of agent tools</strong></summary>

### Bridging the Gap to the Real World

An agent without tools is just a brain in a jar. It can think, but it cannot act. This module explores the `Tool` class in crewAI and LangChain. Tools are the interfaces that allow agents to interact with APIs, databases, files, and the internet.

We categorize tools into two types:
1.  **Data Retrieval Tools:** Google Search (SerperDev), Wikipedia scrapers, internal document readers (RAG - Retrieval Augmented Generation). These tools bring information *into* the context window.
2.  **Action Tools:** File writers, Email senders, API posters (e.g., posting a tweet or a Slack message). These tools push information *out* to the world.

We discuss the technical definition of a tool. A tool consists of a **Name**, a **Description**, and the **Function Logic**. The *Description* is critically important. The LLM uses the description to decide *when* and *how* to use the tool. If the description is vague, the agent might hallucinate how to use it or ignore it entirely.

We also cover **Fault Tolerance** in tools. What happens if the API fails? We learn how to build error handling into tools so that if a tool crashes, the agent receives an error message (e.g., "Search failed, try a different query") rather than the whole program crashing. This resilience is what allows agents to recover and retry, a key characteristic of autonomous systems.

</details>

<details>
<summary><strong>8. Tools for a customer outreach campaign</strong></summary>

### The Hunter and The Scribe

Applying our knowledge of tools, we build a **Sales & Outreach Crew**. This scenario requires agents to actively find leads and craft personalized communications.

**The Lead Generator Agent:**
We equip this agent with search tools to find potential clients based on specific criteria (e.g., "AI startups in San Francisco"). We discuss the ethical and technical aspects of scraping and data gathering. This agent's output is a list of names, companies, and maybe recent news about them.

**The Personalization Agent:**
Standard cold emails are ignored. To fix this, we create a Personalization Agent. This agent uses tools to read the specific website or recent blog posts of the lead found by the previous agent. It looks for "hooks"—shared interests, recent awards, or specific pain points.

**The Outreach Writer:**
Finally, a writer agent takes the lead data and the personalization hooks to draft the email. We use a **File Write Tool** to save these drafts to a local folder.

This module demonstrates how tools can be chained. The output of the search tool becomes the input for the scraping tool, which becomes the context for the writing tool. We also see the value of structured output. We might ask the agents to output data in JSON format so that it can be programmatically parsed later, rather than just free text.

</details>

<details>
<summary><strong>9. Recap of tools</strong></summary>

### Best Practices and Optimization

This section reviews the tool ecosystem. We reiterate the importance of **Tool Assignment**. Not every agent should have every tool. Giving a "Writer" agent access to a "Google Search" tool might distract it; it might try to verify facts instead of just writing. Restricting tools enforces the separation of concerns.

We discuss **Custom Tools**. While crewAI comes with pre-built tools (via LangChain), the real power comes from writing custom Python functions decorated as tools. We review how to use the `@tool` decorator. This allows us to wrap *any* Python code—complex mathematical models, database SQL queries, or proprietary internal APIs—into a format the agent can use.

We also touch on **Argument Schemas**. Using Pydantic models, we can enforce strict typing on the inputs agents provide to tools. If a tool requires a URL, we can ensure the agent doesn't pass a random string. This type-safety prevents many common runtime errors in agentic workflows. The recap emphasizes that tools are the "limbs" of the agent, and they must be strong, precise, and well-described.

</details>

<details>
<summary><strong>10. Key elements of well defined tasks</strong></summary>

### The Blueprint of Execution

If Agents are the workers and Tools are the equipment, **Tasks** are the instructions. This module argues that a poorly written task is the most common cause of agent failure.

We dissect the `Task` object in crewAI. It requires:
1.  **Description:** A detailed, natural language explanation of what needs to be done.
2.  **Expected Output:** A clear definition of what the result looks like. (e.g., "A 4-paragraph blog post formatted in Markdown," vs "A blog post.")
3.  **Agent:** The specific agent assigned to this task.

**Context Propagation:**
We explain how tasks are linked. In crewAI, the output of Task A is automatically passed as context to Task B (in sequential mode). However, we can also manually explicitly `context=[task1, task2]`. This allows a task later in the chain to reference data generated several steps prior, not just the immediate predecessor.

**Asynchronous Execution:**
We introduce the concept of `async_execution=True` for tasks. Some tasks, like gathering data from 5 different websites, don't need to block the main thread. We can have agents working in the background while others proceed, speeding up the overall process.

**Human Input:**
Sometimes, AI isn't enough. We discuss the `human_input=True` parameter. This pauses the execution and asks the human user for approval or feedback before the agent finalizes the task. This is crucial for high-stakes tasks like publishing content or sending money.

</details>

<details>
<summary><strong>11. Automate event planning</strong></summary>

### Logistics and Coordination

Event planning is a chaotic process involving venues, catering, scheduling, and budgeting. It is the perfect stress test for a multi-agent system. In this module, we build a crew to plan a tech conference.

We introduce specialized agents:
* **Venue Coordinator:** Finds locations based on capacity and budget.
* **Logistics Manager:** Handles catering and equipment.
* **Marketing Communications:** Creates the agenda and promotional materials.

This module highlights the **Interdependency of Tasks**. The Logistics Manager cannot book catering until the Venue Coordinator has confirmed the location (because the venue might have exclusive catering contracts). The Marketing agent cannot finalize the agenda until the speakers are confirmed.

We see how to structure these dependencies in the `crew` definition. We also use this opportunity to structure complex outputs. We ask the agents to produce a detailed P&L (Profit and Loss) statement and an hour-by-hour itinerary. This shows that agents can handle numerical constraints (budget) and temporal constraints (schedule) simultaneously, provided the task descriptions are explicit about these limits.

</details>

<details>
<summary><strong>12. Recap on tasks</strong></summary>

### The Art of Delegation

This review consolidates our understanding of task management. We focus on the iterative process of **Task Refinement**. Rarely does a developer write the perfect task description on the first try. We discuss the debugging loop: Run the crew -> Observe the output -> Tweak the task description -> Repeat.

We revisit **Task Context**. We clarify the difference between *context* (inputs from other tasks) and *tools* (external lookups). We emphasize that context is usually better for maintaining narrative flow, while tools are better for fetching facts.

We also discuss **Callback Functions**. crewAI allows developers to attach callbacks to tasks. This means when a task completes, a specific Python function can trigger (e.g., logging the output to a database or sending a Slack notification to the human user). This allows the AI system to integrate tightly with existing non-AI software infrastructure.

</details>

<details>
<summary><strong>13. Multi agent collaboration</strong></summary>

### Beyond the Assembly Line

Up until this point, we have largely relied on **Sequential** processes (A -> B -> C). This module introduces **Hierarchical** processes. This is a more advanced orchestration method available in crewAI.

In a Hierarchical process, we introduce a **Manager Agent** (often powered by a more intelligent model like GPT-4, while workers use GPT-3.5 or smaller models). The user gives the goal to the Manager. The Manager then analyzes the goal, looks at the available "worker" agents, and dynamically assigns tasks to them.

This mimics a real-world boss. The Manager reviews the output of the workers. If the output is insufficient, the Manager can reject it and ask the worker to redo it, or assign it to a different worker. This loop continues until the Manager is satisfied.

We discuss the trade-offs. Hierarchical processes are more autonomous and can handle more ambiguity, but they are also more expensive (more tokens used for management overhead) and slower. We learn how to choose between Sequential (fast, predictable) and Hierarchical (adaptive, powerful) based on the business use case. We also touch on **Shared Memory** here—how the Manager maintains the "Global State" of the project while workers focus on local tasks.

</details>

<details>
<summary><strong>14. Multi agent collaboration for financial analysis</strong></summary>

### High-Stakes Analytical Crews

Financial analysis requires precision, data aggregation, and cautious interpretation. In this module, we build a crew to analyze stock market data.

**The Data Analyst Agent:**
Uses tools to scrape stock prices, news sentiment, and SEC filings.
**The Trading Strategy Agent:**
Analyzes the data to identify trends (Bullish/Bearish).
**The Investment Advisor Agent:**
Synthesizes the analysis into a coherent report with risk warnings.

This module is distinct because it heavily utilizes **Quantitative Data**. LLMs are historically bad at math. We discuss how to solve this by giving agents "Calculator Tools" (Python REPL) so they compute numbers programmatically rather than predicting them token-by-token.

We also explore **Consensus Building**. In a financial context, we might want different agents to debate. One agent plays "Devil's Advocate" against a particular investment thesis. By forcing the agents to critique each other's logic before generating the final output, we reduce the risk of biased or overly optimistic advice. This "Adversarial Agent" pattern is a powerful technique for high-stakes decision-making.

</details>

<details>
<summary><strong>15. Build a crew to tailor job applications</strong></summary>

### The Capstone Project

The final module brings everything together in a personal productivity tool: The Resume Tailor. This solves a common pain point—customizing a CV for every single job application.

**The Workflow:**
1.  **Job Board Scraper:** Finds the job description URL.
2.  **HR Specialist Agent:** Analyzes the job description to extract key keywords, required skills, and cultural values.
3.  **Resume Strategist:** Takes the user's "Master Resume" and the "Job Analysis." It maps the user's experience to the job requirements.
4.  **Interview Prep Agent:** Generates a list of likely interview questions based on the specific resume-job gap.

**Key Learnings:**
* **File I/O:** Reading a local PDF/Markdown resume and writing a new version.
* **Personalization:** Unlike the generic article writer, this crew must adhere strictly to the facts of the user's actual experience (to avoid lying on a resume). We discuss how to prompt agents to "highlight" truth rather than "invent" fiction.
* **Presentation:** The final output isn't just text; it's a structured document ready for submission.

This project serves as the final validation of the learner's ability to orchestrate agents, manage context, utilize file-system tools, and solve a tangible real-world problem with crewAI.

</details>

---

## Acknowledgement

This repository is intended for **learning and educational purposes only**.

The course content, syllabus, and primary concepts are the intellectual property of **[DeepLearning.AI](https://www.deeplearning.ai/)** and **[crewAI](https://www.crewai.com/)**. All rights regarding the course material belong to their respective owners. This README serves as a structured study guide and documentation of the concepts covered in their official "Multi AI Agent Systems with crewAI" course.