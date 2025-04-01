# [UI] Dark Mode Implementation

## Description
This todo file tracks the implementation of dark mode support for mydraft.cc. This involves:
- Creating a theme system that supports both light and dark modes
- Configuring Ant Design theming
- Implementing CSS/SCSS variables for custom components
- Adding user controls for theme switching
- Persisting theme preferences

The implementation should be comprehensive across all UI components while maintaining the application's existing functionality and user experience. No pseudo code or implementations will be included here - only detailed steps on how to achieve dark mode support.

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
- The application uses Ant Design 5.24.5, which has built-in dark mode support via ConfigProvider
- Current color variables are defined in src/style/_vars.scss
- Main layout structure is in App.tsx using Ant Design Layout components
- No existing theme toggle or ConfigProvider implementation exists
- The app already uses Redux Toolkit for state management
- Dark theme styling has been partially implemented with CSS variables approach
- Added `body.dark-theme` class selector for theme switching
- Fixed issues with SASS color functions (like darken()) by adding pre-computed darker variants
- Fixed scrollbars mixin, box-shadow mixins and other SASS color functions to work with CSS variables
- Implemented ThemeProvider that applies both the body class and Ant Design ConfigProvider
- Implemented Redux state for theme preferences, including system preference detection
- Observed that the main canvas area and UI components need further refinement for dark mode
- Created theme-aware utility functions for shape components to support dark mode
- Implemented canvas background color switching based on current theme
- Fixed circular dependency issues with ThemeShapeUtils by:
  - Removing direct store dependency
  - Creating a global theme state that can be updated by ThemeProvider
  - Updating shape components to properly handle theme changes at render time
  - Using a safer approach to apply theme colors without modifying shape properties directly
- Made the canvas background darker in dark mode for better contrast
- Updated the navigation bar to be theme-aware with improved styling
- Fixed the main canvas drawing area background in dark mode
- Ensured proper background fill and stroke color based on theme
- Improved form controls (input fields, select boxes, etc.) in dark mode
- Improved color picker component to use theme variables

## Active
- [x] Set up Redux state management for theme
  - [x] Create a new slice for theme settings
  - [x] Add actions for toggling and setting theme
  - [x] Create selectors for theme state
  - [x] Connect to localStorage for persistence

- [x] Create SCSS dark theme variables
  - [x] Create new _theme.scss file with light/dark mode variables
  - [x] Refactor _vars.scss to use theme variables
  - [x] Add CSS custom properties (variables) for runtime theme switching

- [x] Implement Ant Design theming
  - [x] Add ConfigProvider to index.tsx
  - [x] Create light and dark theme algorithms/tokens
  - [x] Connect ConfigProvider theme to Redux state

- [x] Add theme toggle component
  - [x] Create ThemeToggle component with icon button
  - [x] Add it to the top menu in App.tsx
  - [x] Connect toggle action to Redux

- [x] Update canvas and diagram rendering for dark mode
  - [x] Fix white background in main drawing canvas
  - [x] Create theme-aware colors for shapes
  - [x] Use darker background for better contrast in dark mode

- [x] Fix UI component theming
  - [x] Update sidebar styles (light backgrounds still present)
  - [x] Ensure proper tab styling in dark mode
  - [x] Fix navigation bar in dark mode
  - [x] Fix input fields and form controls for dark mode
  - [ ] Style toolbar buttons and dropdowns consistently

- [ ] Update custom components for theme support
  - [x] Fix shape components (buttons, checkboxes, etc.) to respect theme
  - [x] Update SVG/canvas components to respect theme
  - [x] Fix circular dependencies in theme-aware components
  - [x] Fix color picker for dark mode
  - [ ] Ensure diagrams display correctly in both themes

- [ ] Handle theme-specific styles for wireframe components
  - [x] Ensure shapes are visible against dark backgrounds
  - [x] Update hardcoded colors in shape definitions
  - [x] Make sure colors in editor are theme-aware
  - [x] Add proper contrast to text elements in dark mode

- [x] Implement system theme detection
  - [x] Add media query listener for prefers-color-scheme
  - [x] Set initial theme based on system preference
  - [x] Add option to follow system theme

## Pending
- [ ] Optimize theme switching performance
  - [ ] Consider code splitting theme-specific styles
  - [ ] Look into reducing flashes during theme changes

- [ ] Accessibility improvements
  - [ ] Ensure proper contrast ratios in both themes
  - [ ] Add high-contrast mode option
  - [ ] Test with screen readers in both themes

- [ ] Add transition animations for theme switching
  - [ ] Research smooth transition techniques
  - [ ] Implement subtle animations between themes

## Completed
- [x] Initial analysis of codebase for theme implementation (2024-07-04)
- [x] Implemented Redux state management for theme (2024-07-08)
- [x] Created CSS variables system for theming (2024-07-08)
- [x] Added ThemeToggle component with dropdown menu (2024-07-08)
- [x] Fixed SASS color function issues with CSS variables (2024-07-08)
- [x] Fixed canvas background for dark mode (2024-07-09)
- [x] Created theme-aware shape utility functions (2024-07-09)
- [x] Updated sidebar and tab styles for dark theme (2024-07-09)
- [x] Updated rectangle and button components to be theme-aware (2024-07-09)
- [x] Fixed circular dependency issue in theme utilities (2024-07-09)
- [x] Improved navigation bar for dark theme (2024-07-09)
- [x] Enhanced canvas background contrast in dark mode (2024-07-09)
- [x] Fixed main canvas drawing area background (2024-07-09)
- [x] Improved form controls for dark mode (2024-07-09)
- [x] Enhanced color picker for dark mode (2024-07-09)

## Current Issues Identified
1. ~~Canvas area: The main drawing canvas still has a white background in dark mode~~ (Fixed)
2. ~~Sidebar backgrounds: The sidebars are still using light backgrounds in dark mode~~ (Fixed)
3. ~~Tab styling: The tabs in the left sidebar don't have proper dark mode styling~~ (Fixed)
4. ~~Wireframe components: Shape components (buttons, checkboxes, etc.) don't adapt to dark theme~~ (Fixed)
5. ~~Main navigation bar: Header needs dark mode styling~~ (Fixed)
6. ~~Form elements: Input fields, dropdowns and other form elements need dark styling~~ (Fixed)
7. ~~Color picker: Color selector needs dark mode treatment~~ (Fixed)
8. ~~Shape borders: The borders and grid elements need better contrast in dark mode~~ (Fixed)
9. Text elements: Text needs proper contrast adjustments in dark mode

## Next Steps
1. ~~Focus on the canvas area to ensure proper dark background~~ (Completed)
2. ~~Update the sidebar styles to use theme variables~~ (Completed)
3. ~~Fix main navigation bar in dark mode~~ (Completed)
4. ~~Complete form elements and input fields styling~~ (Completed)
5. Test all components in both themes to ensure consistency
6. Address any remaining UI inconsistencies
7. Consider adding transition animations for theme switching 

## Test Theme Across All Features
- [ ] Verify theme consistency across all pages/views
- [ ] Test theme transitions
- [ ] Ensure all UI elements are visible in both themes
- [ ] Test with various browsers and screen sizes 