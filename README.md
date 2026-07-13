# Independer Design System Playground

A small Nuxt application for experimenting with AI-assisted development using Figma MCP and components from the [`independer-design-system`](https://www.npmjs.com/package/independer-design-system) package.

This repository is intended as a safe place for colleagues to try vibe coding: describe an outcome, give the coding agent relevant Figma context, review what it proposes, and improve the result through conversation. It is a learning environment, not a shortcut around code review, accessibility, testing, or design-system conventions.

## Requirements

- Node.js 20 or newer
- npm

## Setup

Install the dependencies:

```bash
npm install
```

Start the development server:

```bash
npm run dev
```

The playground is then available at [http://localhost:3000](http://localhost:3000).

## Connect Figma to your coding assistant

Figma MCP gives your coding assistant structured context about frames, components, variables, layout, and assets. This playground uses the desktop MCP server, which reads the current selection from the Figma desktop app.

Before configuring your coding assistant:

1. Install or update the [Figma desktop app](https://www.figma.com/downloads/).
2. Open a Figma Design file.
3. Enter Dev Mode with `Shift+D`.
4. Enable the desktop MCP server from the MCP section of the inspect panel.
5. Keep Figma open while working. The server runs at `http://127.0.0.1:3845/mcp`.

See [Figma's desktop MCP instructions](https://developers.figma.com/docs/figma-mcp-server/local-server-installation/) if the controls or connection steps have changed.

### Codex

Connect Codex to the running desktop server:

```bash
codex mcp add figma --url http://127.0.0.1:3845/mcp
```

Restart Codex if the newly configured tools do not appear. Select the intended frame or layer in Figma before prompting Codex.

### Visual Studio Code with GitHub Copilot

You can also use this playground with Visual Studio Code, the GitHub Copilot extension, and Figma's desktop MCP server.

#### Requirements

- a recent version of [Visual Studio Code](https://code.visualstudio.com/);
- the [GitHub Copilot extension](https://marketplace.visualstudio.com/items?itemName=GitHub.copilot);
- a GitHub account with Copilot access enabled;
- access to the Figma file you want to use;
- Agent mode enabled in Copilot Chat.

Sign in to GitHub from VS Code and confirm that Copilot Chat works before configuring Figma.

#### Connect the desktop Figma MCP server

1. Open this repository as a folder in VS Code.
2. Confirm that the desktop MCP server is enabled in Figma Dev Mode.
3. Open the Command Palette with `Cmd+Shift+P` on macOS or `Ctrl+Shift+P` on Windows and Linux.
4. Run **MCP: Add Server**.
5. Choose **HTTP**.
6. Enter `http://127.0.0.1:3845/mcp`.
7. Name the server `figma` and add it to this workspace.

The resulting workspace configuration should look similar to:

```json
{
  "inputs": [],
  "servers": {
    "figma": {
      "type": "http",
      "url": "http://127.0.0.1:3845/mcp"
    }
  }
}
```

8. Click **Start** above the Figma server in the editor.
9. Open Copilot Chat and switch to **Agent** mode.
10. Open the tools picker and confirm that Figma tools are available.

The workspace configuration applies only to this repository. If you want to use Figma MCP in every VS Code workspace, run **MCP: Open User Configuration** instead.

#### Use Figma context in Copilot Chat

The desktop server is selection-based. Select the smallest relevant frame or layer in the Figma desktop app, then prompt Copilot:

```text
Use the Figma MCP tools to inspect my current Figma selection.

Implement it in this Nuxt and Vue 3 playground. Inspect the repository before
editing. Reuse existing Ind components, do not add or use Tailwind in
application code, and use minimal scoped CSS for layout. Preserve accessible
labels, keyboard behavior and responsive behavior. Run npm run build and
summarize the changed files when finished.
```

Keep the relevant page open in the editor when prompting. You can also attach or mention specific workspace files in Copilot Chat to narrow the task, for example `app/pages/index.vue`.

#### Troubleshoot VS Code and Copilot

- If **MCP** commands are missing, update VS Code and the GitHub Copilot extension.
- If Figma tools do not appear, start the server from the MCP configuration and reopen Copilot Chat.
- If Copilot does not call Figma automatically, say “Use the Figma MCP tools to inspect my current selection.”
- If the desktop server cannot connect, confirm that Figma desktop is open and that its MCP server is enabled.
- If Copilot proposes React or Tailwind, restate that the target is Nuxt with Vue 3, existing `Ind` components, and regular scoped CSS.

## Your first Figma-to-code experiment

Start small. A single component, form section, or mobile frame gives the agent clearer context than an entire application.

1. Run the playground with `npm run dev`.
2. In the Figma desktop app, select the relevant frame or layer.
3. Open a Codex task for this repository and ask it to inspect your current Figma selection.
4. State the target file, framework, and constraints explicitly.
5. Ask Codex to inspect the repository and design before making changes.
6. Let Codex implement and verify the result.
7. Review the changed files and the page in the browser.
8. Continue with focused feedback rather than restarting with a broad prompt.

For example:

```text
Use the Figma MCP tools to inspect my current Figma selection.

Implement it on the playground page using Nuxt, Vue 3 and the existing
Independer Design System components. First inspect the repository and Figma
context. Reuse Ind components wherever possible. Do not add Tailwind to the
playground and do not modify the design-system package. Use scoped CSS only
for layout that the design system does not provide. Preserve accessibility,
then run the production build and summarize the files you changed.
```

## Prompt recipes

### Understand a design before coding

```text
Analyse my current Figma selection using the Figma MCP tools. Do not edit files
yet. Describe its structure, reusable design-system components, responsive
behavior, states, assets, and any details that are ambiguous or missing.
```

### Implement one section

```text
Implement only the selected Figma form section in app/pages/index.vue. Use
the Figma MCP tools, existing Ind components and Vue conventions. Do not use
Tailwind classes in application code. Add minimal scoped CSS where necessary.
Run the build when finished.
```

### Compare code with Figma

```text
Compare the current playground page with my current Figma selection.
Inspect both before editing. List the visual and behavioral differences, then
fix the meaningful differences without replacing existing Ind components.
Verify the result in the browser at mobile and desktop widths.
```

### Ask for a code review

```text
Review the current changes as production code. Check component reuse,
accessibility, responsive behavior, duplicated CSS, TypeScript issues, and
whether the implementation matches the selected Figma frame. Do not change
anything; report findings by severity with file and line references.
```

### Iterate with precise feedback

```text
Keep the current structure. Make the form narrower on desktop, preserve the
mobile layout, and match the spacing shown in the Figma frame. Do not change
the button or input APIs. Verify the page again when finished.
```

## Give the agent useful context

Good results depend more on clear context than on a long prompt. Include:

- the smallest relevant frame or layer selected in the Figma desktop app;
- the page or component that may be changed;
- whether the task is analysis, implementation, or review;
- required states such as loading, disabled, validation, empty, and error;
- expected mobile and desktop behavior;
- which existing components must be reused;
- explicit boundaries, such as no new dependencies or no design-system edits;
- the verification you expect, such as a build or browser comparison.

Figma MCP supplies design context; Codex still writes and adapts the final code. It does not automatically understand team conventions unless they are present in the repository, expressed in the prompt, or mapped through [Figma Code Connect](https://developers.figma.com/docs/figma-mcp-server/code-connect-integration/).

## Vibe coding guardrails

Treat generated code like code written by a colleague:

- start each experiment on a separate Git branch;
- check `git diff` before accepting or committing changes;
- never paste credentials, customer data, or other sensitive information into a prompt;
- do not let an agent publish, deploy, push, or open a pull request unless that action is intentional;
- prefer existing `Ind` components over recreated controls;
- check keyboard navigation, labels, focus states, contrast, and validation messages;
- verify responsive behavior rather than judging one screenshot;
- run `npm run build` before sharing the result;
- ask a human colleague to review changes intended for production.

If the agent starts changing too much, stop it and narrow the task. A good correction is: “Keep the existing implementation, change only `app/pages/index.vue`, and explain any additional change before making it.”

## Troubleshooting Figma MCP

### Codex cannot access the Figma file

- Confirm that the Figma desktop app is open.
- Confirm that the desktop MCP server is enabled in Dev Mode.
- Confirm that your coding assistant is connected to `http://127.0.0.1:3845/mcp`.
- Restart the coding assistant after adding or reconnecting the server.

### The wrong frame is used

- Select the intended frame or layer in the Figma desktop app before prompting.
- Confirm that only the relevant layer or frame is selected.
- Use a smaller nested frame when the page is large or complex.

### The output looks like React or Tailwind

Figma context can resemble web or React code, but it is not the required output format. Tell Codex explicitly to implement the result in Nuxt and Vue 3, reuse `Ind` components, and avoid Tailwind in playground files.

### The result does not match the design system

- Ask Codex to inspect existing `Ind` component APIs before implementing.
- Mention the exact component you expect it to reuse.
- Check whether the Figma component is mapped through Code Connect.
- Fix reusable component behavior in the design-system repository, not with playground-specific overrides.

### The request is slow or returns too much context

Select a smaller frame or individual component. Large pages and broad requests consume more context and make it harder for the agent to identify the intended hierarchy.

## Using design-system components

The design system is registered as a Nuxt module in `nuxt.config.ts`:

```ts
export default defineNuxtConfig({
  modules: ["independer-design-system"],
});
```

Its components are auto-imported with the `Ind` prefix and can be used without explicit imports:

```vue
<template>
  <IndFormInput v-model="name" name="name" label="Naam" />
  <IndButton label="Verder" tone="filled-yellow" />
</template>
```

## Styling boundary

Tailwind CSS is an implementation detail of the design system. The design-system module owns its Tailwind configuration, theme, generated utilities, and component source discovery.

The playground should therefore:

- not register `@tailwindcss/vite`;
- not import `tailwindcss` from application CSS;
- not depend directly on `tailwindcss` or `@tailwindcss/vite`;
- not use Tailwind utility classes in application templates;
- use design-system components or regular scoped CSS for playground layout and presentation.

The design-system stylesheet should restrict class discovery to its own component sources:

```css
@import "tailwindcss" source(none);
@source "../components/**/*.{vue,js,ts}";
```

This keeps Tailwind out of the public contract and prevents consuming applications from accidentally depending on design-system tokens or utility classes.

For playground-specific styles, use regular Vue scoped CSS:

```vue
<template>
  <main class="playground">
    <IndButton label="Verder" tone="filled-yellow" />
  </main>
</template>

<style scoped>
.playground {
  max-width: 70rem;
  margin-inline: auto;
  padding: 2rem 1rem;
}
</style>
```

## Production build

Create a production build:

```bash
npm run build
```

Preview that build locally:

```bash
npm run preview
```

Generate a static build when required:

```bash
npm run generate
```

## Updating the design system

Install the latest compatible release:

```bash
npm install independer-design-system@latest
```

After updating, run a production build and check the component examples in the browser:

```bash
npm run build
```

When a component style is missing, fix its source discovery or class definition in `independer-design-system` rather than adding a Tailwind safelist to this playground.
