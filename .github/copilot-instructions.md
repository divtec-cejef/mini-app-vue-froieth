# Vue.js Mini App with Vuetify 3

Vue.js Mini App is a modern web application built with Vue 3, Vuetify 3, and Vite. It provides a clean starting template with Material Design components, automatic component importing, and file-based routing.

**ALWAYS reference these instructions first and fallback to search or bash commands only when you encounter unexpected information that does not match the info here.**

## Working Effectively

### Prerequisites and Environment Setup
- Node.js v20.19.5+ and npm v10.8.2+ are already available in the environment
- No additional SDK downloads or installations required

### Bootstrap, Build, and Test Commands
Run these commands in sequence for a fresh clone:

1. **Install Dependencies**:
   ```bash
   npm install
   ```
   - Takes ~15 seconds. Set timeout to 60+ seconds to be safe.
   - Preferred over `yarn install` (which takes 35+ seconds and shows warnings)

2. **Build the Application**:
   ```bash
   npm run build
   ```
   - Takes ~3 seconds. Set timeout to 60+ seconds.
   - **NEVER CANCEL** - Build is fast but always wait for completion
   - Creates production build in `dist/` directory
   - Build succeeds despite known linting errors (see Troubleshooting section)

3. **Run Development Server**:
   ```bash
   npm run dev
   ```
   - Starts Vite dev server in ~800ms
   - Serves on http://localhost:3000
   - Includes hot module replacement (HMR)
   - **CRITICAL**: Always verify server is accessible before proceeding

4. **Run Production Preview**:
   ```bash
   npm run preview
   ```
   - Serves production build from `dist/` on http://localhost:4173
   - Requires `npm run build` to be run first

5. **Run Linting**:
   ```bash
   npm run lint
   ```
   - Takes ~2 seconds. Set timeout to 30+ seconds.
   - **Known Issue**: Currently fails with 2 import/no-duplicates errors in `src/router/index.js`
   - Build and runtime still work despite linting failures
   - Run before committing changes but expect current failures

## Validation and Testing

### Manual Validation Requirements
After making ANY changes to the codebase:

1. **ALWAYS run the bootstrap sequence**:
   - `npm install` (if dependencies changed)
   - `npm run build` 
   - `npm run dev`

2. **ALWAYS test the running application**:
   - Navigate to http://localhost:3000
   - Verify the Vuetify welcome page loads with:
     - Vuetify logo at the top
     - "Welcome to Vuetify" heading
     - "Get started" card with rocket icon
     - Four navigation cards: Documentation, Features, Components, Community
   - Test clicking on external links to ensure they open properly
   - Verify Material Design icons render correctly

3. **ALWAYS run linting before finishing**:
   - `npm run lint` (expect current failures, don't add new ones)

### End-to-End Testing Scenario
Execute this complete workflow after making changes:
1. Stop any running dev servers
2. Run `npm run build`
3. Run `npm run preview`
4. Navigate to http://localhost:4173
5. Verify the application loads and displays correctly
6. Test basic navigation and component rendering

## Troubleshooting

### Known Issues
- **Linting Errors**: `src/router/index.js` has 2 import/no-duplicates errors that don't affect functionality
- **Font Loading**: Google Fonts may show net::ERR_BLOCKED_BY_CLIENT in some environments but app works fine
- **TypeScript Warnings**: ESLint config has unmet peer dependency warnings when using yarn (use npm instead)

### Build Failures
- If build fails, check Node.js version: `node --version` (should be 20.19.5+)
- If dependencies fail to install, delete `node_modules` and `package-lock.json`, then `npm install`
- Never mix npm and yarn - stick to npm for this project

### Development Server Issues
- Dev server should start on port 3000. If port conflicts, Vite will auto-increment
- If HMR stops working, restart the dev server
- If components don't render, check console for JavaScript errors

## Project Structure and Key Files

### Root Directory
```
/
├── README.md                 # Project documentation
├── package.json             # Dependencies and scripts
├── vite.config.mjs          # Vite configuration
├── eslint.config.js         # ESLint configuration
├── index.html               # HTML entry point
├── jsconfig.json            # JavaScript configuration
├── .browserslistrc          # Browser compatibility
├── .editorconfig            # Editor configuration
└── .gitignore               # Git ignore rules
```

### Source Structure (`src/`)
```
src/
├── App.vue                  # Root Vue component
├── main.js                  # Application entry point
├── components/              # Reusable Vue components
│   ├── HelloWorld.vue       # Main welcome component
│   └── AppFooter.vue        # Footer component
├── pages/                   # File-based routes
│   └── index.vue            # Home page route
├── layouts/                 # Layout components
│   └── default.vue          # Default layout
├── plugins/                 # Vue plugins
│   ├── index.js             # Plugin registration
│   └── vuetify.js           # Vuetify configuration
├── router/                  # Vue Router configuration
│   └── index.js             # Auto-generated routes
├── stores/                  # Pinia state stores
│   ├── app.js               # App state
│   └── index.js             # Store registration
├── styles/                  # SCSS/CSS styles
└── assets/                  # Static assets
```

### Important Files to Monitor
- **Always check `src/components/HelloWorld.vue`** after making UI changes
- **Always check `src/router/index.js`** after modifying routing (has known linting issues)
- **Always check `vite.config.mjs`** when adding new plugins or build configuration
- **Always check `package.json`** when adding dependencies or scripts

## Common Development Tasks

### Adding New Components
1. Create `.vue` files in `src/components/`
2. Components are auto-imported via `unplugin-vue-components`
3. Use Vuetify components directly (e.g., `<v-btn>`, `<v-card>`)

### Adding New Routes
1. Create `.vue` files in `src/pages/`
2. Routes are auto-generated via `unplugin-vue-router`
3. File structure maps to URL structure

### Styling and Theming
1. Global styles in `src/styles/`
2. Vuetify theme configuration in `src/plugins/vuetify.js`
3. SCSS variables in `src/styles/settings.scss`

### State Management
1. Create stores in `src/stores/` using Pinia
2. Stores are auto-imported via `unplugin-auto-import`

## Package Manager Commands

### NPM (Recommended)
- Install: `npm install` (15 seconds)
- Dev server: `npm run dev`
- Build: `npm run build` (3 seconds)
- Preview: `npm run preview`
- Lint: `npm run lint` (2 seconds, expect failures)

### Yarn (Alternative, Not Recommended)
- Install: `yarn install` (35+ seconds, shows warnings)
- Dev server: `yarn dev`
- Build: `yarn build`
- Preview: `yarn preview`
- Lint: `yarn lint`

**Use npm for consistency and better performance.**

## Technology Stack
- **Frontend Framework**: Vue 3.5.17
- **UI Framework**: Vuetify 3.9.1
- **Build Tool**: Vite 6.3.6
- **State Management**: Pinia 3.0.3
- **Routing**: Vue Router 4.5.1 with auto-generation
- **Styling**: SCSS with Vuetify Material Design
- **Icons**: Material Design Icons (@mdi/font)
- **Fonts**: Roboto via Google Fonts

## CI/CD Notes
- No GitHub Actions workflows currently configured
- Linting must be fixed before any CI setup
- Build artifacts in `dist/` should not be committed
- Auto-generated files (`auto-imports.d.ts`, `components.d.ts`, `typed-router.d.ts`) are gitignored

## Performance Notes
- Development server starts in ~800ms
- Build completes in ~3 seconds  
- Hot module replacement is instant
- Application loads quickly with code splitting
- Vuetify components are tree-shaken automatically