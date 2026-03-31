# Local npm Registry

This workspace uses Verdaccio as a local open-source npm registry.

Registry URL:

`http://127.0.0.1:4874/`

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
