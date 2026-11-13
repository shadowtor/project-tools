# Specialist toolbelt and selection guide

Reviewed 6 October 2026. Read this reference when a project needs a capability listed below. These are conditional choices, not a starter dependency list. Preserve working integrations, use the selected component system first, and verify current releases, maintenance, licenses and hosted limits at adoption. Browser libraries do not automatically support React Native. `stack.md` remains the source for core architecture, Chart.js, hosting and mobile defaults.

## Forms, validation, APIs and utilities

| Tool | Decision | When to use it / boundary |
| --- | --- | --- |
| [Zod](https://zod.dev/) | Preferred runtime validation for custom TypeScript applications | Validate API inputs, forms, environment configuration and external data. Share schemas across web/mobile/server where useful; enforce validation on the server. Types alone do not validate runtime input or confer authorization. Avoid duplicating CMS/platform-owned schema rules. |
| [React Hook Form](https://react-hook-form.com/) + [Zod resolver](https://github.com/react-hook-form/resolvers) | Preferred for complex custom React forms | Use when field state, validation and dynamic forms justify it. Simple forms and an existing Mantine form solution may need nothing else. Use accessible labels and server error handling; check resolver/version compatibility. |
| [OpenAPI Generator](https://openapi-generator.tech/) | Optional contract-driven SDK generation | Useful for a documented HTTP API consumed by web, mobile or external clients, particularly across languages. Generate from one versioned OpenAPI specification, select a maintained generator and keep generated code reproducible in GitHub CI. Generated clients/stubs do not implement authorization or prove endpoint correctness. |
| [GraphQL](https://graphql.org/) | Requirement-driven API alternative | Consider when clients need flexible queries across connected data or an existing backend exposes GraphQL. Keep HTTP/REST APIs as the straightforward starting point. Account for resolver authorization, query complexity/rate limits, caching and schema ownership. GraphQL is an API approach, not another database or automatic service. |
| [dotenv](https://github.com/motdotla/dotenv) | Optional for standalone Node scripts/services | Use only when the runtime/framework does not already load local environment files. Production values come from the chosen deployment secret store. Keep real .env files out of Git and frontend bundles; commit a safe .env.example. |
| [Lodash](https://lodash.com/) | Optional targeted utilities | Prefer built-in JavaScript APIs for simple operations. Use selected imports for genuinely useful helpers; do not add the entire library to every client bundle. |
| [Airbnb JavaScript guide](https://github.com/airbnb/javascript) | Reference or existing-project convention | Preserve the repository's lint/format configuration. Do not impose Airbnb's full toolchain over current TypeScript/framework conventions merely because the guide is available. |
| [Luxon](https://github.com/moment/luxon) | Preferred optional complex date/time handling | Useful for timezone-aware schedules, durations and DST-sensitive workflows. Store instants consistently and retain the intended IANA timezone for local recurring schedules. Test DST boundaries. |
| [Day.js](https://day.js.org/) | Optional lightweight date utility | Use for simpler date formatting/manipulation or an existing integration. Native Intl may suffice. Check required timezone/UTC plugins and use one date library per application unless a dependency requires another. |

## Models, moderation and interactive media

| Tool | Decision | When to use it / boundary |
| --- | --- | --- |
| [Three.js](https://threejs.org/) | Preferred optional custom browser 3D | Use for model viewers, product previews and interactive 3D scenes. Prefer glTF/GLB for web delivery where appropriate; convert other model formats deliberately. Lazy-load the viewer and large assets, optimise textures/geometry, cap rendering work and provide a static fallback. A viewer does not validate printability, dimensions or manufacture pricing. |
| [NSFWJS](https://github.com/infinitered/nsfwjs) | Optional image-moderation signal | Useful for upload triage or client-side previews using TensorFlow.js. Benchmark thresholds and false positives/negatives on representative images. Browser checks are bypassable: enforce any publication gate on the backend and route uncertain cases to review. It is not age verification, consent verification or a detector of all prohibited content. Budget model download and inference work; do not infer video-wide coverage from a sampled frame. |
| [Tone.js](https://tonejs.github.io/) | Optional interactive audio | Use for synthesis, sequencing or music/audio tools. Plain playback does not need it. Respect browser audio activation requirements and provide mute/volume controls. |
| [p5.js](https://p5js.org/) | Optional creative coding | Use for generative art, experiments and interactive 2D sketches. Choose Three.js for substantial 3D model presentation. Measure rendering cost and provide reduced-motion/fallback behaviour. |

### Jev and Clef decision models

Keep [TypeSafe Jev](https://typesafe.ai/blog/introducing-system-one-models-and-jev) and [Cloudflare Clef/Clef-flash](https://blog.cloudflare.com/clef-decision-models/) as **experimental, optional decision-model candidates** for classification, routing, prioritisation and bounded choices. They are not default chat/generation models or replacements for deterministic business rules. Clef provides Apache-2.0 weights for local deployment and a Workers AI hosted route; Jev is a hosted API candidate. Verify current availability, model terms and API pricing rather than treating either hosted route as free.

For cost-sensitive projects, evaluate Clef's portable weights where existing hardware can run them, alongside hosted inference; do not provision a GPU server merely to add it to the toolbelt. Keep model calls behind a provider adapter with versioned inputs, outputs, timeout/retry limits and usage budgets. Evaluate accuracy and probability calibration on representative labelled examples, including ambiguous cases. Typed outputs guarantee shape only; they do not guarantee a correct decision. Apply confidence thresholds, an abstain/fallback path and review for consequential actions. Application authorization and transactional rules remain deterministic server responsibilities. Models used for moderation need their own task evaluation; neither replaces NSFWJS by assumption.

## Animation and data visualisation

| Tool | Decision | When to use it / boundary |
| --- | --- | --- |
| [Motion](https://motion.dev/) | Optional React animation alternative | Useful for React component transitions, layout and interaction animation. Choose its open-source library when it fits better than the main GSAP/Anime.js defaults; paid Motion products are optional. Select one primary motion library for a frontend. |
| [react-spring](https://www.react-spring.dev/) | Specialist alternative | Consider for a concrete physics/spring interaction requirement or an existing integration. Do not install alongside Motion/GSAP/Anime.js without a distinct need. |
| [D3](https://d3js.org/) | Optional bespoke visualisation | Use for custom layouts, unusual interactions or visualisations Chart.js cannot reasonably express. Chart.js remains the default for standard charts. Use D3 modules selectively and define accessible equivalents. |
| [Locomotive Scroll](https://scroll.locomotive.ca/) | Design-specific exception | Only when a demonstrated scrolling/parallax requirement warrants it. Prefer native scrolling and existing motion tooling; test keyboard navigation, anchors, restoration, touch and reduced motion. |
| [fullPage.js](https://github.com/alvarotrigo/fullPage.js) | Licensing/design exception | Use only for an explicit full-screen section experience. Check GPLv3 compatibility or obtain the appropriate commercial license. Do not assume it is free for closed-source commercial sites; native CSS scroll snapping may meet the need. |

## Tables, drag-and-drop, galleries and maps

Use the selected shadcn/Kibo, Mantine or native HTML component first. Add specialist libraries for missing capabilities, not as competing complete UI systems. Provide keyboard/button alternatives for drag operations and test touch interactions.

| Tool | Decision | When to use it / boundary |
| --- | --- | --- |
| [SortableJS](https://sortablejs.github.io/Sortable/) | Optional simple DOM list sorting | Suitable for framework-light reorderable lists. Persist authorised order changes on the backend; ensure DOM manipulation fits the framework and offer non-drag controls. |
| [React DnD](https://react-dnd.github.io/react-dnd/about/) | Optional complex React drag/drop | Consider for richer drag sources, targets and workflows when the existing UI toolkit is insufficient. Verify maintained releases and mouse/touch/backend integration. |
| [Shopify Draggable](https://github.com/Shopify/draggable) | Existing-project/specialist alternative | Not a new default: its original authors/Shopify no longer maintain it. Check current collaborator activity and accessibility support before selection; prefer a supported solution meeting the requirement. |
| [Hammer.js](https://hammerjs.github.io/getting-started/) | Existing-project exception | Its documentation describes low-maintenance mode. Prefer Pointer Events, native touch behaviour or a supported component's gesture handling for new projects. |
| [AG Grid](https://www.ag-grid.com/) | Optional advanced data grid | Use Community when its free features satisfy editing, filtering and data-volume requirements. Enterprise features require a production license; check the feature split before committing to the UI. Preserve existing component tables for ordinary lists. |
| [Grid.js](https://gridjs.io/) | Optional framework-light table | Consider for standalone/non-React tabular interfaces. Verify maintenance and required features at adoption; avoid adding it beside an adequate application table system. |
| [React Datepicker](https://reactdatepicker.com/) | Optional missing date-input capability | Use only when the chosen component system's date picker is inadequate. Check accessibility, locale/timezone behaviour and mobile usability. |
| [TOAST UI](https://ui.toast.com/) | Select individual specialist packages | Its Markdown editor, calendar or image editor may fill a concrete gap. Do not adopt the whole suite or replace the CMS editor by default. Check each package's maintenance, dependencies and optional usage-statistics behaviour. |
| [Swiper](https://swiperjs.com/) | Optional advanced galleries/carousels | Use for touch galleries or carousel behaviour beyond the selected UI toolkit/CSS. Import needed modules only, provide accessible navigation and avoid forced autoplay. It is a web library despite the mobile-friendly name. |
| [Leaflet](https://leafletjs.com/) | Preferred optional straightforward web maps | Useful for markers, locations and basic overlays. The library is free; tile hosting, geocoding and routing are separate services with limits, attribution and costs. Do not assume public OpenStreetMap tiles provide an unrestricted production backend. |
| [Mapbox GL JS](https://docs.mapbox.com/mapbox-gl-js/guides/) | Requirement-driven hosted mapping alternative | Choose for justified vector-map/style/location features. Review [Mapbox pricing](https://www.mapbox.com/pricing), token restrictions, attribution and licensing; avoid an automatic paid dependency when Leaflet meets the requirement. |
| [Formik](https://formik.org/) | Preserve suitable existing forms | React Hook Form is the preferred new complex React form choice. Do not migrate working Formik forms solely to standardise a library name. |

## Testing and performance

| Tool/resource | Decision | How to use it |
| --- | --- | --- |
| [Playwright](https://playwright.dev/docs/intro) | Preferred new browser journey testing | Test critical login, forms and purchase/account flows in GitHub CI. Preserve an adequate existing test framework. |
| [Cypress](https://www.cypress.io/) | Valid testing alternative | Retain existing suites or choose for a documented fit. Local/CI tests need not use Cypress Cloud; hosted recording/orchestration is optional. Do not install both Cypress and Playwright without a distinct testing requirement. |
| [Lighthouse](https://developer.chrome.com/docs/lighthouse/overview/) | Preferred local/CI audit | Use for performance, accessibility, SEO and best-practice diagnostics. Run against representative built pages; CI work belongs on GitHub runners, not shared production hosts. Scores are lab measurements, not proof of all-user performance or accessibility compliance. |
| [PageSpeed Insights](https://pagespeed.web.dev/) | Preferred public production check | Review lab diagnostics alongside real-user CrUX data where available. Field data reflects a historical period; absence of field data is not a pass or failure. Do not expose protected staging to make an external test work. |
| [WebPageTest](https://www.webpagetest.org/) | Optional deeper investigation | Use waterfalls, repeat visits, filmstrips and relevant device/network/location conditions when simpler diagnostics leave a question unresolved. Check current hosted/API allowances before automation; own infrastructure also has costs. |
| [GTmetrix](https://gtmetrix.com/) | Optional second opinion/reporting | Useful when its reports or client-facing workflow add value. Verify free-account restrictions and paid monitoring requirements; do not automatically subscribe to overlapping monitoring. |
| [web.dev performance learning](https://web.dev/learn/performance/) and [fast-load guidance](https://web.dev/explore/fast) | Current learning references | Start here for modern loading, rendering, images, fonts and performance measurement. Validate advice against the selected framework. |
| [High Performance Browser Networking](https://hpbn.co/) | Background reference | Useful networking concepts; verify older protocol/browser advice against current official documentation. |
| [Smashing Magazine 2021 checklist](https://www.smashingmagazine.com/2021/01/front-end-performance-2021-free-pdf-checklist/) | Historical reference | Use as a source of ideas, not a current release checklist. Re-check individual recommendations. |

The supplied SitePoint book URL and CSS-Tricks lazy-loading URL could not be retrieved during this review. Do not retain them as operational dependencies or authoritative current instructions. Use maintained web.dev/framework documentation for media loading instead. Set project-specific budgets for payloads and user experience; measure mobile/network conditions and regressions rather than chasing a single perfect score.
