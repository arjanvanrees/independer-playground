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

Figma MCP gives your coding assistant structured context about frames, components, variables, layout, and assets. This playground uses Figma's remote MCP server. It connects directly to Figma, works without the Figma desktop app, and uses links to identify files, frames, and layers.

### Install the Figma plugin in Codex

1. Open the Codex app.
2. Click **Plugins** in the upper-left corner.
3. Click **+** next to Figma.
4. Click **Install Figma**.
5. Complete the Figma authentication flow and click **Allow access**.
6. Start a new Codex task in this repository.

See [Figma's remote MCP installation guide](https://developers.figma.com/docs/figma-mcp-server/remote-server-installation/#codex) if the Codex interface or authentication flow has changed.

### Codex CLI setup

Codex CLI users can configure the same remote server manually:

```bash
codex mcp add figma --url https://mcp.figma.com/mcp
```

Complete the authentication flow when prompted, then start a new Codex session. You only need to configure the server once.

### Visual Studio Code with GitHub Copilot

You can also use this playground with Visual Studio Code, the GitHub Copilot extension, and Figma's MCP server.

#### Requirements

- a recent version of [Visual Studio Code](https://code.visualstudio.com/);
- the [GitHub Copilot extension](https://marketplace.visualstudio.com/items?itemName=GitHub.copilot);
- a GitHub account with Copilot access enabled;
- access to the Figma file you want to use;
- Agent mode enabled in Copilot Chat.

Sign in to GitHub from VS Code and confirm that Copilot Chat works before configuring Figma.

Colleagues who use Visual Studio Code with GitHub Copilot can install the community [MCP Figma Extension](https://marketplace.visualstudio.com/items?itemName=SethFord.mcp-figma-extension). It offers guided MCP configuration, connection controls, testing, and companion Figma plugin instructions. Follow its marketplace documentation because it is a separate workflow from the your coding assistant remote MCP setup above.

### Troubleshoot your coding assistant and Figma

- If Figma tools do not appear, confirm that the Figma plugin is installed in your coding assistant and start a new task.
- If authentication fails, reconnect the plugin and confirm that your Figma account can open the linked file.
- If the wrong design is returned, use **Copy link to selection** on the intended frame or layer and paste the new URL.
- If a large design is slow, link a smaller nested frame or individual component.
- If your coding assistant does not call Figma automatically, explicitly say “Use the Figma MCP tools” in the prompt.
- If your coding assistant proposes React or Tailwind, restate that the target is Nuxt with Vue 3, existing `Ind` components, and regular scoped CSS.

## Your first Figma-to-code experiment

Start small. A single component, form section, or mobile frame gives the agent clearer context than an entire application.

1. Run the playground with `npm run dev`.
2. In Figma, copy the link to the relevant frame or layer.
3. Open a your coding assistant task for this repository, paste the URL, and ask it to inspect the linked selection.
4. State the target file, framework, and constraints explicitly.
5. Ask your coding assistant to inspect the repository and design before making changes.
6. Let your coding assistant implement and verify the result.
7. Review the changed files and the page in the browser.
8. Continue with focused feedback rather than restarting with a broad prompt.

For example:

```text
Use the Figma MCP tools to inspect this frame: <FIGMA_URL>

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
Analyse this Figma frame using the Figma MCP tools: <FIGMA_URL>. Do not edit
files yet. Describe its structure, reusable design-system components, responsive
behavior, states, assets, and any details that are ambiguous or missing.
```

### Implement one section

```text
Implement only the form section from this Figma frame in app/pages/index.vue:
<FIGMA_URL>. Use the Figma MCP tools, existing Ind components and Vue conventions. Do not use
Tailwind classes in application code. Add minimal scoped CSS where necessary.
Run the build when finished.
```

### Compare code with Figma

```text
Compare the current playground page with this Figma frame: <FIGMA_URL>.
Inspect both before editing. List the visual and behavioral differences, then
fix the meaningful differences without replacing existing Ind components.
Verify the result in the browser at mobile and desktop widths.
```

### Ask for a code review

```text
Review the current changes as production code. Check component reuse,
accessibility, responsive behavior, duplicated CSS, TypeScript issues, and
whether the implementation matches the linked Figma frame. Do not change
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

- a link to the smallest relevant Figma frame or layer;
- the page or component that may be changed;
- whether the task is analysis, implementation, or review;
- required states such as loading, disabled, validation, empty, and error;
- expected mobile and desktop behavior;
- which existing components must be reused;
- explicit boundaries, such as no new dependencies or no design-system edits;
- the verification you expect, such as a build or browser comparison.

Figma MCP supplies design context; your coding assistant still writes and adapts the final code. It does not automatically understand team conventions unless they are present in the repository, expressed in the prompt, or mapped through [Figma Code Connect](https://developers.figma.com/docs/figma-mcp-server/code-connect-integration/).

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

### Your coding assistant cannot access the Figma file

- Confirm that the Figma plugin is installed and authenticated in your coding assistant.
- Confirm that your Figma account has access to the linked file.
- Paste a link to the specific frame or layer rather than only a screenshot.
- Start a new task after installing or reconnecting the plugin.

### The wrong frame is used

- Right-click the intended frame or layer in Figma and choose **Copy link to selection**.
- Confirm that the pasted URL identifies the intended node.
- Use a smaller nested frame when the page is large or complex.

### The output looks like React or Tailwind

Figma context can resemble web or React code, but it is not the required output format. Tell your coding assistant explicitly to implement the result in Nuxt and Vue 3, reuse `Ind` components, and avoid Tailwind in playground files.

### The result does not match the design system

- Ask your coding assistant to inspect existing `Ind` component APIs before implementing.
- Mention the exact component you expect it to reuse.
- Check whether the Figma component is mapped through Code Connect.
- Fix reusable component behavior in the design-system repository, not with playground-specific overrides.

### The request is slow or returns too much context

Link a smaller frame or individual component. Large pages and broad requests consume more context and make it harder for the agent to identify the intended hierarchy.

## Using design-system components

Independer's design system is registered as a Nuxt module in `nuxt.config.ts`:

```ts
export default defineNuxtConfig({
  modules: ["independer-design-system"],
});
```

The module provides the following design tokens and utilities, (mostly) aligned with the DS2 version of the design system in Figma.

- Components and their states
- Colours
- Typography
- Breakpoints
- Shadows

Its components are auto-imported with the `Ind` prefix and can be used without explicit imports:

```vue
<template>
  <IndFormInput v-model="name" name="name" label="Naam" />
  <IndButton label="Verder" tone="filled-yellow" />
</template>
```

By default the module registers the following components:

`IndButton`, `IndButtonToggle`, `IndDialog`, `IndModal`, `IndPill`, `IndFormCheckbox`, `IndFormInput`, `IndFormLabel`, `IndFormRadio`, `IndFormRow`, `IndFormFieldset`, `IndFormSelect`, `IndFormTabs`, and `IndFormTextarea`.

For more information about `independer-design-system` see its [documentation](https://www.github.com/arjanvanrees/independer-design-system).

When a component style is missing, ask the maintainer of `independer-design-system` to add it. Do not create playground-specific overrides.

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