# Undergraduate Research Seminar in Mathematics — site

A Jekyll site: schedule, archive, about/contact, and a mailing-list signup
callout. Deploys via GitHub Pages. Speakers are invited directly by the
organising committee — there's no public "propose a talk" page.

## Before you push anywhere: three placeholders to fill in

Open `_config.yml` and replace:

- `contact_email` — the organisers' inbox
- `mailing_list_url` — see "Setting up the mailing list" below

## Setting up the mailing list

GitHub Pages is static — it can't collect signups itself, so this needs an
external service. Pick one:

- **JISCMail** — the standard UK academic mailing list service. Worth
  checking if QMUL already has infrastructure/precedent for this.
- **Google Group** — simplest to set up, `mailing_list_url` just points at
  the group's join page.
- **Google Form → Mailchimp/Buttondown** — more control over onboarding
  emails and unsubscribes, more setup.

Whichever you pick, `mailing_list_url` in `_config.yml` is the only place
you need to point at it — it's used on the homepage and schedule page.

## Running it locally

You'll need Ruby installed, then:

```bash
bundle install
bundle exec jekyll serve
```

Visit `http://localhost:4000`. `jekyll serve` auto-rebuilds when you save a
file, so keep it running while you edit.

## Adding a talk

Add a new file to `_talks/`, named anything ending in `.md`, e.g.
`_talks/2026-12-02-your-title-slug.md`:

```yaml
---
title: "Your Talk Title"
speaker: "Dr. Speaker Name"
affiliation: "Postdoctoral Researcher, School of Mathematical Sciences"
date: 2026-12-02
abstract: >
  One or two sentences describing the talk. This is what shows on the
  schedule card and gets truncated — keep the first sentence strong.
slides: "https://..."     # optional, add once available
recording: "https://..."  # optional, add once available
---

Anything you write below the `---` shows on the talk's own page, under
the abstract. Optional — leave blank if the front matter is enough.
```

`time` and `room` are optional — leave them out and the talk inherits
`schedule_time`/`schedule_room` from `_config.yml`. Only set them on a
specific talk if that session runs at a different time or in a different
room than usual (e.g. a one-off room change).

That's it — it'll automatically appear on the homepage (if it's one of the
next three upcoming) and on `/schedule.html`, and move itself to
`/archive.html` once the date passes. No need to manually move files
between "upcoming" and "past."

## Adding an organiser

Add a new file to `_organisers/`, named `firstname-lastname.md` (this exact
name is also the photo-matching key, see below):

```yaml
---
name: "Jane Doe"
role: "PhD Student, School of Mathematical Sciences"
order: 2   # controls left-to-right/top-to-bottom position on the about page
---

A couple of sentences of bio — what they study, what they're interested in.
This is markdown and becomes the card's blurb on the about page.
```

To add a photo, drop a JPEG at `assets/images/organisers/firstname-lastname.jpg`
— same basename as the markdown file, no front matter needed to wire it up.
No photo yet? Leave it out; the card falls back to an initials monogram
until one's added.

That's it — the about page picks up every file in `_organisers/`
automatically, sorted by `order`. To remove someone, delete their file (and
photo, if any).

## Deploying — starting on your own GitHub account

1. Push this repo to a new repository on `github.com` (personal account or
   a free org).
2. In the repo's **Settings → Pages**, set the source to the `main` branch
   (root). GitHub will build and publish it automatically — no config
   changes needed, the defaults in `_config.yml` (`url: ""`, `baseurl: ""`)
   work for both a custom domain and a `username.github.io/repo-name`
   project URL, since all internal links use Jekyll's `relative_url` filter.
3. Wait a minute or two, then check `https://<username>.github.io/<repo>/`.

## Migrating to QMUL's GitHub Enterprise later

Once QMUL confirms GitHub Pages is enabled and public on
`github.qmul.ac.uk` (ask IT/the instance admin — GitHub Enterprise Server
requires an admin to switch Pages to public-visible, otherwise sites are
only viewable to people with QMUL logins):

1. Create a new repo under the appropriate QMUL org on `github.qmul.ac.uk`.
2. Add it as a second git remote and push:
   ```bash
   git remote add qmul https://github.qmul.ac.uk/YOUR-ORG/YOUR-REPO.git
   git push qmul main
   ```
3. Enable Pages in that repo's settings.
4. If the resulting URL structure differs (e.g. a different subpath),
   update only the `url`/`baseurl` lines in `_config.yml` — nothing else
   in the site needs to change, since no content hardcodes a domain.
5. Once you're happy with it, redirect or retire the original GitHub.com
   site (a one-line note + link in its README is enough).

You can also keep both running simultaneously for a while — push to both
remotes — if you want to test the QMUL instance before fully switching over.

## Structure

```
_config.yml       site-wide settings — title, term, mailing list URL
_talks/           one file per talk (see "Adding a talk" above)
_organisers/      one file per organiser (see "Adding an organiser" above)
_layouts/         default.html (all pages), talk.html (individual talk pages)
_includes/        header, footer, <head>, mailing-list signup box
assets/css/       stylesheet
assets/images/organisers/  organiser photos, matched by filename
index.html        homepage — hero + next 3 talks + signup
schedule.html     full list of upcoming talks
archive.html      past talks
about.html        blurb + organiser cards + contact
```

## Possible future additions

- `.ics` calendar feed people can subscribe to
- Tag-based filtering on the archive once it has enough entries
- A GitHub Action to auto-mirror pushes to both a public and QMUL remote
