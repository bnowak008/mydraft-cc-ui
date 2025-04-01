# [Feature] Decouple App Theme and Design Theme

## Description
This todo file outlines the steps required to separate the control of the application's UI theme (App Theme: sidebars, toolbars, menus) from the theme applied to the elements on the design canvas (Design Theme: shape fills, strokes, text colors, canvas background). This addresses feedback regarding the mixing of these two concepts and allows users to, for example, use the app in dark mode while designing a light-mode wireframe, or vice-versa.

We will introduce a new state management mechanism and UI control specifically for the Design Theme, distinct from the existing App Theme controls.

## Memory Section Guidelines
- ALWAYS maintain the Memory section in each todo file
- UPDATE the Memory section when:
  - Making significant implementation decisions
  - Overcoming technical challenges
  - Discovering future considerations
  - Gaining new technical insights
  - Adding or modifying dependencies
- ENSURE the Memory section:
  - Provides clear context for future development
  - Documents rationale behind decisions
  - Tracks evolution of implementation
  - Records lessons learned
  - Notes potential improvements
- USE the Memory section to:
  - Aid in knowledge transfer
  - Support maintenance decisions
  - Guide future enhancements
  - Prevent repeated mistakes
  - Maintain implementation context

## Memory
- **Rationale:** Initiated based on PR feedback highlighting the need to separate user interface theme preferences from the theme applied to the wireframe design itself. This enhances user flexibility.
- **Current State:** App theme (light/dark/system) is managed via Redux (`uiSlice` likely) and applied using Ant Design `ConfigProvider` and `body.dark-theme` class. Shape/canvas styling currently directly uses this global theme state.
- **Target State:** Two independent theme states:
    - `appTheme`: Controls UI chrome (current mechanism mostly reused).
    - `designTheme`: Controls canvas background and default shape/element appearance on the canvas. Managed by a new Redux state/slice.
- **Key Components Involved:**
    - Redux Store (`store.ts`, new slice)
    - Theme Provider (`ThemeProvider.tsx` or similar)
    - App Layout (`App.tsx`) - for placing the new toggle
    - Canvas Editor (`src/wireframes/renderer/Editor.tsx`)
    - Canvas Item Layer (`src/wireframes/renderer/ItemsLayer.tsx`)
    - Theme Utilities (`src/wireframes/engine/ThemeShapeUtils.ts`)
    - Shape Components (e.g., `src/wireframes/shapes/shared/icon.ts`, various shape renderers)
- **Potential Challenges:** Ensuring theme state propagation is efficient and updates occur correctly without unnecessary re-renders. Refactoring `ThemeShapeUtils` logic carefully.

## Active

**Phase 1: State Management for Design Theme**
- [ ] Define `designTheme` state structure (e.g., `{ mode: 'light' | 'dark' | 'match-app' }`). Consider if 'match-app' is desired or if explicit light/dark is sufficient initially.
- [ ] Create a new Redux slice (e.g., `designThemeSlice`) to manage the `designTheme` state.
  - [ ] Include initial state (e.g., defaulting to 'light' or 'match-app').
  - [ ] Add reducers/actions to set the design theme (`setDesignThemeMode`).
  - [ ] Create selectors to access the `designTheme` state (`selectDesignThemeMode`).
- [ ] Integrate the new slice into the main Redux store (`store.ts`).
- [ ] Consider persistence for the `designTheme` setting (e.g., using `localStorage` similar to the app theme).

**Phase 2: UI Control for Design Theme**
- [ ] Design a UI control for selecting the Design Theme (e.g., a dropdown or segmented control). Options: Light / Dark / (Optional: Match App Theme).
- [ ] Determine the best location for this control. Suggestions:
    - Canvas properties panel (if one exists).
    - Main toolbar near zoom controls.
    - View menu.
- [ ] Implement the UI component for the Design Theme selector.
- [ ] Connect the UI component to the Redux store:
    - Display the current `designTheme` state.
    - Dispatch the `setDesignThemeMode` action on user selection.

**Phase 3: Refactor Canvas & Layer Rendering**
- [ ] Access the `designTheme` state within `src/wireframes/renderer/Editor.tsx` (using `useAppSelector`).
- [ ] Propagate the `designTheme` state down to `ItemsLayer.tsx` (e.g., as a new prop `designThemeMode`).
- [ ] Update `Editor.tsx` (or relevant component) to set the main *canvas background color* based on the `designTheme` state, not the app theme state. Remove logic that currently ties canvas background to `appTheme`.

**Phase 4: Refactor Shape & Icon Rendering Logic**
- [ ] Analyze `src/wireframes/engine/ThemeShapeUtils.ts`:
    - Identify functions that return theme-dependent colors/styles (fills, strokes, text colors).
    - Modify these functions to accept the `designThemeMode` as a parameter.
    - Update the logic within these functions to return values based on the passed `designThemeMode`, removing reliance on the global `appTheme` state for these calculations.
- [ ] Update `ItemsLayer.tsx` (or the component responsible for rendering individual shapes):
    - Pass the `designThemeMode` prop down to individual shape rendering components or utility functions when calculating styles.
- [ ] Refactor specific shape components (like `src/wireframes/shapes/shared/icon.ts`) that currently use the global `appTheme` (`theme.isDarkMode`) for *on-canvas* element styling. They should now use the `designThemeMode` passed down via props or context.
    - **Crucially:** Ensure UI elements *within* the app chrome (like toolbar icons, if separate) continue to use the `appTheme`. The focus here is changing elements *rendered on the design canvas*.
- [ ] Update any logic that sets default colors for new shapes to use the current `designTheme` setting.

**Phase 5: Testing and Refinement**
- [ ] Test toggling the **App Theme**: Verify only the UI chrome changes, and the canvas/shapes remain styled according to the **Design Theme**.
- [ ] Test toggling the **Design Theme**: Verify only the canvas background and shape/element styles on the canvas change, while the UI chrome remains styled according to the **App Theme**.
- [ ] Test the (optional) 'Match App Theme' setting if implemented.
- [ ] Test creating new shapes – ensure they adopt the current **Design Theme** correctly.
- [ ] Test loading existing diagrams – ensure they render correctly (consider how saved diagrams without a design theme preference should behave - default to light?).
- [ ] Perform cross-browser testing.
- [ ] Check for any performance regressions during theme toggling.

## Pending
- [ ] Consider adding persistence for the Design Theme choice (localStorage).
- [ ] Refine the UI/UX of the Design Theme selector based on initial testing.
- [ ] Documentation update: Explain the difference between App Theme and Design Theme to users.

## Completed
- [ ] Initial analysis of App Theme vs Design Theme coupling (YYYY-MM-DD) 