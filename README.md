# Vyuit / Doori
IBM Bob Hackathon Submission | Team ZOHA Y

## Overview

Apartment handover and rental move-in can place people under pressure to inspect, understand and document unfamiliar issues.

Vyuit helps non-professionals inspect an apartment room by room, document observations with photos and editable descriptions, and produce an organized PDF report.

Doori is the conversational companion developed during the hackathon. Its intended role is to explain terminology, guide the inspection one step at a time, answer related questions and suggest factual descriptions based on the user's observations.

The professional content and inspection sequence were defined by the creator, an architect. The solution does not replace a professional inspection or provide engineering certification.

## Start here: interactive experience demo

1. Download this repository and extract it if downloaded as a ZIP.
2. Open `demo.html` in a web browser.
3. Select **חדר רחצה 1** (Bathroom 1).
4. Follow the conversation, try different answers, edit a suggested finding and simulate saving it.
5. Continue to the summary.

The interface is in Hebrew. No installation, account or IBM credentials are needed for this demo.

**This is a scripted demonstration of the intended user experience.** The conversation and saving actions are simulated. It is not connected to watsonx Orchestrate or the app database. Demo findings exist only during the current page session and are lost when the page is reloaded.

The orange banner identifies the simulation.

## Existing Vyuit app

[Open the existing Vyuit app](https://vyuit.app)

The app is available through its existing pilot access process. Follow the on-screen access and sign-in instructions.

The creator checked access to the app before submission. The original app was developed in Lovable before the hackathon. Its source code and infrastructure are not included in this repository.

The original app's embedded chat widget was connected to the cloud agent during development. This integration is separate from the standalone scripted demo.

## Repository contents

| File or folder | Purpose |
|---|---|
| `demo.html` | Standalone demonstration of the intended experience |
| `doori_submission_agent.yaml` | Submission agent definition and conversation instructions |
| `doori_submission_kb.yaml` | Knowledge base configuration |
| `doori_app_guide.txt` | App guidance and the approved Bathroom 1 knowledge sample |
| `bob_sessions/` | Screenshots of project-related Bob task session summaries |
| `README.md` | Project explanation, setup instructions and limitations |

Keep the two YAML files and the knowledge document in the same directory.

## Development with IBM Bob

During the hackathon, I created a separate branch in the existing GitHub repository and connected Bob to the project.

I created an agent in IBM watsonx Orchestrate and used Build with Bob to develop Doori's role, conversational style, scope, response boundaries, agent configuration and knowledge files.

I defined the professional knowledge, inspection sequence and product decisions based on my architectural experience. Bob supported configuration, analysis, initial checks and preparation of the demonstration.

watsonx Orchestrate provides the runtime for the actual conversational agent and access to its knowledge base.

## Implementation status

The original agent and knowledge base were uploaded to the cloud. Conversations, knowledge retrieval and the embedded chat widget were tested during development.

Further local changes were made to the instructions, including the explicit sink and faucet sequence and the single-step guidance rule. Not all local updates were imported and verified in the cloud.

**The renamed submission configurations in this repository have not been imported or tested as a complete package.** They are provided for review and reproduction in the reviewer's own environment. Previous tests of the original components do not establish that this new package works unchanged.

The creator also opened and tried the standalone HTML demo.

## Optional: set up the actual agent

This section is for reviewers who want to import the configurations into their own IBM watsonx Orchestrate environment. It is not needed to run `demo.html`.

### Prerequisites

- Your own IBM watsonx Orchestrate cloud environment, with permission to import agents and knowledge bases.
- Your own service instance URL and API key.
- Python 3.12 and an isolated Python environment.
- IBM watsonx Orchestrate ADK CLI.

ADK version 2.16.1 was reported as used for the original project's successful imports. The project also had a local environment with version 2.19.1. Compatibility of the submission package across these versions has not been tested.

The configured chat and embedding models must be available in the reviewer's environment.

### 1. Install the CLI

In your activated Python environment:

```bash
python -m pip install ibm-watsonx-orchestrate==2.16.1
```

### 2. Configure your environment

Replace `YOUR_INSTANCE_URL` with your own service instance URL:

```bash
orchestrate env add -n doori-review -u "YOUR_INSTANCE_URL"
orchestrate env activate doori-review
orchestrate env list
```

Enter your own API key when prompted. Confirm that `doori-review` is the active environment.

Use your own review environment. Do not target the creator's active environment. Keep credentials out of repository files.

### 3. Import the knowledge base

Open a terminal in the directory containing the submission files and run:

```bash
orchestrate knowledge-bases import -f doori_submission_kb.yaml
orchestrate knowledge-bases status --name doori_submission_kb
```

Wait until the status indicates that the knowledge base is ready.

### 4. Import the agent

```bash
orchestrate agents import -f doori_submission_agent.yaml
```

Importing may update an existing component with the same name. The submission uses the separate names `doori_submission_agent` and `doori_submission_kb`.

If Hebrew text causes encoding errors on Windows PowerShell, the original project used this setting before importing:

```powershell
$env:PYTHONUTF8="1"
```

### 5. Open and test

Open `doori_submission_agent` in the watsonx Orchestrate builder preview. Check that its knowledge base is `doori_submission_kb` and that the detailed instructions were imported.

Start a conversation in Hebrew:

> בואו נתחיל לבדוק את חדר הרחצה שלי

Test whether the agent uses the knowledge content, waits for answers, handles a terminology question and returns to the inspection. These are intended behaviors, not guaranteed outcomes.

The imported agent is separate from `demo.html`. These instructions do not connect it to the existing Vyuit app.

### Setup references

- [Configure environments](https://developer.watson-orchestrate.ibm.com/environment/initiate_environment)
- [Import knowledge bases](https://developer.watson-orchestrate.ibm.com/knowledge_base/deploy_kb)
- [Check knowledge base status](https://developer.watson-orchestrate.ibm.com/knowledge_base/manage_kb)
- [Import agents](https://developer.watson-orchestrate.ibm.com/agents/import_agent)

Command syntax was checked against current documentation. This was not a fresh installation test or a complete validation against a version-specific 2.16.1 documentation snapshot.

## Known limitations and next steps

- Detailed professional guidance in the agent prototype is scoped to Bathroom 1.
- Consistently maintaining the inspection sequence remains unresolved.
- Two tools were planned following an IBM developer's recommendation: `get_next_item` to retrieve the next unanswered item, and `record_answer` to save an answer and mark it completed. They have not been implemented, imported or tested.
- Shared persistent storage for inspection progress remains an unresolved design requirement. The current configuration does not establish reliable progress retention between sessions.
- The agent does not write defects to the app database. Users manually copy suggested wording, edit it, add a photo and save through the existing app.
- Direct transfer into the defect form, draft preservation during that transfer and verification of separation between users' chat sessions remain follow-up tasks.
- Plan reading, image understanding, voice interaction and phone measurement are future capabilities.
- Initial visual design work on Doori has begun. Animation conveying waiting, explanation and progress is planned.

This submission presents the existing prototype, its limitations and a demonstration of the intended complete experience.