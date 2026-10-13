# Java + Next.js Dev Container

Project-independent development environment for Java / Spring Boot (Maven) backends and Node.js / Next.js frontends.

## Included

- Java 25 JDK and Maven (installed by the Dev Containers Java feature)
- Node.js 24 LTS and npm (installed by the Node feature)
- Git, GitHub CLI, zsh, and VS Code extensions for Java, Spring Boot, TypeScript/JavaScript, and formatting
- Oh My Zsh with autosuggestions, syntax highlighting, the `fred` theme, and shared `.zshrc` / `.vimrc` dotfiles (same setup script as the other dev containers in this repository)
- Forwarding for Next.js (`3000`) and Spring Boot (`8080`)
- Non-root `vscode` user and persistent Maven (`~/.m2`) and npm (`~/.npm`) caches

No application code, dependency manifests, database, or framework-specific project configuration is included. Versions of Maven dependencies and npm packages belong to each consuming project.

## Use in a new or existing project

1. Copy **only** the `.devcontainer/` directory into the root of your own repository (the directory you open in VS Code).
2. Open that repository in VS Code with the Dev Containers extension, then run **Dev Containers: Reopen in Container**.
3. Create or open your Spring Boot/Maven and Next.js projects as usual. The repository is mounted at `/workspace`.

During container creation, `.devcontainer/setup-shell.sh` configures Oh My Zsh, plugins, the `fred` theme, and the shared shell/Vim configuration. The script fetches those files from GitHub and requires network access. As with the other templates, it overwrites the container user's `~/.zshrc` and `~/.vimrc`. It can be rerun to update the cloned repositories and dotfiles.

For example, after the consuming project has been created:

```sh
# From the Maven project directory; prefer the project's Maven Wrapper if available
./mvnw spring-boot:run  # or: mvn spring-boot:run

# From the Next.js project directory, in a separate terminal
npm ci
npm run dev
```

Port forwarding is configured, but neither server is started automatically. If a project requires access from outside the container's loopback interface, configure its dev server to listen on `0.0.0.0` (for Next.js, `npm run dev -- --hostname 0.0.0.0`).

## Adapting the template

- Extend `.devcontainer/docker-compose.yml` with PostgreSQL, Redis, RabbitMQ, or other services when the project actually needs them.
- Change the Java image tag and/or Node feature version to match project requirements.
- Project dependencies are installed on demand, not during container creation. Maven and npm caches use named Docker volumes, so they survive container rebuilds. If you switch projects or dependencies, use your project's normal clean/restore commands as appropriate.
- The shell customization is intentionally identical to the existing Prolog, Python, and Scala templates. Modify or omit `setup-shell.sh` and its `postCreateCommand` invocation if a consuming project needs different dotfiles.
