
## Tableau Pulse

With the foundation laid, we can now turn on the engine of insight delivery: Tableau Pulse. Tableau Pulse provides high-level monitoring rather than deep-dive exploration. It's a tool for staying informed.

Pulse monitors metrics you've defined or chosen to follow from various dashboards. It continuously analyzes them for statistically significant changes, trends, outliers, and key drivers. When something noteworthy occurs, it generates an insight. Here is an example from the Tableau Pulse Demo (https://www.youtube.com/watch?v=Cnbdkye0X_8_)

**Total Sales** this month reached **$494.2k** on January 30, 2024. The unexpected small increase moved Total Sales **below the expected range** of **$499.4k to $549.3k**. The trend for **Total Sales** changed 8 days ago and is now **increasing at a slower rate.**

![](img/66cf6b89349fda3581a4f196cb3c98a4_690_0.png)

Figure 15.1: Tableau Pulse

When creating a metric in Tableau Pulse, the important step is defining relevant dimensions, such as Region or Category. They provide the context that Pulse AI uses to automatically evaluate performance drivers and explain the *why* behind a change.

Once your metrics are created, Pulse delivers insights through several channels:

- **The For You Page**: Your personalized homepage in Tableau Cloud, aggregating insights from all the metrics you follow
- Email and Slack Digests: Receive a daily, weekly, or monthly summary of key metric changes directly in your inbox or Slack channel. These digests now include sparklines for quick trend identification
- Tableau Mobile: Get the same personalized insights on the go, with deep links to explore further

### Generative AI in Pulse

The most transformative part of Pulse is its use of generative AI to create Insight Summaries. When Pulse detects a change in one of your metrics, it doesn’t just show you a chart, it also tells you about it in plain language. For example, you might receive a notification that reads:

Sales are up 12% this week, driven primarily by a strong performance in the West region (up 22%) and a surge in the 'Electronics' category. This continues the upward trend observed over the last month. You can then ask more questions about why this happened.

![](img/66cf6b89349fda3581a4f196cb3c98a4_691_0.png)

Figure 15.2: Tableau Pulse from Demo Video

This summary is powered by an LLM. Your underlying data remains private and is not used to train public models. The AI simply translates the analytical findings of Pulse into a coherent, human-readable story.

## Enhanced Q&A and correlation insights

Tableau 2026.1 significantly expands the conversational power of Pulse with Enhanced Q&A Updates, available in the Tableau+ edition.

- Analyze with AI on the Homepage: Users can now go from curiosity to insight faster with a new Analyze with AI entry point directly on the Tableau Pulse homepage. This allows business users to ask their own custom queries first, no longer needing to start with a pre-defined suggested question.
- Correlation Insights: Perhaps the most powerful addition, Enhanced Q&A now surfaces meaningful Correlation Insights between metrics. Pulse will report the relationship strength, direction, and significance between different metrics, complete with a supporting visualization. This allows you to reliably connect the dots across your business, even if the metrics originate from different data sources.

Pulse also learns from you. By using the thumbs-up/thumbs-down feedback on insights, you train the system to prioritize the types of insights you find most valuable, further personalizing your feed.

## Tableau Next

If Pulse is the intelligent notification system, Tableau Next is the autonomous analyst that can act on those notifications. Unveiled as the world’s first agentic analytics platform, Tableau Next represents a fundamental leap forward. It combines the analytical power of Tableau with the autonomous capabilities of the Agentforce platform.

![](img/66cf6b89349fda3581a4f196cb3c98a4_692_0.png)

Figure 15.3: The Tableau Next GUI from https://www.youtube.com/watch?v=pNSb-hIFNrF8

Tableau Next is an API-first, composable platform. This means its capabilities can be accessed and embedded anywhere, in a custom web application, a Salesforce record page, or a Slack conversation. It is powered by Hyperforce for enterprise-grade scalability and security, and it relies on Tableau Semantics for its trusted understanding of your business.

- Agentforce Tableau: This is the most tangible manifestation of agentic analytics. Imagine you're a sales manager looking at a dashboard of your team's opportunities. In a Tableau Next dashboard, you can now interact with **Concierge**, an embedded AI agent. Concierge is a pre-built Agentforce skill that functions as an LLM-based reasoning engine tightly integrated with Tableau's semantic layer. When a user submits a natural language query, Concierge first inherits the dashboard's current context, including active filters, field selections, user role, and row-level security permissions. It then consults the Tableau Semantics layer, which acts as a governed business glossary mapping technical field names to human-readable concepts, and generates an optimized query using the Einstein Trust Layer to ensure data privacy and governance policies are enforced. The query executes against the Hyper data engine, and the results are returned as a visualization or natural language summary, with citations linking back to the underlying data sources. You can ask complex questions conversationally:
  - Which deals over $50k are slipping in the negotiation stage?
  - Show me the forecast for next quarter based on our current pipeline
  - What's the historical win rate for this region?

