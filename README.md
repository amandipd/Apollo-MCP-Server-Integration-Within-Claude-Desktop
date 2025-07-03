# MCP Server Integration Within Claude Desktop Instructions

## Creating a Dummy Apollo Graph and Obtaining Credentials
1. Create an empty directory in an ideal location for the Apollo Graph
2. Navigate to the root of the directory with the command line
3. Once in the root of the directory, run “rover init” to initialize an Apollo Graph
4. Follow the instructions that pop up in the terminal when creating the graph
5. Once the graph is created, your APOLLO_GRAPH_REF and APOLLO_KEY should pop up; save these credentials 
6. Inside the codebase, replace the contents of the supergraph.yaml file with the following:
   ```bash
   federation_version: =2.10.0
   subgraphs:
     listings:
       routing_url: https://rt-airlock-subgraphs-listings.herokuapp.com/
       schema:
         subgraph_url: https://rt-airlock-subgraphs-listings.herokuapp.com/	
   ```
   - These are dummy APIs that retrieve data about spaceships as used in the Apollo MCP tutorial

  ## Cloning and Running the Apollo Runtime Container
  1. Clone the following repository: https://github.com/apollographql/apollo-runtime?tab=readme-ov-file
  2. Navigate to the root of the newly cloned repository and ensure the Docker daemon is running (Docker Desktop)
  3. Run the command below, replacing the INSERT_HERE in the APOLLO_GRAPH_REF and APOLLO_KEY with your respective keys
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
4. If the command is successful:
   - Navigating to localhost:4000 should provide you with the GraphQL API Endpoint
   - Navigating to studio.apollographql.com/sandbox?endpoint=httpL//localhost:4000 should provide you with the Explorer where you can create and test      sample queries
   
