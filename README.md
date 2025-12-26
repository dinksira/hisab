Hisab is a **subscription and recurring bill intelligence app** that helps users and small teams see where their money leaks through forgotten subscriptions, overlapping tools, and poorly tracked bills. It centralizes all recurring payments, sends renewal and due‑date alerts, and surfaces analytics like spend by service, team, and category so people can cancel or renegotiate in time.[litslink+2](https://litslink.com/blog/33-mobile-app-ideas-for-business-startups)

For teams, Hisab acts as a lightweight **SaaS inventory**: who owns which tool, how much it costs, when it renews, and which cost center it belongs to, giving founders and finance leads a clear picture of recurring burn.[techvify+1](https://techvify.com/saas-product-ideas)

## Frontend framework

- **Flutter (Dart)** mobile app (Android + iOS) as the main frontend.
- Rationale:
    - Great fit for **fintech‑grade** UIs, animations, and charts with high performance on both platforms.[afgprogrammer+2](https://afgprogrammer.com/ui-kits/flutter-fintech-online-banking-application-ui-kit)
    - Single codebase with strong ecosystem for state management (Bloc/Riverpod), secure storage, and push notifications.[ripenapps+1](https://ripenapps.com/blog/flutter-for-fintech-app-development/)

Key frontend responsibilities:

- Auth, onboarding, and team switching.
- Subscription & bill list, detail pages, and calendar view.
- Analytics screens (monthly trends, by service, by member).
- Notification preferences and profile/settings.[afgprogrammer+1](https://afgprogrammer.com/ui-kits/flutter-fintech-online-banking-application-ui-kit)

## Backend framework

- **Dart Frog** as the primary backend framework (REST API in Dart).[somniosoftware+1](https://somniosoftware.com/blog/how-to-build-full-stack-apps-using-flutter-and-dart-frog)
- Possible structure: Flutter + Dart Frog monorepo to share models and validation logic.[somniosoftware](https://somniosoftware.com/blog/how-to-build-full-stack-apps-using-flutter-and-dart-frog)

Core backend responsibilities:

- JWT‑based auth (individual and team accounts), invitation flows, role management.[dev](https://dev.to/djsmk123/build-full-stack-application-using-flutter-ft-dart-frog-and-mongodb-part-1-1e2k)
- CRUD APIs for Subscriptions, Bills, Payments, Teams, Members, and Events (e.g., “trial ending”, “price change”).[dev+1](https://dev.to/djsmk123/build-full-stack-application-using-flutter-ft-dart-frog-and-mongodb-part-1-1e2k)
- Scheduled jobs for upcoming renewals, reminders, and stale data cleanup.
- Webhook endpoints in future for bank/email integrations or third‑party billing systems.[dev](https://dev.to/techwithsam/full-stack-mobile-development-flutter-serverpod-4-task-crud-operations-5fh7)

If you ever decide Dart Frog is too minimal, a Dart‑native alternative like **Serverpod** can be used for more batteries‑included full‑stack features while still staying in Dart.[dev](https://dev.to/techwithsam/full-stack-mobile-development-flutter-serverpod-4-task-crud-operations-5fh7)

## AI layer

Hisab should have a small but meaningful **AI assistance layer** that starts simple and can be expanded:

- **Recommendation engine**
    - Uses embeddings + vector search to group similar subscriptions and suggest cuts (e.g., “You pay for 3 project management tools; consider cancelling one”).[getgalaxy+1](https://getgalaxy.io/learn/data-tools/best-vector-databases-2025)
    - Suggests plan downgrades or annual vs monthly savings based on usage patterns and pricing.
- **Smart insights & summaries**
    - Natural‑language summaries like “This month your recurring spend increased by 18% mainly due to X and Y.”[liveblocks](https://liveblocks.io/blog/whats-the-best-vector-database-for-building-ai-products)
    - “Nudge” messages before renewals: “You rarely log into Tool Z; think before renewing.”

Implementation details:

- Use an external LLM API (e.g., OpenAI/Anthropic) from the backend.
- Store subscription metadata and user behavior embeddings in a vector store such as **pgvector** (PostgreSQL extension) or a managed vector DB like Pinecone/Weaviate, which are standard for AI recommendations in 2025.[getgalaxy+1](https://getgalaxy.io/learn/data-tools/best-vector-databases-2025)

## Database layer

Primary **transactional database**:

- **PostgreSQL** as the main relational DB.
- Rationale:
    - Battle‑tested in fintech, strong relational modeling for users, teams, subscriptions, bills, and payments.[aalpha+1](https://www.aalpha.net/blog/fintech-app-development-using-flutter/)
    - Supports **pgcrypto** for encryption and **pgvector** for AI recommendation embeddings in the same cluster if desired.[liveblocks+1](https://liveblocks.io/blog/whats-the-best-vector-database-for-building-ai-products)

Core tables (high‑level):

- `users` – individual users (with locale, currency, role flags).
- `teams` – startups/companies; one user can belong to multiple teams.
- `team_members` – membership, roles (owner, finance, member).
- `subscriptions` – recurring services (name, vendor, amount, currency, interval, next_renewal, team_id/user_id).
- `bills` – non‑subscription recurring bills (rent, utilities) with due dates.
- `transactions` – logged payments, adjustments, refunds.
- `events` – reminders sent, AI recommendations issued, user actions (cancelled, downgraded).