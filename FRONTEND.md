You are developing frontend code for this project.

First, inspect the existing frontend before writing code:
- Identify the framework, routing style, styling system, component library, icon library, data-fetching pattern, form pattern, and folder structure.
- Match existing conventions instead of introducing new libraries or visual systems.
- Reuse existing components, hooks, utilities, tokens, and layout patterns before creating new ones.

Implementation rules:
- Use TypeScript types where the project supports them.
- Keep client/browser-only code isolated to client components or browser-safe files.
- Put reusable UI in the project’s existing components directory.
- Keep route/page files focused on composition; move repeated UI and logic into components/hooks.
- Prefer small, explicit components over clever abstractions.

Styling:
- Use the project’s design tokens, theme variables, utility classes, or design system.
- Avoid hardcoded colors, spacing, shadows, and font sizes unless the project already does so.
- Preserve dark mode, responsive behavior, and existing brand style.
- Match the project’s density: dashboards should be efficient and scannable; marketing pages can be more expressive.
- Ensure text never overflows awkwardly on mobile or narrow containers.

Accessibility:
- Use semantic elements: `<button>`, `<a>`, `<form>`, `<label>`, `<nav>`, `<main>`, etc.
- Do not use `div onClick` for interactive controls.
- Every interactive element needs keyboard access and a visible focus state.
- Icon-only controls need `aria-label`; decorative icons should be `aria-hidden`.
- Forms need labels, correct input types, autocomplete where appropriate, validation messages, and `aria-invalid`.
- Use live regions or alerts for async/form status messages.

State handling:
- Every async UI should handle loading, empty, error, and success states.
- Disable submit buttons during pending actions.
- Show actionable recovery paths for errors.
- Avoid layout shift by reserving space or using skeletons/placeholders.
- Keep optimistic updates conservative unless the existing app already uses them.

Data and validation:
- Use the project’s existing API client, query library, schema validators, and error model.
- Validate user input before submitting.
- Parse and type-check API responses when the project has schemas or contracts.
- Keep data-fetching logic in hooks/services if that is the local pattern.

Interaction and polish:
- Prefer simple, fast transitions for hover/focus/open/close states.
- Respect reduced-motion preferences for nonessential animation.
- Keep copy concise, specific, and action-oriented.
- Use icons consistently with the existing icon set.
- Preserve hit targets large enough for touch.

Before finishing:
- Run the relevant formatter, linter, typecheck, tests, or build command.
- Check at least mobile, tablet, and desktop widths for layout issues.
- Verify keyboard navigation for new interactive flows.
- Do not leave placeholder text, unreachable buttons, fake links, or half-wired UI unless explicitly requested.
- Mention any verification that could not be run.
