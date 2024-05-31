# goldsky-deploy

A Github Action for deploying subgraphs to Goldsky. 

# How to use

See the Goldsky [ethereum-blocks](https://github.com/goldsky-io/ethereum-blocks/blob/master/.github/workflows/main.yaml)
repo for an example.

You will need to get an API key from https://app.goldsky.com and set that up as a [Github Repository Secret](https://docs.github.com/en/actions/security-guides/using-secrets-in-github-actions#creating-secrets-for-a-repository).

In order to deploy your subgraph to Goldsky you will have to first build it. Here's an example of how to do that:

```yaml
      - name: Install node
        uses: actions/setup-node@v4
        with:
          node-version: 18
          cache: "npm"
      - name: Install dependencies
        run: npm install
      - name: codegen
        run: npm run codegen
      - name: build
        run: npm run build
```

Once your subgraph is built, you can then use the `goldsky-deploy` action to run the deployment.

```yaml
      - name: Deploy Subgraph
        uses: goldsky-io/goldsky-deploy@latest
        with:
          api_key: ${{ secrets.GOLDSKY_API_KEY }}
          subgraph_name: "ethereum-blocks"
          path: "."
```

There are 3 parameters to the action:

1. **api_key**: Required. The API key you got from Goldsky. This is best stored as a repository secret and referenced as `${{ secrets.GOLDSKY_API_KEY }}`.
2. **subgraph_name**: Required. The name of the subgraph you are deploying. This will be displayed in the Goldsky UI.
3. **path**: Optional. The path to the subgraph you are deploying - this can either point directly to the `build` directory or to the parent of the `build` directory. Defaults to `.`.

Note that the version of the subgraph that's deployed will be the short SHA of the commit from which the action was invoked.
