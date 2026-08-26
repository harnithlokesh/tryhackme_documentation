**SECURING AI SYSTEMS**

*User Interface*	Developer-facing chat widget embedded in the code review platform

*API Gateway*	Authentication, rate limiting, request routing

*Orchestration Layer*	Manages conversation state, routes requests, coordinates components

*Prompt Construction*	Combines the system prompt, user query, and retrieved context into the final prompt sent to the model

*LLM*	The language model (hosted internally or accessed via API) that generates responses

*Tool Layer*	Functions the LLM can invoke: database queries, documentation search, CI/CD status checks

*Output Processing*	Response formatting, content filtering, length enforcement

*Logging and Monitoring*	Conversation storage, usage analytics, audit trail

*Vector Store*	Embedded representations of internal documentation for retrieval-augmented generation (RAG)

_______________________________________________________________________
_______________________________________________________________________

**BOUNDARIES**

*user-to-system:* untrusted natural language enters the system 

*System-to-LLM:* constructed prompt is then sent to the model 

*LLM-to-tools:* model output triggers database queries, API calls, or file operations.

*System-to-external-data:* retrieved documents from vector store    or external sources enter the prompt

*system-to-user:* generated response is delivered to the user


