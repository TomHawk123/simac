# simac.io

Marketing site for Simac, a multi-cloud CNAPP platform. The site is plain static HTML and CSS with no build step, published to GitHub Pages at `simac.io`.

```
site/                          Static website (published as-is)
.github/workflows/pages.yml    Deploys site/ to GitHub Pages on every push to main
```

## Preview locally

```sh
python3 -m http.server -d site 8000
# open http://localhost:8000
```

## First-time setup

1. **Push the repo to GitHub.**

   ```sh
   gh repo create simac --public --source . --push
   ```

   Pages on a private repo requires a paid GitHub plan.

2. **Turn on Pages.** Go to *Settings → Pages → Build and deployment*, set **Source** to **GitHub Actions**, and then re-run the workflow from the *Actions* tab (or push a change).

3. **Verify the domain (recommended).** This stops anyone else from claiming `simac.io` on GitHub Pages. Go to your GitHub account's *Settings → Pages → Add a domain*, enter `simac.io`, and add the TXT record it shows you in GoDaddy DNS.

4. **Add the DNS records in GoDaddy** (*My Products → simac.io → DNS*). Delete any existing `A` record for `@` (often a GoDaddy "Parked" record) and any existing `www` record first.

   | Type  | Name | Value                  |
   |-------|------|------------------------|
   | A     | @    | 185.199.108.153        |
   | A     | @    | 185.199.109.153        |
   | A     | @    | 185.199.110.153        |
   | A     | @    | 185.199.111.153        |
   | AAAA  | @    | 2606:50c0:8000::153    |
   | AAAA  | @    | 2606:50c0:8001::153    |
   | AAAA  | @    | 2606:50c0:8002::153    |
   | AAAA  | @    | 2606:50c0:8003::153    |
   | CNAME | www  | TomHawk123.github.io   |

5. **Set the custom domain.** In the repo's *Settings → Pages*, enter `simac.io` under **Custom domain** and save. Once the DNS check passes and the certificate is issued (usually within an hour), tick **Enforce HTTPS**. GitHub redirects `www.simac.io` to `simac.io` automatically.

## Deploying changes

Edit files in `site/` and push to `main`. The workflow publishes the site in about a minute.

## Notes

- **Custom domain lives in repo settings,** not in a `CNAME` file. Sites deployed with GitHub Actions ignore `CNAME` files.
- **Paths:** `index.html` uses relative paths, so it also works at the `https://tomhawk123.github.io/simac/` preview URL. `404.html` uses root-absolute paths because it's served at any URL, so it only renders correctly on the custom domain.
- **Security headers:** GitHub Pages can't set custom response headers. A Content-Security-Policy is set with a `<meta>` tag in each page instead. If you add third-party scripts or a form service later, update those tags.
- **No contact info yet.** The site has no email address or form.
