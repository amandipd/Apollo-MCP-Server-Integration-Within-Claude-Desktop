# MCP Server Integration Within Claude Desktop Instructions

## Creating a Dummy Apollo Graph and Obtaining Credentials
1. Create an empty directory in your preferred location for the Apollo Graph project
2. Open your terminal and navigate to the root of this directory
3. Run rover init to initialize a new Apollo Graph
4. Follow the prompts that appear in the terminal to set up your graph
5. After successful creation, your APOLLO_GRAPH_REF and APOLLO_KEY credentials will be displayed - copy and save these values securely
6. In your project directory, locate the supergraph.yaml file and replace its entire contents with:
   ```bash
   federation_version: =2.10.0
   subgraphs:
     listings:
       routing_url: https://rt-airlock-subgraphs-listings.herokuapp.com/
       schema:
         subgraph_url: https://rt-airlock-subgraphs-listings.herokuapp.com/	
   ```
   - This configuration uses sample APIs that provide spaceship data from the Apollo MCP tutorial

  ## Cloning and Running the Apollo Runtime Container
  1. Clone the Apollo Runtime repository from: https://github.com/apollographql/apollo-runtime?tab=readme-ov-file
  2. Navigate to the root directory of the cloned repository and verify that Docker Desktop is running
  3. Execute the following command, substituting your actual credentials for INSERT_HERE:
  ```bash
  docker run \
--env APOLLO_GRAPH_REF="INSERT_HERE" \
--env APOLLO_KEY="INSERT_HERE" \
--env MCP_ENABLE=1 \
--env MCP_UPLINK=1 \
--rm \
-p 4000:4000 \
-p 5050:5000 \
ghcr.io/apollographql/apollo-runtime:latest
  ```
4. Upon successful execution:
   - Visit localhost:4000 to access the GraphQL API endpoint
   - Visit studio.apollographql.com/sandbox?endpoint=http://localhost:4000 to access Apollo Studio Explorer for creating and testing queries
   
## Running the MCP Server and Connecting to Claude Desktop
1. Within your Apollo graph project, create a new folder named operations
2. Inside the operations folder, create a file called listings.graphql and add this sample query:
```bash
  query ExampleQuery {
  products {
    name
    description
    id
  }
}
  ```
3. Start the MCP Server by running this command from your project root:
```bash
rover dev --supergraph-config ./supergraph.yaml `
  --mcp `
  --mcp-operations ./operations/listings.graphql
```
4. Launch Claude Desktop and access Settings from the application menu
5. Navigate to the Developer section and click "Edit Config"
6. Update the claude_desktop_config file with this configuration:
```bash
{
  "mcpServers": {
    "airlock": {
      "command": "npx",
      "args": [
        "mcp-remote",
        "http://127.0.0.1:5000/mcp",
        "--transport",
        "http-first"
      ]
    }
  }
}
```
7. Restart Claude Desktop to save these changes
8. Return to Claude Desktop and enable the new tool that appears in the interface. You can now ask Claude to execute API queries and receive results from your Apollo Graph
