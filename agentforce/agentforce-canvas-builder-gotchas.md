# Agentforce Canvas Builder: lessons learned & gotchas

## General Builder Behavior

- The canvas uses / to embed inline actions directly in instructions (not @). Typing /run triggers an action chip inline.
- @ is used to reference variables and resources, not to embed actions.
- Variables created in a Flow are NOT automatically available in the agent canvas — they must be created separately as agent-level custom variables.
- The canvas Script view and Canvas view are two different ways to see the same thing — Script is read-only for verification, Canvas is where you build.
- Always hit Save before switching between topics or tabs or you will lose work.

## Variables

- Flow variables and agent canvas variables are separate systems. You must create variables in both places if you need them in both.
- When creating agent-level variables from the canvas action "Select variable" dropdown, use Create a variable — it creates a custom variable scoped to the agent.
- Variable Data Type in the canvas is called String not Text — they are the same thing.
- Always set a Default Value on variables that need a fallback — especially for demo purposes where values are hardcoded.
- Boolean variables default to False unless specified.
- For demo purposes, hardcode default values at the variable level rather than in instructions to keep the canvas clean and professional.

## Flows

- Use Autolaunched Flow (No Trigger) for AgentForce actions.
- Flow variables have three availability settings: Input, Output, or both. Be deliberate — input variables receive data, output variables return data to the agent.
- If a value is always the same for demo purposes, set it as the Default Value on the flow variable rather than parsing it dynamically. This is cleaner and more reliable.
- After building a flow you MUST Activate it before it appears as an available action in the AgentForce canvas.
- Flow labels and descriptions are visible in the canvas and potentially in demos — keep them professional and avoid language that reveals implementation details (e.g. never say "hardcoded").

## Prompt Templates

- Prompt Templates are built in Prompt Builder via Setup, not inside the AgentForce canvas.
- Use Flex as the Prompt Template Type for AgentForce actions.
- Use Free Text as the Source Type for simple string inputs like model numbers.
- The retriever is embedded directly inside the prompt template using Retrievers:File_[DataLakeObjectName]("query") syntax.
- You MUST Activate the prompt template before it appears as an available action in the canvas.
- When adding a prompt template as a canvas action, select Prompt Template as the Reference Action Type.
- The prompt template handles the retriever internally — you do not need to wire the retriever separately in the canvas.
- product_model input should be marked as Required since the retriever needs it to run.
- Citation Mode and Preview Only inputs should NOT be marked as required.
- Prompt Response output should have Show in conversation checked so results display to the user.
- Response time can be slow (20+ seconds) because it runs the full retriever pipeline — this is expected and can be framed positively in demos as the agent "doing real work."

## RAG / Knowledge / Retrievers

- RAG in Agentforce is configured at the agent level via the knowledge block in the agent script — NOT as an action.
- The knowledge block in the agent script has three fields: rag_feature_config_id, citations_url, and citations_enabled.
- If these fields are empty and citations_enabled: False, the knowledge/RAG is not connected.
- The three supported action target schemes are: flow://, apex://, and prompt (prompt template). RAG is not an action target.
- Data Cloud Document AI configs (with vector embeddings active) are the correct source for retriever content.
- Confirm Data Lake Objects show Active status for Directory Table, Content chunks, and Vector Embedding before connecting.

## Action Configuration in Canvas

- When adding an action, always use Create and Open not just Create so you can immediately configure inputs and outputs.
- Input mappings: the left side is what the flow/template expects, the right side is where the agent gets the value from.
- Output mappings: the left side is what comes back from the action, the right side is the agent variable that stores it.
- "Agent Populated" means the agent will figure out the value from context — use this for optional inputs.
- For inputs you want to pass a specific variable to, click "Agent Populated" and switch to the variable.
- Show in conversation on an output means the result displays visibly in the chat — always check this for outputs you want the customer to see.
- Filter from agent context keeps the output from influencing the agent's reasoning — leave unchecked unless specifically needed.

## Demo-Specific Rules

- Never hardcode product names or values directly in canvas instructions — always reference via variable (e.g. {!@variables.modelNumber}) so it looks dynamic even if the backend is fixed.
- Never use descriptions or labels that reveal that values are hardcoded or static — keep all language professional and functional.
- Flow descriptions, variable descriptions, and action descriptions are all potentially visible — treat them as customer-facing.
- The card UI (product image, buttons) only renders correctly in the actual messaging widget (e.g. the QA URL on mobile) — not in the AgentForce Preview simulator.
- Preview is useful for testing routing, action firing, and response content — but not for testing card/rich UI rendering.
- Progress indicator messages (e.g. "Retrieving guided steps") show during action execution — use these to reinforce the story of the agent doing intelligent work.

## Naming Conventions (for demo cleanliness)

- Action names: Agentforce Unboxing Agent [Function] Script or Agentforce Unboxing Agent [Function] [Type]
- Flow names: match the action name exactly for consistency
- Variable names: camelCase (modelNumber, setupSteps, welcomeMessageOutputValues)
- Descriptions: always complete, professional, functional — never blank
