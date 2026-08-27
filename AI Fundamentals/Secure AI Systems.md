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

_______________________________________________________________________
_______________________________________________________________________

**OWASP LLM TOP 10**

**LLM01**_Prompt injection:_ manipulating llm behaviour through crafted input.
**LLM02**_Sensitive information disclosure:_ leaking confidential information. PII. or system details through responses.
**LLM03**_Supply chain:_ compromised pre-trained models, datasets and third party dependancies introduced before deployement.
**LLM04**_Data and model poisoning:_ corrupt model data, or weights to alter behavious.
**LLM05**_Improper Output handling:_ LLM output is causing injection downstream systems.
**LLM06**_Excessive agency:_ AI components with more previlege or autonomy than necessary.
**LLM07**_System prompt leakage:_ Exposure of system level instructions and internal configuration.
**LLM08**_Vector and embedding weaknesses:_ Exploiting retrieval mechanisms and embedding pipelines.
**LLM09**_Misinformation:_ LLM generating false or misleading content.
**LLM010**_Unbounded Consumption:_ Resource exhaustion, cost explosion, denial of service.

