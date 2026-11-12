# # Tableau Pulse integration

Tableau Pulse represents a fundamental shift in how Tableau delivers insights, moving from a toolkit for building dashboards to an intelligent, personalized analytics experience powered by AI. It's designed to put metrics and explanations—not just charts—at the center of the user's workflow.

Instead of starting with a dashboard, Pulse starts with your key business Metrics. You define or automatically generate these critical KPIs (like "Monthly Recurring Revenue" or "Customer Churn Rate") from your data sources. Pulse then continuously monitors them.

Tableau Pulse adds an intelligent layer on top of your existing data and Tableau content. Its AI engine automatically:

+   - It scans your data to suggest relevant, trustworthy metrics and maintains them as a single source of truth.

+   - For each metric, Pulse doesn't just show a trend line. Its AI provides natural-language summaries, highlighting significant changes, contributing factors, and anomalies directly in context.

+   - It tailors the experience for every user. A sales director and a regional manager looking at the same "Revenue" metric will see explanations and breakdowns relevant to their specific role and data segment.

+   - Insights are delivered through digest emails, a personalized web homepage, and integrated directly into tools like Slack and Salesforce. Users don't have to go hunting; key changes come to them.

Pulse features a natural language interface where users can ask follow-up questions in plain English about any metric or insight, drilling deeper without needing to build new views. This is where it integrates with capabilities like Einstein Co-Pilot for a seamless conversational flow from insight to action.

Tableau Pulse is the manifestation of Tableau's AI vision, automating the lifecycle of a data insight. It transforms static dashboards into a dynamic, intelligent, and personalized conversation with your data, making analytics more accessible, proactive, and integrated into daily decision-making.

# # Enhanced data connectivity

The 2026.1 release introduces two major enhancements for connecting to data that deserve special attention.

+   - Connect to data from your REST API endpoint without writing custom code. The new REST API Connector unlocks API data for business users without developer resources, enabling self-service analytics and reducing time-to-analysis. The REST API Connector is the intended replacement for the deprecated Web Data Connector option. This is a game-changer for organizations that rely on API data. Instead of waiting for IT to build custom connectors or write Python scripts, business users can now connect directly to REST APIs and bring that data into Tableau for analysis.

+   - Accelerate your journey to the cloud and centralize governance with Bring Your Own Connector (BYOC), now in Pilot. Organizations using proprietary systems or unsupported databases can now deploy custom drivers and connectors, enabling migration to Tableau Cloud. BYOC unlocks the full power of web authoring, published data sources, extracts, and Tableau Prep flows while ensuring site-level governance and usage monitoring.