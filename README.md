# Local npm Registry

This workspace uses [Verdaccio](https://verdaccio.org/) as a local open-source npm registry.

Registry URL: `http://127.0.0.1:4874/`

## Start the registry

```powershell
npm.cmd install
npm.cmd run start
```

## Publish the shared package

From [oneAutomation](c:\Projects\oneQA\oneAutomation):

```powershell
npm.cmd run build
npm.cmd run publish:local
```

## Install from the starter repo

From [oneAutomation-starter-repo](c:\Projects\oneQA\oneAutomation-starter-repo):

```powershell
npm.cmd install
```

The repo-level `.npmrc` files already point both projects at the local registry.

---

## Multiple `.npmrc` Configuration (Local vs Production)

You can maintain separate `.npmrc` files for local development and production registries, keeping the default `.npmrc` unchanged.

### File layout (in each project)

| File | Purpose | Used by |
|------|---------|---------|
| `.npmrc` | Default config for `npm install` (points to npmjs.org) | `npm install` (automatic) |
| `.npmrc.local` | Local Verdaccio registry | `--userconfig .npmrc.local` |
| `.npmrc.prod` | Production registry (e.g. GitHub Packages, JFrog) | `--userconfig .npmrc.prod` |

> **Note:** `npm install` and all npm commands **always** read the default `.npmrc` file.
> The named files (`.npmrc.local`, `.npmrc.prod`) are **only** used when explicitly passed via `--userconfig`.

### Example `.npmrc.local`

```ini
registry=http://127.0.0.1:4874/
//127.0.0.1:4874/:_authToken="your-local-token"
```

### Example `.npmrc.prod`

```ini
registry=https://your-prod-registry.com/
//your-prod-registry.com/:_authToken="${PROD_NPM_TOKEN}"
```

### Publishing

In `package.json` scripts:

```json
{
  "scripts": {
    "publish:local": "npm publish --userconfig .npmrc.local",
    "publish:prod":  "npm publish --userconfig .npmrc.prod"
  }
}
```

```powershell
npm.cmd run publish:local   # → pushes to Verdaccio
npm.cmd run publish:prod    # → pushes to production registry
```

### Installing (consumer repos)

```powershell
npm install                              # uses default .npmrc (npmjs.org)
npm install --userconfig .npmrc.local    # pulls from Verdaccio
npm install --userconfig .npmrc.prod     # pulls from production registry
```

### Best practices

- Keep the default `.npmrc` pointing to `https://registry.npmjs.org/` so plain `npm install` always works.
- Add `.npmrc.local` to `.gitignore` if it contains local-only tokens.
- Use environment variables (`${PROD_NPM_TOKEN}`) in `.npmrc.prod` for CI/CD secrets.
