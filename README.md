# vkgraph

**The goal** is to provide comprehensive coverage of:
- packages, versions, and dependencies;
- public exports;
- API events;
- documentation;
- policies/CI;
- tokens and assets;
- cross-repository relationships.

**The strategy** involves using facets F1–F8 and the Compact Grouped XML format (BEGIN_XML/END_XML).

**The task** is to link the graph

# VK Ecosystem — Consolidated Knowledge Graph (RAG Index)

## Purpose
- This repository aggregates the latest slices of official VKCOM repos to help a RAG system build a complete, cross-repo knowledge graph of the VK Mini Apps ecosystem.
- This README is a machine- and human-oriented guide that sets goals, vocabulary, output format, sections to produce, and quality gates. It is a “compass” for the indexer: what to analyze, how to normalize data, and how to output it.

## Scope (included official repos)
- UI
  - VKCOM/VKUI
  - VKCOM/vkui-tokens (referenced as external dependency)
- Mini Apps SDK
  - VKCOM/vk-bridge
  - VKCOM/vk-bridge-react
- Dev tooling
  - VKCOM/create-vk-mini-app
  - VKCOM/vk-tunnel-client (package: @vkontakte/vk-tunnel)
  - VKCOM/vk-qr (package: @vkontakte/vk-qr)
- API schema and types
  - VKCOM/vk-api-schema (package: @vkontakte/api-schema)
  - VKCOM/api-schema-typescript (package: @vkontakte/api-schema-typescript)
  - VKCOM/api-schema-typescript-generator (package: @vkontakte/api-schema-typescript-generator)
  - VKCOM/vk-mini-apps-api (package: @vkontakte/vk-mini-apps-api)
- Examples and templates
  - VKCOM/vk-mini-apps-examples
- Code quality configs
  - VKCOM/eslint-plugin (package: @vkontakte/eslint-plugin)
  - VKCOM/stylelint-config (package: @vkontakte/stylelint-config)

## Repository layout (suggested)
- slices/
  - VKCOM/VKUI/… (code and docs snapshot)
  - VKCOM/vk-bridge/…
  - VKCOM/vk-bridge-react/…
  - …
- seeds/ (optional)
  - f1_vkui_xml_like.md (baseline compact XML for F1, see “Seed: Patched F1 (VKUI)”)
- README.md (this file)

## Primary goal
- Build a total, cross-repo knowledge graph that captures:
  - Packages, versions, dependencies (runtime/peer/dev), and inter-package relations.
  - Public exports (components/hooks) for VKUI.
  - VK Bridge API events (ae), outcomes (ev), and typed results (ty).
  - Docs mapping and website anchors.
  - Policies, lint rules, CI, invariants.
  - Tokens, assets, platforms, schemes.
  - Examples, templates, navigation semantics, security patterns.
- Output MUST use the Compact Grouped XML format described below (token-efficient).

## Compact Grouped XML format (mandatory)
- Global envelope:
  BEGIN_XML
  <graph>
    … groups …
  </graph>
  END_XML
- Groups of nodes:
  <nodes k="…" l="…" [idPrefix="…"] fmt="csv_pipe">
  header|header2|…
  row|row2|…
  </nodes>
  - k (node kind code):
    - r = repo
    - p = package (npm package)
    - x = external dependency (npm)
    - co = component (UI component export)
    - hk = hook (React hook export)
    - ae = API event (e.g., VKWebAppInit)
    - ev = API event outcome (result/failure)
    - ty = type (TS type/interface)
    - rt = router (navigation signal)
    - sv = service (external API service, e.g., vk-api endpoints)
    - po = policy (code quality, theming, a11y)
    - ln = lint (lint rule)
    - iv = invariant (non-lint rule invariant)
    - sec = security pattern (e.g., URLSearchParams)
    - ci = CI pipeline node (eslint/stylelint/typecheck/test)
    - tok = tokens (design tokens set)
    - as = asset (CSS, etc.)
    - st = site (documentation site)
    - pl = platform (ios, android, vkcom)
    - sch = scheme (light, dark)
    - dc = doc (documentation “virtual page handle”)
    - tm = template (scaffold template)
    - fi = file (source or example file)
    - app = application (example app)
  - l (layer): env | providers | tooling | docs | observability | policies | api | navigation | security
  - idPrefix (optional): prefix applied to all i values in the body, e.g. pkg:@vkontakte/
  - Table headers (csv_pipe):
    - i (id, required)
    - v (version; for k=p)
    - path (file path; for k=as or k=fi)
    - ci (boolean; for repo nodes if needed)
