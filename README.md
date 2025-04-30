# Import Workspace Configuration

This action imports configuration to your Tonic Structural workspace.

## Inputs

- `api_key` (required): Structural API key with access to the workspace
- `workspace_id` (required): ID of the Structural workspace to import to
- `config_path` (required): Path to the workspace configuration file to import
- `api_url` (optional): Base URL for Structural API, defaults to 'https://app.tonic.ai'

## Outputs

- `result`: Result message from the import operation

## Example Usage

### Basic Usage

```yaml
steps:
  - name: Import Structural workspace
    id: import
    uses: TonicAI/structural-import-workspace@v1
    with:
      api_key: ${{ secrets.STRUCTURAL_API_KEY }}
      workspace_id: ${{ vars.STRUCTURAL_WORKSPACE_ID }}
      config_path: './workspace-config.json'
```

### Complete Workflow Example

```yaml
name: Import Structural Workspace

on:
  workflow_dispatch:
    inputs:
      workspace_id:
        description: 'ID of the Structural workspace to import to'
        required: true
        type: string
      config-file:
        description: 'Path to configuration file (relative to repository root)'
        required: true
        default: 'exports/workspace-config.json'
        type: string

jobs:
  import:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Import Structural workspace
        id: import
        uses: TonicAI/structural-import-workspace@v1
        with:
          api_key: ${{ secrets.STRUCTURAL_API_KEY }}
          workspace_id: ${{ github.event.inputs.workspace_id }}
          config_path: ${{ github.event.inputs.config-file }}

      - name: Output result
        run: echo "Import result: ${{ steps.import.outputs.result }}"
```

## Security

This action requires a Structural API key with permissions to import workspace configuration. Store this key as a secret in your GitHub repository and reference it as shown in the example workflow.