Concierge understands the question, queries the semantic layer, and returns an accurate answer, often in the form of a chart or a written summary, all without you ever leaving your workflow.

- Actionable insights and embedded workflows: Getting an insight is one thing; acting on it is another. Tableau Next bridges this gap by embedding actions directly into the analytics experience. From within a dashboard or an agent's response, a user could trigger a workflow. For example, if the Concierge agent flags a potential supply chain disruption, a button could appear to Create a Jira Ticket or Send a notification to the procurement team in Slack. This transforms analytics from a passive information source into an active decision-making engine.

- An API-first, composable architecture: For the enterprise architect, Tableau Next offers unprecedented flexibility. You can use the **External Embedding SDK** to embed agentic analytics components, a single metric, a viz, or a whole dashboard, directly into any web application. The new **MCP Server** (Model Context Protocol) allows your own custom AI agents to securely connect to Tableau's semantic layer. This means your internal AI tools can execute complex analytical tasks and retrieve governed, trusted data on demand, powering automation and decision-making across your entire tech stack.

### Dashboard Summaries, Personal Orgs, and Admin Insights

The 2026.1 release introduces several powerful new capabilities in Tableau Next, all currently in Beta, that further enhance the agentic experience.

- **Dashboard Summaries (Beta)**: This feature allows you to quickly understand what is happening and why by configuring a dynamic text component that provides an AI-generated, text-based summary across the metrics in your dashboard. This is powered by the Tableau Agent and highlights meaningful trends and key drivers without requiring manual interpretation.
- **Personal Orgs (Beta):** To scale innovation while maintaining governance, **Personal Orgs** provides analysts with a secure and flexible environment to build and test new analytics. Teams can self-service by extending governed assets with new data, experimenting freely before moving approved assets into production. This balances the need for creativity with centralized control.
- **Admin Insights (Beta):** This feature empowers administrators to move from reactive to proactive analytics adoption. It provides a unified dashboard that consolidates content usage, user engagement, and consumption insights. As an analyst, you might not use this directly, but knowing your admins have these insights means they can better support your work by identifying underutilized assets or inefficiencies.

Here is a case study of sorts to envision the workflow.

- 1. **Start in Slack:** You receive a proactive alert in your Tableau Next Slack digest: Inventory for Product X has dropped 15% below target levels
- 2. **Investigate:** You click the link, which opens an embedded Tableau Next visualization directly in Slack. You see the trend and a Dashboard Summary generated by the Concierge agent: This is driven by a 40% increase in orders from the East coast in the last week.
- 3. **Ask and Act:** You tag the Concierge agent in the Slack thread: @Concierge, what’s the projected stock-out date? The agent instantly replies: At the current consumption rate, stock for Product X will be depleted in 4 days.
- 4. **Trigger a Workflow:** A Recommendation panel appears, suggesting Create a Rush Replenishment Order. You click on it, and a pre-filled purchase order form from your ERP system opens, ready for your approval.

This scenario illustrates the power of the Tableau Next vision: insights are no longer static; they are dynamic, conversational, and directly tied to business outcomes.

## Einstein Discovery

Alongside the proactive insights of Pulse and the autonomous agents of Tableau Next, a third, more established AI pillar remains critical for the master’s toolkit: Einstein Discovery. While Pulse answers what happened? and Tableau Next begins to ask, what should we do about it?, Einstein Discovery answers fundamental and predictive questions: Why did it happen? and What will happen next?

Think of Einstein Discovery as your embedded data scientist. It is an augmented analytics engine that uses **Automated Machine Learning (AutoML)** to statistically analyze your data, uncover hidden patterns, and generate predictions. It doesn’t require you to write code or build complex models from scratch. Instead, it works in the background, surfacing insights that might otherwise remain buried.

A word of caution: Integrating Einstein Discovery into a dashboard requires a disciplined approach to Field Mapping. The developer must ensure that live Tableau fields such as Lead Source or Deal Size align precisely with the inputs of the deployed Salesforce model to ensure the prediction pane provides accurate drivers and recommended actions.