- Groups of edges:
  <edges t="…" s="…" [sc="…"] [destPrefix="…"] fmt="csv_pipe">
  i
  target-id
  </edges>
  - t (edge type code):
    - ct = contains
    - dp = depends_on (use sc)
    - ex = exposes (pkg → comp/hook or pkg → ae/ev/ty)
    - us = uses
    - cp = composes
    - rq = requires_css
    - ut = uses_tokens
    - co = conditional_on (platform/scheme-based conditions)
    - dc = docs (node → doc page)
    - cb = checked_by (repo → ci)
    - ef = enforces (repo → policy)
    - cw = complies_with (pkg → policy)
    - tb = typed_by (event/outcome → type)
    - ig = integrates (pkg → service)
    - em = emits (app → event)
    - ui = used_in (comp/hook → file)
    - nv = navigates (file → router)
    - rs = res (event → outcome result)
    - fl = fail (event → outcome failure)
    - scs = sec_src (security source → file)
  - s = full source id
  - sc (scope for dp): runtime | peer | dev | external
  - destPrefix (optional): id prefix for i values

## ID conventions
- repo ids: repo:<RepoName>, e.g., repo:VKUI, repo:vk-bridge
- package ids: pkg:@vkontakte/<name> (no version in id)
- external deps: ext:<npm-name> (preserve scope, e.g., ext:@floating-ui/react-dom)
- components: comp:<package-scope>:<ComponentName> (e.g., comp:vkui:Button)
- hooks: hook:<package-scope>:<HookName> (e.g., hook:vkui:useAdaptivity)
- api events: ae:<EventName>, outcomes: ev:<OutcomeName>, types: ty:<Name>
- docs: doc:/<route>.txt (virtual handle for documentation pages)
- site: site:<host>, e.g., site:vkui.io
- file: fi:<repo>/<relative_path> (to avoid collisions across repos)
- tokens: tokens:<name>, assets: asset:<name>
- router: router:<action>, service: svc:<name>, policy: pol:<name>, lint: lint:<engine:rule>, invariant: invariant:<name>
- platform: platform:<name>, scheme: scheme:<name>, app: app:<name>, security: sec:<pattern>

## Normalization rules
- Versions: attribute v on k=p nodes (from package.json).
- dp edges (depends_on):
  - sc=peer for peerDependencies
  - sc=runtime for dependencies
  - sc=dev for devDependencies
- Use idPrefix to minimize token footprint in grouped nodes/edges.
- Public exports only:
  - VKUI: components/hooks from the public entrypoint.
  - vk-bridge: events and outcomes actually exposed/typed.
- Deduplicate by id at merge time. Chunks (PART k/n) allowed.

## Facet plan (what to create)
- F1. Repo/Packages/Deps/Policies/CI (per repo and cross-repo)
  - Nodes: repo, site, packages (with versions), external deps, tokens, assets, policies, lint, invariants, ci, platforms, schemes.
  - Edges: contains (repo→packages), depends_on (pkg→ext/pkg), checked_by (repo→ci), enforces (repo→policy), policy contains (lint/invariant/asset), complies_with/uses_tokens/requires_css for key packages.
  - Output: One or multiple compact XML blocks per repo; cross-repo union allowed.
- F2. Public exports (components/hooks) for VKUI
  - Nodes: comp:vkui:*, hook:vkui:*
  - Edges: exposes (pkg:@vkontakte/vkui→comp/hook), requires_css, uses_tokens, composes/uses, conditional_on (platform/scheme), depends_on (component→provider) where applicable.
