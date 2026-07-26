# CLASP Studio — website

One file, no build step, no dependencies: `index.html`. Everything runs in the visitor's browser.

## Put it online

Pick one. All three are free and give you a real URL you can print on a poster.

### GitHub Pages
```bash
cd /Users/jasper/Downloads/wall/site
git init && git add . && git commit -m "CLASP Studio"
gh repo create clasp-studio --public --source=. --push
gh api -X POST repos/:owner/clasp-studio/pages -f source[branch]=main -f source[path]=/
```
Live at `https://<your-github-username>.github.io/clasp-studio/` in about a minute.

### Netlify
```bash
cd /Users/jasper/Downloads/wall/site
npx netlify-cli deploy --prod --dir .
```

### Vercel
```bash
cd /Users/jasper/Downloads/wall/site
npx vercel --prod
```

Or drag this folder onto [app.netlify.com/drop](https://app.netlify.com/drop) — no terminal, no account needed for a first deploy.

## Custom domain

All three hosts take a custom domain in their dashboard. If you buy something like `claspstudio.org`,
point it there and the URL on your ISEF board stops looking like a subdomain.

## Test locally

```bash
cd /Users/jasper/Downloads/wall/site && python3 -m http.server 8000
```
Then open `http://localhost:8000`.

## What is inside

| Part | What it does |
| --- | --- |
| Direct stiffness solver | Assembles the global stiffness matrix, applies supports, solves for joint displacements, recovers axial force in every stick. Detects mechanisms as a singular matrix. |
| Member checks | Crushing, Euler buckling, and joint pull-out, per stick. |
| Progressive collapse | Ramps the load, breaks the worst stick, re-solves without it, repeats. Produces a computed failure sequence, not a scripted one. |
| Position-based dynamics | Joints become particles, sticks become distance constraints. Runs the collapse under gravity with ground contact. Broken sticks detach as debris. |
| Robot | Two-link arm solved with inverse kinematics (cosine rule), climbing carriage, claw that opens and closes on each stick. |

## Editing it

It is plain HTML, CSS and JavaScript in one file — open it in any editor. The tuneable constants sit
near the top of the `<script>`:

- `MATS` — material properties for pine and bamboo (E, strengths, density, joint capacity)
- `ROBOT` — mass, reach, clamp moment capacity, arm segment lengths
- `PHYS` — physics substeps, solver iterations, damping, gravity

Changing a number in `MATS` changes every result on the page, because nothing is hard-coded downstream.

## Honesty notes for the fair

Material and joint values are literature estimates for model-scale stock, not measurements. Every joint
is modelled as a frictionless pin, so real screwed and lashed joints are stiffer than this. Say both
out loud before a judge asks.
