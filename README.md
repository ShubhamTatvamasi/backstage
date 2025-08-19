# [Backstage](https://backstage.io)

This is your newly scaffolded Backstage App, Good Luck!

To start the app, run:

```sh
yarn install
yarn start
```

---

### Setup

Create a new backstage app:
```bash
npx @backstage/create-app@latest
```

Start backstage:
```bash
yarn start
```

In the backend directory, add the GitHub auth provider:
```bash
yarn --cwd packages/backend add @backstage/plugin-auth-backend-module-github-provider
```

---

Reference:

https://github.com/backstage/backstage/blob/master/packages/catalog-model/examples/acme/team-a-group.yaml

Install mkdocs:
```bash
pipx install mkdocs-techdocs-core --include-deps --force
```
