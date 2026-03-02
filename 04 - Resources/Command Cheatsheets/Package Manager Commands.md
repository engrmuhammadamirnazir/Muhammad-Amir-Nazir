---
type: cheatsheet
tags: [commands, npm, yarn, pnpm, package-manager, cheatsheet]
created: 2026-03-02
---

# Package Manager Commands (npm / yarn / pnpm)

---

## Side-by-Side Comparison

| Action | npm | yarn | pnpm |
|--------|-----|------|------|
| Initialize | `npm init -y` | `yarn init -y` | `pnpm init` |
| Install all | `npm install` | `yarn` | `pnpm install` |
| Clean install | `npm ci` | `yarn install --frozen-lockfile` | `pnpm install --frozen-lockfile` |
| Add package | `npm install pkg` | `yarn add pkg` | `pnpm add pkg` |
| Add dev dep | `npm install -D pkg` | `yarn add -D pkg` | `pnpm add -D pkg` |
| Add global | `npm install -g pkg` | `yarn global add pkg` | `pnpm add -g pkg` |
| Remove | `npm uninstall pkg` | `yarn remove pkg` | `pnpm remove pkg` |
| Update all | `npm update` | `yarn upgrade` | `pnpm update` |
| Update one | `npm update pkg` | `yarn upgrade pkg` | `pnpm update pkg` |
| Run script | `npm run dev` | `yarn dev` | `pnpm dev` |
| Run test | `npm test` | `yarn test` | `pnpm test` |
| Run build | `npm run build` | `yarn build` | `pnpm build` |
| Execute bin | `npx pkg` | `yarn dlx pkg` | `pnpm dlx pkg` |
| List deps | `npm list` | `yarn list` | `pnpm list` |
| Audit | `npm audit` | `yarn audit` | `pnpm audit` |
| Cache clean | `npm cache clean --force` | `yarn cache clean` | `pnpm store prune` |
| Publish | `npm publish` | `yarn publish` | `pnpm publish` |

---

## npm

```bash
npm init -y
```

```bash
npm install
```

```bash
npm ci
```
> Clean install from lockfile (for CI/CD)

```bash
npm install express
```

```bash
npm install -D nodemon
```

```bash
npm install -g typescript
```

```bash
npm uninstall express
```

```bash
npm update
```

```bash
npm run dev
```

```bash
npm run build
```

```bash
npm test
```

```bash
npm start
```

```bash
npx create-react-app myapp
```

```bash
npm list --depth=0
```

```bash
npm outdated
```

```bash
npm audit
```

```bash
npm audit fix
```

```bash
npm cache clean --force
```

```bash
npm link
```
> Link local package for development

```bash
npm pack
```
> Create tarball of package

```bash
npm version patch
```
> Bump version (patch/minor/major)

```bash
npm publish
```

```bash
npm login
```

```bash
npm whoami
```

---

## yarn

```bash
yarn init -y
```

```bash
yarn
```
> Install all dependencies

```bash
yarn install --frozen-lockfile
```

```bash
yarn add express
```

```bash
yarn add -D nodemon
```

```bash
yarn global add typescript
```

```bash
yarn remove express
```

```bash
yarn upgrade
```

```bash
yarn upgrade --interactive
```

```bash
yarn dev
```

```bash
yarn build
```

```bash
yarn dlx create-react-app myapp
```

```bash
yarn list --depth=0
```

```bash
yarn info package-name
```

```bash
yarn why package-name
```
> Why is this package installed

```bash
yarn cache clean
```

```bash
yarn link
```

```bash
yarn workspaces list
```

---

## pnpm

```bash
pnpm init
```

```bash
pnpm install
```

```bash
pnpm install --frozen-lockfile
```

```bash
pnpm add express
```

```bash
pnpm add -D nodemon
```

```bash
pnpm add -g typescript
```

```bash
pnpm remove express
```

```bash
pnpm update
```

```bash
pnpm dev
```

```bash
pnpm build
```

```bash
pnpm dlx create-react-app myapp
```

```bash
pnpm list --depth=0
```

```bash
pnpm why package-name
```

```bash
pnpm store prune
```
> Clean unused packages from store

```bash
pnpm store path
```

---

## package.json Scripts

```json
{
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start",
    "lint": "eslint .",
    "test": "jest",
    "test:watch": "jest --watch",
    "format": "prettier --write .",
    "typecheck": "tsc --noEmit"
  }
}
```

---

*See also: [[Python Commands]] | [[Linux & Bash Commands]]*
