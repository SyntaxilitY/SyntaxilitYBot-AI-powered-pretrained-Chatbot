# SyntaxilitYBot — AI powered pretrained Chatbot

SyntaxilitYBot is a AI-based chat integration platform that connects your application to modern LLM providers (OpenAI ChatGPT, Google Gemini / PaLM) to provide conversational automation, context-aware responses, and synchronization tools for multi-channel chat workflows. The project demonstrates how to integrate generative AI into a web app while managing API configuration, secure settings, and conversation persistence.

A AI-powered chatbot backend that integrates ChatGPT and Google Gemini to deliver AI-driven conversational features for web applications and services.

## Highlights & Features
- Integrates with OpenAI ChatGPT and Google Gemini (PaLM) APIs.
- Django-based architecture for user/session management and persistent conversation history.
- Abstractions for swapping LLM providers with minimal code changes.
- Example endpoints and basic frontend integration for testing conversations.
- Environment-driven configuration to keep API keys secure.
- Designed to be extended for multi-channel sync (web, Slack, Telegram, etc.).

## Technologies & Tools
- Language: Python (Django)
- Web framework: Django (REST endpoints / views)
- AI APIs: OpenAI (ChatGPT), Google Gemini / PaLM (via Google Cloud APIs)
- Data: Django ORM (SQLite/Postgres)
- Dev tooling: pip, virtualenv / venv
- Optional: Docker for local/dev containerization

## Skills & Expertise Demonstrated
- Backend web development with Django
- RESTful API design and secure configuration management
- Integration with external APIs (OpenAI, Google Cloud)
- Asynchronous thinking for conversational flows and background tasks
- Environment and secret management for production safety
- Adapting to multiple LLM providers via clear abstraction layers

## Challenges encountered and how to overcome them
1. Managing different provider APIs and request formats
   - Solution: Implement a provider abstraction/interface that normalizes request/response shapes, so switching or adding providers requires minimal changes.

2. Securely storing and using API keys
   - Solution: Use environment variables and a .env approach; never commit secrets, provide clear docs for required env vars.

3. Conversation context management (history, tokens)
   - Solution: Persist conversation history in the database with trimming or summarization to respect token limits; add configurable window sizes and optional summarization before sending to the LLM.

4. Rate limits and retries
   - Solution: Add exponential backoff, retry strategies, and optional queuing (e.g., Celery/RQ) for high-throughput scenarios.

5. Bridging sync across multiple channels
   - Solution: Design an intermediate message model and adapters for each channel (Slack, Telegram, web UI) to unify message shape and metadata.

## Real-world use cases
- Customer support automation: Use SyncBot as an assistant to draft responses, suggest replies, or fully automate low-risk interactions.
- Content generation: Prototyping content or suggestions based on conversation prompts.
- Internal knowledge base assistant: Integrate with internal docs and provide employees a conversational interface to query policies or procedures.
- Multi-channel notification sync: Coordinate conversations that originate in one channel and continue across others while preserving context.

---

## Quickstart — Local Development

Prerequisites:
- Python 3.10+ (adjust per project requirements)
- pip
- Git
- (Optional) Docker

1. Clone the repo
   git clone https://github.com/TariqMehmood1004/syncbot-django-python-chatgpt-gemini-ai.git
   cd syncbot-django-python-chatgpt-gemini-ai

2. Create and activate a virtual environment
   python -m venv .venv
   source .venv/bin/activate   # macOS / Linux
   .venv\Scripts\activate      # Windows

3. Install dependencies
   pip install -r requirements.txt

4. Environment variables
   Create a `.env` file in the project root (or set env vars in your environment). Example variables:
   DJANGO_SECRET_KEY=your-django-secret-key
   DJANGO_DEBUG=True
   DATABASE_URL=sqlite:///db.sqlite3
   OPENAI_API_KEY=sk-...
   GOOGLE_API_KEY=AIza...
   GOOGLE_PROJECT_ID=your-gcloud-project
   DEFAULT_LLM_PROVIDER=openai  # or google

   Notes:
   - For Google Gemini, ensure you enable the appropriate Google Cloud APIs and provide credentials per Google Cloud SDK instructions (service account or API key depending on usage).
   - Use a service account JSON and GOOGLE_APPLICATION_CREDENTIALS when using server-side Google APIs if required.

5. Run migrations
   python manage.py migrate

6. Create a superuser (optional)
   python manage.py createsuperuser

7. Start the dev server
   python manage.py runserver

8. Try the example endpoints
   - Visit the provided UI endpoints (if included) or use curl/Postman to POST to the conversation endpoints as documented in the repository's code.

## Configuration & Extensibility
- LLM Provider Abstraction: The project should have a provider interface (e.g., providers/base.py) and concrete implementations (providers/openai.py, providers/google_palm.py). Add a new provider by implementing the interface and adding the provider to settings.
- Conversation Persistence: Conversation model stores messages with metadata (sender, timestamp, provider tokens). Add summarization jobs or trimming policies as needed.
- Background Jobs: For heavy loads or long-running requests, integrate Celery or RQ to offload API calls.

## Security & Production Notes
- Never commit .env or credential files to source control.
- Use secret management for production (e.g., Google Secret Manager, AWS Secrets Manager, or environment variables in your orchestration platform).
- Rate-limit incoming requests at the app or API gateway layer and handle provider rate limits gracefully.
- Monitor usage and costs from LLM providers.

## Contributing
1. Fork the repository
2. Create a feature branch
3. Run tests (if present) and linting
4. Open a pull request with a clear description of your changes

## Troubleshooting
- "401 Unauthorized" or permission errors: check API keys and Google Cloud project setup.
- "Token limit exceeded": implement history trimming or summarization before sending to providers.
- High latency: consider asynchronous calls, background workers, caching common responses.

## License
MIT License
