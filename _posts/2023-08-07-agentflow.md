---
layout: single
title: "Introducing Agentflow: Execute complex LLM workflows with simple JSON"
tags: large-language-models workflows automation gpt-3.5 gpt-4
---

Large language models (LLMs) are powerful tools, but implementing complex workflows with them can be a challenge. 

Yes, tools like [Auto-GPT](https://github.com/Significant-Gravitas/Auto-GPT) and [BabyAGI](https://github.com/yoheinakajima/babyagi) allow LLMs to execute multiple steps, but _autonomously_&mdash;the LLMs plan and then execute tasks themselves. Because of this, in my experience with Auto-GPT, things can quickly get out of control.

What I want is to have LLMs execute multiple steps, but under my control, following a predefined path. So I scratched my own itch and built [Agentflow](https://github.com/simonmesmith/agentflow), an open source solution that lets you execute complex workflows with simple JSON. 

With Agentflow, you can:

## 1. Write workflows in plain English
Just add tasks in a JSON file like this:

```json
{
    "system_message": "Optional guiding message",
    "tasks": [
        {
            "action": "Step one."
        },
        {
            "action": "Step two."
        },
        {
            "action": "..."
        }
    ]
}
```

## 2. Add variables for dynamic outputs
You can include variables in {curly quotes} that you populate when running a workflow. For example, `target_market` is a variable here:

```json
{
    "system_message": "You are an innovative entrepreneur.",
    "tasks": [
        {
            "action": "Generate 10 product ideas for {target_market}"
        },
        {
            "action": "..."
        }
    ]
}
```

## 3. Create and use custom functions
Custom functions expand LLMs' capabilities beyond text generation. Easily define new functions by inheriting from the `BaseFunction` class. Specify functions to run using `function_call` as shown here:

```json
{
    "system_message": "You are a creative artist.",
    "tasks": [
        {
            "action": "Brainstorm 10 painting ideas for {painting_subject}."
        },
        {
            "action": "Choose the best idea."
        },
        {
            "action": "Write a prompt for an AI art generator to produce an image of the painting."
        },
        {
            "action": "Generate the painting image using the prompt.", 
            "settings": {
                "function_call": "create_image"
            }
        },
        {
            "action": "..."
        }
    ]
}
```

## 4. Run workflows with a simple command
To run a workflow, just use the command line like this:

```bash
python -m run --flow=workflow_name
```

Or, for workflows with variables, like this:

```bash
python -m run --flow=workflow_with_variables_name --variables 'variable_1_name=value1' 'variable_2_name=value2'
```

Agentflow executes the specified workflow and provides a link to a folder with all outputs, including a JSON file containing all of the LLM's responses.

## Get started with Agentflow!

Check out the [installation instructions](https://github.com/simonmesmith/agentflow), explore [ideas and open issues](https://github.com/simonmesmith/agentflow/issues), and feel free to contribute to expanding Agentflow's capabilities.
