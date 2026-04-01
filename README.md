# imc-2026

## Kong API Platform Interactive Demo

This project includes a hands-on Jupyter Notebook (`kong_api_platform.ipynb`) designed to demonstrate the implementation of a modern API Gateway strategy using **Kong Konnect**. 

The notebook follows a "Construction" narrative, guiding users step-by-step as they transform a vulnerable, "Unprotected API" into a "Secure, AI-Ready Platform".

**Note:** The backend service we are proxying throughout this tutorial is the [Kong-Air-Flights-API](https://portal.kongair.dev/apis/flights-api/versions/74c7c2b2-fdaf-4ab3-9579-5f636eaa7370/operations/get-flights) (`api.kongair.dev`).

### What the Notebook Does

The notebook acts as an interactive lab environment. Through a sequence of Python and Bash cells, you will interact with the Kong `deck` CLI and the Kong Konnect API to incrementally build your gateway configuration:

1. **Clean Slate Initialization**: Wipes the Gateway to ensure a fresh environment.
2. **Step 1: Create a Service and Route**: Connects a public proxy endpoint to an unprotected backend (`api.kongair.dev`).
3. **Step 2: Create a Consumer and Generate a Key**: Uses Python's `secrets` module to dynamically generate a secure API key for a new Consumer and saves it to your environment variables.
4. **Step 3: Identity Guard (key-auth)**: Applies the `key-auth` plugin to block anonymous access.
5. **Step 4: Traffic Control (rate-limiting)**: Adds the `rate-limiting` plugin to prevent service abuse (DDoS), allowing only 10 requests per minute.
6. **Step 5: AI Transformation (ai-mcp-proxy)**: Implements the Model Context Protocol (MCP) by attaching the `ai-mcp-proxy` plugin. This transforms standard REST responses into a semantic toolset ("Get Flights", "Get Routes") that AI Agents can consume natively.

Each step includes a **Verification** block where a shell script loops or triggers a curl command to prove the new gateway behavior works (e.g., triggering a `429 Too Many Requests` error).

Finally, the notebook concludes by launching the official **MCP Inspector** web UI, dynamically passing your generated API key and Serverless URL, so you can interactively test your new AI tools in a browser!

### Prerequisites
Before running the notebook, ensure you have the following installed:
1. [Kong Konnect Serverless Gateway](https://developer.konghq.com/serverless-gateways/): You must have a Serverless Gateway provisioned and know its Control Plane Name and Proxy URL.
2. [decK](https://developer.konghq.com/deck/): The declarative configuration tool for Kong.
3. [Python](https://www.python.org/) and a Jupyter environment (like JupyterLab or the VS Code Jupyter extension).
4. The `python-dotenv` package:
   ```bash
   pip install python-dotenv
   ```

### Configuration Setup
The notebook relies on local configuration files to run. You must create these in the same directory as the notebook before starting:

1. Copy the `.deck.yaml.template` file to `.deck.yaml` for decK authentication (see [docs](https://developer.konghq.com/deck/configuration/#configuration-file)):
   ```bash
   cp .deck.yaml.template .deck.yaml
   ```
   Open `.deck.yaml` and update the values with your actual Konnect credentials:
   - `konnect-token`: Create a Personal Access Token (PAT) by following the instructions [here](https://developer.konghq.com/konnect-api/#personal-access-tokens).
   - `konnect-control-plane-name`: This is the `Name` property displayed in the Gateway Manager overview for your specific Control Plane.
   - `konnect-addr`: Keep this as `https://eu.api.konghq.com` unless your Control Plane is hosted in a different region.

2. Copy the `.env.template` file to `.env` for your environment variables:
   ```bash
   cp .env.template .env
   ```
   Open `.env` and replace the `SERVERLESS_URL` value with the actual Proxy URL found in your Konnect control plane view. Note that `CONSUMER_API_KEY` will be generated and set automatically when you reach Step 3 in the notebook.

### Running the Notebook
1. Open `kong_api_platform.ipynb` in your preferred Jupyter environment (e.g., VS Code).
2. Select your Python Kernel.
3. Run the notebook sequentially from top to bottom by executing each cell (using `Shift + Enter` or the "Run All" button in your editor).
4. Read the markdown context in each step to follow the "Construction" narrative as you build out your secure API platform.