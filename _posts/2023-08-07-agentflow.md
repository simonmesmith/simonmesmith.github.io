---
layout: single
title: Introducing Agentflow: Execute complex LLM workflows with simple JSON
tags: large-language-models workflows automation gpt-3.5 gpt-4
---

Large Language Models (LLMs) are powerful tools, but implementing complex workflows with them can be a challenge. Existing tools lack the balance between control over outputs and the ability to trigger intricate workflows from a single command. Enter [Agentflow](https://github.com/simonmesmith/agentflow), an open source solution I created to address this void.

## 1. Develop workflows using simple language
To use Agentflow, just create a JSON workflow file as follows:

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

## 2. Use variables for dynamic workflow outputs
You can include variables within curly brackets in your workflows, which you can then populate when running them. For example:

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

## 3. Create and incorporate custom functions
Agentflow allows you to use custom functions to enhance its functionality. Define new functions by inheriting from the `BaseFunction` class, then just specify the `function_call` you want to use as shown here:

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

## 4. Activate workflows with a single command
Once you've created workflows and any supporting functions, simply run the following in your command line:

```bash
python -m run --flow=workflow_name
```

Or, for workflows with variables:

```bash
python -m run --flow=workflow_with_variables_name --variables 'variable_1_name=value1' 'variable_2_name=value2'
```

(Note: `workflow_name` is the filename of the workflow JSON, minus the `.json` extension.)

Agentflow executes the specified workflow and provides a link to a folder with all outputs, including a comprehensive JSON file of workflow messages.

## Get started with Agentflow!

Check out the [installation instructions](https://github.com/simonmesmith/agentflow), explore [ideas and open issues](https://github.com/simonmesmith/agentflow/issues), and feel free to contribute to expanding Agentflow's capabilities.
