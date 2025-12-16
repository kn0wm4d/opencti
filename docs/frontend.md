# OpenCTI Frontend Architecture Documentation

## Overview

OpenCTI's frontend is a modern, single-page application (SPA) built with React 19 and TypeScript, using Material-UI (MUI) as the primary component library. The application leverages GraphQL with Relay for efficient data fetching and state management, and Vite as the build tool for fast development and optimized production builds.

## HTML Template

### Base Template (`index.html`)

The application uses a minimal HTML template located at `opencti-platform/opencti-front/index.html`:

```html
<!doctype html>
<html lang="en">
    <head>
        <script>window.BASE_PATH = "%BASE_PATH%"</script>
        %APP_SCRIPT_SNIPPET%
        <title>%APP_TITLE%</title>
        <base href>
        <meta http-equiv="Content-Type" content="text/html; charset=utf-8">
        <meta http-equiv="X-UA-Compatible" content="IE=edge">
        <meta name="viewport" content="width=device-width,initial-scale=1">
        <meta name="dеѕсrірtіоn" content="%APP_DESCRIPTION%">
        <link id="favicon" rel="shortcut icon" href="%APP_FAVICON%">
        <link id="manifest" rel="manifest" href="%APP_MANIFEST%">
    </head>
    <body>
        <noscript>You need to enable JavaScript to run this app.</noscript>
        <div id="root"></div>
        <script type="module" src="/src/front.tsx"></script>
    </body>
</html>
```

**Key Features:**
- Template variables (e.g., `%BASE_PATH%`, `%APP_TITLE%`) are replaced during build/serve time
- Single root div (`#root`) where the React application mounts
- Module script entry point at `/src/front.tsx`
- Responsive viewport configuration
- Progressive Web App (PWA) manifest support

**Note:** The HTML template shown above is an exact copy of the source file at the time of documentation. Some attributes may contain unusual characters (e.g., the meta description name uses Cyrillic characters) or incomplete values (e.g., empty base href) - these reflect the actual state of the source file.

## CSS Framework & Styling

### Primary Styling System

**Material-UI (MUI) v6.5.0** - The application uses MUI as its primary component library and design system:

- **@mui/material**: Core component library
- **@mui/icons-material**: Icon components
- **@mui/lab**: Experimental components (Timeline, LoadingButton, etc.)
- **@mui/x-date-pickers**: Date and time picker components
- **@mui/styles**: Legacy styling solution (deprecated, being phased out)

### Typography

**Font Families:**
- Primary: IBM Plex Sans (`@fontsource/ibm-plex-sans`)
- Secondary: Geologica (`@fontsource/geologica`)

Both fonts are imported directly in `src/front.tsx`:
```typescript
import '@fontsource/ibm-plex-sans';
import '@fontsource/geologica';
```

### Theme System

The application implements a comprehensive theming system with both dark and light modes:

#### Dark Theme (`src/components/ThemeDark.ts`)
- **Background**: `#070d19` (default)
- **Primary**: `#0fbcff` (cyan blue)
- **Secondary**: `#00f1bd` (turquoise)
- **Paper**: `#09101e`
- **Navigation**: `#070d19`
- **Accent**: `#0f1e38`

#### Light Theme (`src/components/ThemeLight.ts`)
- **Background**: `#f8f8f8` (light grey)
- **Primary**: `#001bda` (deep blue)
- **Secondary**: `#0c7e69` (teal)
- **Paper**: `#ffffff` (white)
- **Navigation**: `#ffffff`
- **Accent**: `#dfdfdf`

Both themes support customization for:
- Logo and collapsed logo
- Primary and secondary colors
- Background, paper, navigation, and accent colors
- Text colors

### Custom CSS Files

Located in `src/static/css/`:
- **index.css**: Global styles, scrollbar styling, markdown rendering
- **leaflet.css**: Styles for Leaflet map components
- **timerange.css**: Custom time range selector styles
- **CKEditorDark.css**: Dark theme styles for CKEditor
- **CKEditorLight.css**: Light theme styles for CKEditor

### Additional CSS Dependencies

The application imports CSS from various third-party libraries:
```typescript
import 'ckeditor5/ckeditor5.css';
import 'react-grid-layout/css/styles.css';
import 'react-mde/lib/styles/css/react-mde-all.css';
```

