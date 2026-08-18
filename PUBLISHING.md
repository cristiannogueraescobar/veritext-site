# Publishing the privacy policy

Every store demands a public privacy policy URL before it will accept the
extension. This is how to get one without making the code repository public.

## The constraint

GitHub Pages serves from **public** repositories on the free plan. Private
repositories only work on Pro, Team or Enterprise. On a free account with a
private repo, Pages will not build and the URL returns 404.

The plan below sidesteps that entirely: a second, tiny **public** repository
that contains nothing but the privacy page. The extension's code stays private.

Everything in `site/` is that repository's contents — three files, no build
step, no dependencies.

---

## Step by step (GitHub web interface, no command line)

**1. Create the public repository**

- github.com → **+** (top right) → **New repository**
- Name: `veritext-site`
- Visibility: **Public** ← this is the whole point
- Tick **Add a README file** (it gets replaced in a moment)
- **Create repository**

**2. Upload the three files**

- On the new repo's page: **Add file** → **Upload files**
- Drag in `index.html`, `README.md` and `.nojekyll` from `site/`
- Commit message: `Privacy policy`
- **Commit changes**

If the browser hides `.nojekyll` because it starts with a dot, use
**Add file → Create new file**, type `.nojekyll` as the name, leave it empty,
and commit. It stops GitHub from running the page through Jekyll, which would
otherwise ignore files beginning with an underscore. Harmless here, but it costs
nothing and removes a class of surprise.

**3. Turn on Pages**

- Repo → **Settings** → **Pages** (left sidebar)
- Source: **Deploy from a branch**
- Branch: `main`, folder: `/ (root)`
- **Save**

**4. Wait, then check**

The first build takes one to two minutes. Reload the Pages settings page until
it shows the live URL, then open it in a private window:

```
https://<your-username>.github.io/veritext-site/
```

You should see the VeriText mark and the policy. If you get a 404, wait another
minute and hard-reload — the first deploy is the slow one.

**5. Use it**

Paste that exact URL into:

- Chrome Web Store → Privacy practices → Privacy policy URL
- Edge Partner Center → Properties → Privacy policy URL
- AMO → the privacy policy field

---

## Same thing from the command line

```bash
# after creating the empty public repo on github.com
cd site
git init -b main
git add .
git commit -m "Privacy policy"
git remote add origin https://github.com/<your-username>/veritext-site.git
git push -u origin main
```

Then Settings → Pages → Deploy from a branch → `main` / `/ (root)`.

---

## Before you submit

- [ ] The URL loads in a **private window** — proves it is genuinely public and
      not just visible to you while logged in
- [ ] The date at the top of the page is right
- [ ] The contact address is one you actually read
- [ ] The URL is the *page*, not the repository. `github.com/you/veritext-site`
      is the repo and some reviewers reject it; `you.github.io/veritext-site/`
      is the policy

## Keeping it in step with the extension

`store/PRIVACY.md` and `site/index.html` say the same thing in two formats. When
one changes, change the other. The obvious drift risk is the settings list in
"What VeriText stores" — it already gained `interface language` in Phase 4.

## If you would rather keep everything private

Cloudflare Pages, Netlify and Vercel all deploy from a **private** GitHub repo
on their free tiers, and the published site is public either way. That gets you
one repository instead of two, at the cost of a second account and a build
configuration. For a single static page, the extra public repo is less
machinery.

## A note on the domain

You already own `plumline.online`, and the policy lists
`contact@plumline.online` as the contact. If you would rather the URL matched
the product, a `.online` domain runs under a pound at Namecheap — the same route
you took for Plumline. Settings → Pages → Custom domain, add the DNS records,
tick **Enforce HTTPS**. Not required for any store; purely cosmetic.