- F3. Docs mapping
  - Nodes: doc:/components/*.txt, doc:/overview/*.txt, site:vkui.io
  - Edges: docs (comp/hook→doc), docs (doc→site).
- F4. Theming/Tokens/Assets/Invariants
  - Nodes: tokens:vkui, asset:vkui_css, platforms, schemes, invariants.
  - Edges: requires_css, uses_tokens, conditional_on, complies_with/enforces/contains (policy→lint/invariant/asset).
- F5. Dependencies and peer deps (all repos)
  - Detailed dp grouping for packages (internal and external), with sc.
- F6. Policies/Lint/CI (all repos)
  - Nodes: pol, ln, iv, ci; Edges: ef, cb, ct, cw.
- F7. Examples/Templates/Used-in/Navigates
  - Nodes: tm:* (templates), fi:* (example/stories files), router:* (actions), app:* (example app).
  - Edges: used_in (comp/hook→file), emits (app→event), navigates (file→router).
- F8. API events (vk-bridge) and VK API schema
  - Nodes: ae:*, ev:*, ty:*, svc:*.
  - Edges: exposes (pkg→ae/ev), res/fail (ae→ev), typed_by (ev→ty), integrates (schema generators→svc).

## Quality gates (required)
- Coverage
  - All packages present with correct versions.
  - All dp edges have correct sc.
  - VKUI exports cover all public components/hooks from entrypoint.
  - vk-bridge events/outcomes/typed_by mapped exhaustively.
  - Docs mapping present for major components/hooks.
- Evidence (outside XML)
  - Provide file paths and excerpts where possible (ignored by parser but used for auditing).
- Chunking
  - If output too large, split into PART k/n. Each part must contain a valid BEGIN_XML/END_XML block.
- Consistency
  - Only Compact Grouped XML outputs. No other formats.

## Seed: Patched F1 (VKUI) — baseline example
- The consolidated graph already includes a patched F1 for VKUI (used as a seed and format reference). Key points:
  - repo:VKUI, site:vkui.io, packages with versions, external deps grouped, policies/CI nodes.
  - dp edges for vkui, vkui-floating-ui, vkui-docs-theme, vkui-codemods.
  - Corrections:
    - vkui-date-fns-tz has NO runtime/peer deps; dev deps only:
      - ext:@date-fns/tz, ext:@swc/core, ext:typescript (dp sc=dev).
    - Removed non-rule lint:eslint:react-hooks/recommended (and its policy link).
- Example delta patch applied:
  BEGIN_XML
  <graph>
    <nodes k="x" l="tooling" idPrefix="ext:" fmt="csv_pipe">
i
@date-fns/tz
    </nodes>
    <edges t="dp" s="pkg:@vkontakte/vkui-date-fns-tz" sc="dev" destPrefix="ext:" fmt="csv_pipe">
i
@date-fns/tz
@swc/core
typescript
    </edges>
  </graph>
  END_XML
  And removal patch:
  BEGIN_XML
  <graph op="remove">
    <nodes k="ln" l="policies" idPrefix="lint:eslint:" fmt="csv_pipe">
i
react-hooks/recommended
    </nodes>
    <edges t="ct" s="pol:code_quality" destPrefix="lint:eslint:" fmt="csv_pipe">
i
react-hooks/recommended
    </edges>
  </graph>
  END_XML

## How to use this README (for the agent)
- Read “Format” and “Facet plan” to understand the extraction goals and output syntax.
- For each repo slice in slices/VKCOM/*:
  - Produce F1 first (packages/versions/deps/policies/ci).
  - Then F2–F8 as applicable (VKUI: F2/F3/F4; vk-bridge: F8; templates/examples: F7; schema repos: F8 integrations).
- Always output Compact Grouped XML between BEGIN_XML/END_XML.
- Provide evidence and notes outside XML (optional but encouraged).
- If limits hit — chunk output and continue (PART k/n).

## Conventions and reminders
- Preserve scoped package names in ids (ext:@scope/name).
- Public exports only (validate via entrypoints/exports).
- Use idPrefix aggressively to minimize token footprint.
- Use fi:<repo>/<path> to disambiguate files across repos.
- Keep edge semantics consistent (t codes), and always set sc for dp edges.

## Deliverables
- A set of BEGIN_XML/END_XML blocks per facet, ready for scripted ingestion and conversion to the target graph.

# Agent direction
По репозиториям в slices/VKCOM/* сгенерируй (по каждому) релевантные фасеты из набора F1–F8 в формате Compact Grouped XML (BEGIN_XML/END_XML), разбивая на PART κ/ν при необходимости