## JavaScript Framework & Libraries

### Core Framework

**React 19.2.1** - The application is built with the latest version of React, utilizing:
- React hooks for state management
- Suspense for code splitting and lazy loading
- StrictMode for development checks
- React Router v6 for routing

### TypeScript

**TypeScript 5.9.3** - Full TypeScript implementation with strict type checking.

Key configuration options (`tsconfig.json`):
- Strict mode enabled
- JSX: `react-jsx` (React 17+ transform)
- Target: ES5 for broad compatibility
- Module system: ESNext with Node resolution
- Path aliases for `@components/*` and `src/*`

### State Management & Data Fetching

**React Relay 20.1.1** - GraphQL client for efficient data fetching:
- **relay-runtime**: Runtime for Relay operations
- **react-relay**: React integration for Relay
- **react-relay-network-modern**: Enhanced network layer
- **graphql**: GraphQL implementation (v16.12.0)
- **graphql-ws**: WebSocket support for GraphQL subscriptions

**Relay Configuration:**
```json
{
  "src": "./src",
  "schema": "./src/schema/relay.schema.graphql",
  "language": "typescript",
  "eagerEsModules": true
}
```

### Routing

**react-router-dom 6.30.2** - Client-side routing with:
- Browser-based routing
- Nested routes
- Lazy loading for code splitting
- Route protection via `AuthBoundaryComponent`

**Main Routes:**
- `/dashboard/*` - Private application routes
- `/public/*` - Public routes
- Default redirect to `/dashboard`

### Form Management

**Formik 2.4.9** - Form state management:
- **formik-mui**: MUI integration
- **formik-mui-lab**: MUI Lab components integration
- **yup**: Schema validation (v1.7.1)

Alternative form libraries:
- **@rjsf/core & @rjsf/mui**: React JSON Schema Forms
- **@jsonforms/react & @jsonforms/material-renderers**: JSON Forms

### UI Component Libraries (Beyond MUI)

#### Rich Text Editing
- **ckeditor5 (43.3.1)**: Modern WYSIWYG editor
- **@ckeditor/ckeditor5-react**: React wrapper
- **react-mde**: Markdown editor

#### Data Visualization
- **apexcharts (4.4.0)** + **react-apexcharts**: Modern charting library
- **recharts (3.2.0)**: Composable charting library
- **d3-scale**, **d3-hierarchy**, **d3-timer**: D3.js utilities

#### Graphs & Networks
- **react-force-graph-2d (1.29.0)**: 2D force-directed graphs
- **react-force-graph-3d (1.24.4)**: 3D force-directed graphs
- **reactflow (11.11.4)**: Node-based diagrams

#### Maps
- **leaflet (1.9.4)**: Interactive maps
- **react-leaflet (5.0.0)**: React wrapper for Leaflet

#### Drag & Drop
- **@hello-pangea/dnd (18.0.1)**: Drag and drop for lists
- **react-draggable (4.5.0)**: Draggable components
- **react-grid-layout (1.5.3)**: Grid layout with drag and drop

#### Other UI Components
- **react-material-ui-carousel (3.4.2)**: Carousel component
- **react-color (2.19.3)**: Color picker
- **react-syntax-highlighter (16.1.0)**: Code syntax highlighting
- **react-virtualized (9.22.6)**: Virtual scrolling for large lists
- **react-wordcloud (1.2.7)**: Word cloud visualization
- **react-pdf (10.2.0)**: PDF viewer

### Internationalization

**react-intl 7.1.14** - Internationalization framework supporting multiple languages with translation files in the `lang/` directory.

### Utility Libraries

#### Date & Time
- **date-fns (4.1.0)**: Modern date utility library
- **moment (2.30.1)** + **moment-timezone**: Legacy date handling

#### Data Processing
- **ramda (0.32.0)**: Functional programming utilities
- **axios (1.13.2)**: HTTP client
- **rxjs (7.8.2)**: Reactive programming

#### Document Generation
- **pdfmake (0.2.20)**: PDF generation
- **html-to-pdfmake (2.5.32)**: Convert HTML to PDF
- **html-to-image (1.11.13)**: Convert HTML to images

