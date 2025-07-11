# Agent Tracing

An [`AgentTrace`][any_agent.tracing.agent_trace.AgentTrace] is returned when calling [`agent.run`][any_agent.AnyAgent.run] or [`agent.run_async`][any_agent.AnyAgent.run_async].

`any-agent` generates standardized (regarding the structure) [OpenTelemetry](https://opentelemetry.io/) traces regardless of the framework used, based on the [Semantic conventions for generative AI systems](https://opentelemetry.io/docs/specs/semconv/gen-ai/).

You can try to 🔍 find 🔍 the subtle differences (regarding the content) across frameworks in the examples below.

!!! tip

    The following are real traces generated by executing [one of our integration tests](https://github.com/mozilla-ai/any-agent/blob/main/tests/integration/test_load_and_run_agent.py).

## Console

Here is what the console output looks like:

=== "AGNO"

    {% include "../tests/assets/AGNO_trace.html" %}

=== "GOOGLE"

    {% include "../tests/assets/GOOGLE_trace.html" %}

=== "LANGCHAIN"

    {% include "../tests/assets/LANGCHAIN_trace.html" %}

=== "LLAMA_INDEX"

    {% include "../tests/assets/LLAMA_INDEX_trace.html" %}

=== "OPENAI"

    {% include "../tests/assets/OPENAI_trace.html" %}

=== "SMOLAGENTS"

    {% include "../tests/assets/SMOLAGENTS_trace.html" %}

=== "TINYAGENT"

    {% include "../tests/assets/TINYAGENT_trace.html" %}

!!! tip

    Showing traces in the console is enabled by default.

    You can disable the console traces using [`disable_console_traces`][any_agent.tracing.disable_console_traces]:

    ```python
    from any_agent.tracing import disable_console_traces

    disable_console_traces()
    ```

## Spans

Here's what the returned [`agent_trace.spans`][any_agent.AgentTrace.spans] look like when [dumped to JSON format](#dumping-to-file):

=== "AGNO"
    ~~~json
    {% include "../tests/assets/AGNO_trace.json" %}
    ~~~

=== "GOOGLE"
    ~~~json
    {% include "../tests/assets/GOOGLE_trace.json" %}
    ~~~

=== "LANGCHAIN"
    ~~~json
    {% include "../tests/assets/LANGCHAIN_trace.json" %}
    ~~~

=== "LLAMA_INDEX"
    ~~~json
    {% include "../tests/assets/LLAMA_INDEX_trace.json" %}
    ~~~

=== "OPENAI"
    ~~~json
    {% include "../tests/assets/OPENAI_trace.json" %}
    ~~~

=== "SMOLAGENTS"
    ~~~json
    {% include "../tests/assets/SMOLAGENTS_trace.json" %}
    ~~~

=== "TINYAGENT"
    ~~~json
    {% include "../tests/assets/TINYAGENT_trace.json" %}
    ~~~

## Dumping to File

The AgentTrace object is a pydantic model and can be saved to disk via standard pydantic practices:

```python
from any_agent import AgentConfig, AnyAgent
from any_agent.tools import search_web

agent = AnyAgent.create(
    "openai",
    agent_config=AgentConfig(
            model_id="gpt-4o",
            tools=[search_web],
    )
)
agent_trace = agent.run("Which agent framework is the best?")
with open("agent_trace.json", "w", encoding="utf-8") as f:
  f.write(agent_trace.model_dump_json(indent=2))
```
