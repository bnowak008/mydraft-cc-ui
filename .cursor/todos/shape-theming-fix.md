# TODO: Fix Shape Theming on Toggle

**Goal:** Ensure shapes on the canvas and icons/shapes in the toolbar update their appearance *immediately* and *correctly* upon the first theme toggle, resolving the lag and inverse-theme issue.

**Related Files:**
*   `src/wireframes/renderer/Editor.tsx`
*   `src/wireframes/engine/ThemeShapeUtils.ts` (Assumed, verify existence/usage)
*   `src/wireframes/components/page-layout/Layout.tsx` (Likely location of theme toggle)
*   Toolbar component(s) (Identify specific files)
*   `src/wireframes/renderer/ItemsLayer.tsx`

## Phase 1: Investigation & Verification

1.  **Trace Theme Propagation:**
    *   [ ] Verify theme state update mechanism (Redux action in `Layout.tsx`?).
    *   [ ] Add temporary logging in `Editor.tsx` to confirm `isDarkMode` update timing (`useAppSelector`).
    *   [ ] Investigate `ThemeShapeUtils.ts`: How does it get the theme? Add logging.
    *   [ ] Identify toolbar component(s) and investigate their theme access. Add logging.

2.  **Analyze Rendering Cycle:**
    *   [ ] Focus on the first theme toggle.
    *   [ ] Inside `ItemsLayer` (or rendered shape components), log the theme value used during rendering *after* a toggle.
    *   [ ] Compare logged theme value with the actual *new* theme state. Is it lagging?
    *   [ ] Determine if the `Engine` or its layers (`diagramLayer`) need explicit notification/update calls on theme change.

## Phase 2: Refactoring & Implementation

1.  **Remove `key` Prop:**
    *   [ ] In `Editor.tsx`, remove `key={`master-${isDarkMode}`}` from the first `ItemsLayer`.
    *   [ ] In `Editor.tsx`, remove `key={`main-${isDarkMode}`}` from the second `ItemsLayer`.

2.  **Explicit Theme Propagation:**
    *   [ ] Pass `isDarkMode` as a prop to both `ItemsLayer` instances in `Editor.tsx`.

3.  **React to Theme Changes:**
    *   [ ] Inside `ItemsLayer` (or relevant child components/engine interaction points), add a `useEffect` hook that depends on the `isDarkMode` prop.
    *   [ ] Within the `useEffect`, trigger the necessary redraw or style update:
        *   [ ] **Determine correct mechanism:** Is updating props sufficient, or does the `diagramLayer` or `Engine` need an explicit call (e.g., `invalidate()`, `updateStyles(theme)`)? Implement the required update call.

4.  **Update `ThemeShapeUtils.ts` (If Necessary):**
    *   [ ] Ensure `ThemeShapeUtils.ts` reliably accesses the *current* theme state during the render cycle.
    *   [ ] Modify it to accept the theme as an argument if it currently tries (and fails) to derive it globally/asynchronously.

5.  **Address Toolbar Theming:**
    *   [ ] Apply similar principles: Ensure toolbar component(s) receive updated theme state correctly (props/context).
    *   [ ] Ensure toolbar components react to theme changes using `useEffect` or similar, updating styles/triggering redraws as needed.

## Phase 3: Testing

1.  [ ] Test toggling light/dark modes repeatedly.
2.  [ ] Verify canvas shapes update immediately and correctly on the *first* toggle.
3.  [ ] Verify toolbar icons/shapes update immediately and correctly on the *first* toggle.
4.  [ ] Check for performance regressions or improvements.
5.  [ ] Test edge cases (e.g., empty canvas, complex diagrams, loading diagrams during toggle).