#### Markdown & HTML
- **markdown-to-jsx (8.0.0)**: Markdown to JSX converter
- **marked (17.0.1)**: Markdown parser
- **react-markdown (10.1.0)**: Markdown component
- **remark-gfm (4.0.1)**: GitHub Flavored Markdown
- **html-react-parser (5.2.10)**: HTML to React parser
- **dompurify (3.3.1)**: HTML sanitizer

#### Security & Authentication
- **react-cookie (8.0.1)**: Cookie management
- **react-otp-input (3.1.1)**: OTP input component
- **qrcode (1.5.4)**: QR code generation

#### Analytics
- **analytics (0.8.19)**: Analytics framework
- **use-analytics (1.1.0)**: React hook for analytics
- **@analytics/google-analytics (1.1.0)**: Google Analytics integration

#### Other
- **uuid (11.1.0)**: UUID generation
- **classnames (2.5.1)**: Conditional CSS classes
- **invert-color (2.0.0)**: Color inversion utility
- **is-svg (6.1.0)**: SVG detection
- **js-base64 (3.7.8)**: Base64 encoding/decoding
- **js-file-download (0.4.12)**: File download utility
- **json5 (2.2.3)**: JSON5 parser
- **buffer (6.0.3)**: Node.js Buffer API for browsers

### Custom Icons
- **filigran-icon (0.21.0)**: Custom icon library
- **mdi-material-ui (7.9.4)**: Material Design Icons for MUI

## Build Tooling

### Primary Build Tool: Vite

**Vite 7.2.6** - Modern frontend build tool providing:
- Lightning-fast Hot Module Replacement (HMR)
- Optimized production builds
- Native ES modules in development
- Plugin ecosystem

**Vite Configuration (`vite.config.mts`)** includes:
- **Build target**: Chrome 58+ for production
- **Path aliases**: `@components` → `./src/private/components`, `src` → `./src`
- **Dev server**: Port 3000 with proxy to backend (localhost:4000) for GraphQL and other API endpoints
- **Dependency optimization**: Pre-optimizes 170+ dependencies to avoid reload on lazy routes
- **Plugins**: React, Relay compiler, static file copying, and custom HTML transformation

### Vite Plugins

- **@vitejs/plugin-react (5.1.1)**: React support with Fast Refresh
- **vite-plugin-relay (2.1.0)**: Relay compiler integration
- **vite-plugin-static-copy (3.1.4)**: Copy static assets

### Alternative Production Build: esbuild

**esbuild 0.27.1** - Extremely fast JavaScript bundler used for production builds via custom builder scripts in `builder/prod/`.

### Code Quality Tools

#### Linting
- **ESLint 9.39.1**: JavaScript/TypeScript linter
- **@eslint/js**: ESLint JavaScript rules
- **@stylistic/eslint-plugin**: Stylistic rules
- **eslint-plugin-react**: React-specific rules
- **eslint-plugin-import**: Import/export linting
- **typescript-eslint (8.48.1)**: TypeScript ESLint rules

#### Type Checking
- **TypeScript 5.9.3**: Static type checking

#### Testing
- **Vitest (4.0.15)**: Modern test framework
  - **@vitest/coverage-v8**: Code coverage
  - **@testing-library/react (16.3.0)**: React testing utilities
  - **@testing-library/jest-dom (6.9.1)**: Jest DOM matchers
  - **@testing-library/user-event (14.6.1)**: User interaction simulation
  - **jsdom (27.2.0)**: DOM implementation for Node.js

#### End-to-End Testing
- **@playwright/test (1.57.0)**: Browser automation and E2E testing
- **monocart-reporter (2.9.23)**: Test reporting

### Development Tools

- **Relay Compiler (20.1.1)**: GraphQL query compiler
- **babel-plugin-relay (20.1.1)**: Babel plugin for Relay
- **i18n-auto-translation (2.2.3)**: Automated translation tool
- **license-checker-rseidelsohn (4.4.2)**: License compliance checking
- **@faker-js/faker (10.1.0)**: Test data generation

## Application Architecture

### Entry Point

**src/front.tsx** - Application entry point that:
- Imports fonts (IBM Plex Sans, Geologica) and CSS files
- Creates React root from `#root` div
- Wraps app in `RelayEnvironmentProvider` for GraphQL state
- Uses React Suspense for lazy loading with custom loading component
- Renders main `<App />` component

### Application Structure

**src/app.tsx** - Main application component structure:
- Wrapped in `CookiesProvider` for cookie management
- Uses `BrowserRouter` with configurable base path
- Protected by `AuthBoundaryComponent` for authentication
- `RedirectManager` handles navigation redirects
- Three main routes:
  - `/dashboard/*` → Private authenticated routes
  - `/public/*` → Public unauthenticated routes  
  - `/*` → Default redirect to dashboard

### Directory Structure

```
opencti-platform/opencti-front/
├── src/
│   ├── app.tsx                 # Main application component
│   ├── front.tsx               # Entry point
│   ├── components/             # Shared UI components
│   │   ├── ThemeDark.ts       # Dark theme configuration
│   │   ├── ThemeLight.ts      # Light theme configuration
│   │   ├── Loader.tsx         # Loading component
│   │   ├── fields/            # Form field components
│   │   ├── filters/           # Filter components
│   │   ├── graph/             # Graph visualization components
│   │   └── ...
│   ├── private/               # Private (authenticated) routes
│   │   ├── Root.tsx           # Private root component
│   │   └── components/        # Private components
│   ├── public/                # Public routes
│   │   └── PublicRoot.tsx     # Public root component
│   ├── relay/                 # Relay GraphQL configuration
│   │   └── environment.ts     # Relay environment setup
│   ├── schema/                # GraphQL schema
│   │   └── relay.schema.graphql
│   ├── static/                # Static assets
│   │   ├── css/               # Global CSS files
│   │   ├── images/            # Images and logos
│   │   ├── geo/               # Geographic data
│   │   └── ext/               # External resources
│   └── utils/                 # Utility functions
├── lang/                      # Internationalization files
├── tests_e2e/                 # End-to-end tests
├── builder/                   # Custom build scripts
│   ├── dev/                   # Development build
│   └── prod/                  # Production build
├── index.html                 # HTML template
├── vite.config.mts           # Vite configuration
├── tsconfig.json             # TypeScript configuration
├── relay.config.json         # Relay configuration
├── eslint.config.mjs         # ESLint configuration
├── vitest.config.ts          # Vitest configuration
├── playwright.config.ts      # Playwright configuration
└── package.json              # Dependencies and scripts
```

## Development Scripts

Located in `package.json`:

- **`yarn dev`** - Start development server with Vite
- **`yarn start`** - Start development server with custom dev script
- **`yarn build`** - Build for production using esbuild
- **`yarn relay`** - Compile Relay GraphQL queries
- **`yarn lint`** - Run ESLint
- **`yarn test`** - Run unit tests with Vitest
- **`yarn test:e2e`** - Run end-to-end tests with Playwright
- **`yarn check-ts`** - Type-check without emitting files

## Key Features

### Performance Optimizations

1. **Code Splitting**: Lazy loading with React.lazy() for routes
2. **Dependency Pre-optimization**: Vite pre-optimizes 170+ dependencies
3. **GraphQL Optimization**: Relay's efficient query batching and caching
4. **Virtual Scrolling**: React-virtualized for large lists
5. **Production Builds**: Optimized with esbuild targeting Chrome 58+

### Accessibility

- Material-UI components with built-in accessibility
- Semantic HTML structure
- ARIA attributes via MUI components
- Keyboard navigation support

### Progressive Web App (PWA)

- Manifest file support
- Service worker ready
- Offline capability support

### Security

- **DOMPurify**: HTML sanitization to prevent XSS attacks
- Content Security Policy ready
- HTTPS enforcement in production
- Cookie-based authentication

## Browser Support

The application targets modern browsers with ES5 support, specifically Chrome 58+ as the minimum target based on the Vite build configuration.

## Conclusion

OpenCTI's frontend represents a modern, well-architected single-page application built with industry-standard tools and libraries. The combination of React, TypeScript, Material-UI, Relay, and Vite provides a solid foundation for building a scalable, performant, and maintainable cyber threat intelligence platform.

The extensive use of specialized libraries for visualization (charts, graphs, maps), rich text editing, and data processing enables complex features while maintaining code quality through TypeScript, ESLint, and comprehensive testing frameworks.
