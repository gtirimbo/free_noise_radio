# FREE NOISE RADIO: the manual

An online radio with a punk-zine look and a community spirit: any genre, any language, DIY, from the people to the people. Music in any genre to hear, poems and other writing to read, plus spoken word and book readings as audio. One rule: everything must be explained.

This manual covers everything: putting the site online, adding mixes and artists, changing text and looks, adding pages, receiving submissions, and keeping the whole thing alive.

**You do not need to be a programmer.** Most weekly jobs mean copying one block of text and changing a few words.

---

## Contents

1. [How the site is built](#1-how-the-site-is-built)
2. [Run it on your computer](#2-run-it-on-your-computer)
3. [Put it online](#3-put-it-online)
4. [Add a mix (the weekly job)](#4-add-a-mix-the-weekly-job)
5. [Add a text to the Read shelf](#5-add-a-text-to-the-read-shelf)
6. [Run the site from a Google Sheet (no code editing)](#6-run-the-site-from-a-google-sheet-no-code-editing)
7. [Which links work](#7-which-links-work)
8. [Hosting your own audio files](#8-hosting-your-own-audio-files)
9. [Playing with the screen off](#9-playing-with-the-screen-off)
10. [Artists and artist pages](#10-artists-and-artist-pages)
11. [Genres](#11-genres)
12. [Receiving submissions and messages](#12-receiving-submissions-and-messages)
13. [Donate and membership links](#13-donate-and-membership-links)
14. [Editing text](#14-editing-text)
15. [Changing the look](#15-changing-the-look)
16. [Add a new page](#16-add-a-new-page)
17. [Install on a phone like an app](#17-install-on-a-phone-like-an-app)
18. [Maintenance routine](#18-maintenance-routine)
19. [Rights, privacy and takedowns](#19-rights-privacy-and-takedowns)
20. [Troubleshooting](#20-troubleshooting)
21. [Growing later](#21-growing-later)

---

## 1. How the site is built

The whole site is **one file: `index.html`**. It contains the pages, the styling and the code. There is no database and no server to run.

```
your-repo/
  index.html      the whole website
  README.md       this manual
  free-noise-radio-content.xlsx   content workbook for Google Sheets (section 6)
  manifest.json   optional, for installing as an app (section 17)
  sw.js           optional, for installing as an app (section 17)
  audio/          optional, if you host mp3 files in the repo (section 8)
```

Inside `index.html` you will find three parts, in this order:

| Part | Starts with | What is in it |
|---|---|---|
| Styling | `<style>` | Colours, fonts, layout |
| Pages | `<body>` | The text and structure of every page |
| Code | `<script>` | **Your data** (mixes, artists, links) at the top, then the logic |

Most of your editing happens at the top of the `<script>` part, in a block marked **EDIT HERE**.

> **Tip:** Use a code editor such as VS Code (free) instead of Notepad. It colours the text and flags typos. You can also edit directly on github.com by opening the file and clicking the pencil icon.

---

## 2. Run it on your computer

**Quickest:** double-click `index.html`. It opens in your browser.

**Better** (behaves like the real website):

1. Open a terminal in the project folder.
2. Run `python3 -m http.server 8000`
3. Open `http://localhost:8000` in your browser.

Change something, save, refresh the browser. That is the whole workflow.

---

## 3. Put it online

Everything below has a free plan that is enough for this site. Audio hosting is the only thing that may cost money (section 8).

### Step 1: Put the files on GitHub

1. Create a free account at github.com.
2. Click **New repository**. Name it, for example, `free-noise-radio`.
3. Upload `index.html` and `README.md` (drag and drop in the browser works).

### Step 2: Publish the repo

Pick **one** of these:

**Cloudflare Pages** (recommended: fast, free, easy custom domain)
1. Create an account at cloudflare.com, open **Workers & Pages**, then **Create** and **Pages**, then **Connect to Git**.
2. Choose your repo.
3. Build command: leave empty. Output directory: `/` (or leave empty).
4. Click **Save and Deploy**. You get an address like `free-noise-radio.pages.dev`.

**GitHub Pages** (simplest, everything in one place)
1. In your repo, open **Settings**, then **Pages**.
2. Source: **Deploy from a branch**. Branch: `main`, folder `/ (root)`. Save.
3. After a minute the site is live at `https://YOUR-NAME.github.io/free-noise-radio/`.

**Netlify**: sign up, **Add new site**, **Import from Git**, choose your repo, leave the build settings empty.

From now on, **every time you save a change to the repo, the site updates itself** in about a minute.

### Step 3: Your own domain

1. Buy a domain at Namecheap, Porkbun or Cloudflare (roughly 10 to 15 EUR per year). Something like `freenoise.radio` or `freenoise.org`.
2. In your hosting dashboard (Cloudflare Pages, GitHub Pages or Netlify) open **Custom domains** and add it. The dashboard shows exactly which DNS records to create at your domain seller.
3. Wait from a few minutes up to a few hours. HTTPS (the padlock) is set up automatically.

> **Important:** Turn on **auto-renew** for the domain. A forgotten renewal is the most common way small sites die.

---

## 4. Add a mix (the weekly job)

> **Using the Google Sheet (section 6)?** Then add a row in the **Shows** tab instead. The fields below mean the same thing. This section describes the code way.

Open `index.html` and find this:

```js
const SHOWS = [
  { id:1, date:"2026-09-28", artist:"Dead Tapes", title:"Basement Static Vol. 1", ... },
  ...
];
```

Each mix is one block between curly braces `{ }`, followed by a comma. To add a mix, **copy an existing block, paste it at the top of the list, and change the values**:

```js
{
  id: 11,
  date: "2026-10-12",
  artist: "Marta Void",
  title: "Rain on Tin Roofs",
  genre: ["Ambient", "Field recording"],
  demo: false,
  url: "https://example.com/audio/rain-on-tin-roofs.mp3",
  desc: "Forty minutes of rain recorded on three different roofs, mixed into one slow piece. Best heard quietly."
},
```

### What each field means

| Field | What to put | Notes |
|---|---|---|
| `id` | A number nobody else uses | Does not need to be in order. Just never repeat one. |
| `date` | `"YYYY-MM-DD"` | The date filter uses it, and it is also the **release date**: a mix dated in the future stays hidden until that day (see "Schedule content in advance" in section 6). Keep the quotes. |
| `artist` | The artist name | **Must be spelled exactly the same** everywhere. It links the mix to the artist page. |
| `title` | The mix title | |
| `genre` | A list in square brackets | `["Punk"]` or `["Folk", "Spoken word"]`. Use as many as fit. Poems do not go here: they go on the Read shelf (section 5). |
| `demo` | `true` or `false` | `true` shows a "demo mode" message instead of a player. Set to `false` for real mixes. |
| `url` | One link to the audio | See [section 7](#7-which-links-work). |
| `desc` | A description | The core rule of the project: the mix is explained. Keep the quotes. |

### Common mistakes (these break the page)

- **Missing comma** between two blocks.
- **Missing quote** at the start or end of a text value.
- **A quote inside the text.** If your description contains `"`, write `\"` instead, or use a single quote `'`.
- **Different spelling** of an artist name (`"Dead Tapes"` vs `"Dead tapes"`).

If the page goes blank after an edit, you probably broke the punctuation. Undo the last change (see [Troubleshooting](#20-troubleshooting)).

### Remove or hide a mix

Delete its whole block. Or keep it and set `demo: true` to keep the card but remove the player. Removing is usually what you want for takedown requests.

---

## 5. Add a text to the Read shelf

> **Using the Google Sheet (section 6)?** Add a row in the **Texts** tab instead. The fields are the same.

The **Read** page shows texts (poems, stories, essays, letters, zine pages) as cassette tapes. Click a tape and the text opens as a photocopied zine page, with a ransom-note title and a taped "Why this is here" note.

Find this block in `index.html`:

```js
const TEXTS = [
  { id:1, date:"2026-10-03", author:"Ines Rook", title:"Rent Is Due", ... },
  ...
];
```

To add a text, copy a block, paste it at the top, and change the values:

```js
{
  id: 7,
  date: "2026-10-14",
  author: "Marta Void",
  title: "What The Tape Remembers",
  kind: "Poem",
  tags: ["tape", "memory"],
  note: "",
  intro: "Why this text exists, in a few honest sentences. When it was written, what to look for.",
  body: `first line of the poem
second line

new paragraph after a blank line
**this phrase is highlighted in yellow**
~~this phrase is struck out~~`
},
```

### What each field means

| Field | What to put | Notes |
|---|---|---|
| `id` | A number nobody else uses | Never repeat one. |
| `date` | `"YYYY-MM-DD"` | Used for sorting, the date filter, and as the **release date**: a text dated in the future stays hidden until that day. |
| `author` | Name or nickname | Spelled the same everywhere. It creates the artist page and the link to it. |
| `title` | The title | Shown as ransom-note letters on the reader page. |
| `kind` | `"Poem"`, `"Short story"`, `"Essay"`, `"Letter"`, `"Zine page"` or your own | New kinds appear as filter chips by themselves. |
| `tags` | A list, may be `[]` | Kept in the data for your own use. |
| `note` | A content note, or `""` | Shown in a dashed box above the text. Use it for heavy topics. |
| `intro` | Why this is here | **Required by the house rules.** It appears on the tape and in the yellow note. |
| `body` | The text between backticks | See below. |

### Writing the `body`

- The text goes between backticks (`` ` ``), not quotes. Line breaks are kept exactly as you type them, so poems work without any extra marks.
- A **blank line** starts a new paragraph.
- `**words**` becomes a yellow highlight. `~~words~~` becomes a strike-through with a pink line.
- **Never type a backtick or the characters `${` inside the text**, they break the page.
- The reading time on the tape is worked out from the word count.

### Always-offered types

```js
const CORE_KINDS = ["Poem","Short story","Essay","Letter","Zine page"];
```

Add names to this list to keep them in the filter and in the submit form even when no text uses them yet.

### What readers can do on a text

- Make the type larger or smaller (A- and A+).
- Print it as a zine page (the print layout hides everything except the text).
- Jump to the next or older text.
- Keep a mix playing while they read. The player bar stays at the bottom.

### Accepting a text from the submit form

1. Read it and check it against the rules on the **Submit a text** page (own words, human-made, explained, up to 1,500 words, no attacks on people).
2. Reply to the author.
3. Paste their text into a new block as above. If they used `**` or `~~`, keep them.
4. Check the page on your phone before moving on.

The sample texts that ship with the site (13 of them, including eight poems) are placeholders. Replace them before launch.

**Poems are texts, not audio.** A poem goes on the Read shelf with `kind: "Poem"`. The Listen page is for mixes and recordings. If a poet also records themselves reading, that recording can go on Listen under the genre `Spoken word`.

---

## 6. Run the site from a Google Sheet (no code editing)

Instead of editing `SHOWS`, `TEXTS` and `ARTISTS` in `index.html`, you can keep your content in a Google Sheet. The site reads the sheet every time someone opens it. Adding a mix becomes: add a row.

This is the recommended way to run the station once it is live. It also means other people (a co-curator, a volunteer) can add content without touching the code or the repo.

### What you get

- `free-noise-radio-content.xlsx`: a ready-made workbook with four tabs: **READ ME**, **Shows**, **Texts**, **Artists**. It is filled with the sample content so you can see the format.

### Set it up (about 1 hour)

1. Upload `free-noise-radio-content.xlsx` to Google Drive, open it, and choose **File, Save as Google Sheets**.
2. Replace the sample rows with your own (see the column guide below). Delete the samples you do not want.
3. **Publish each tab.** Open **File, Share, Publish to web**. Under *Link*, choose the tab (for example `Shows`), choose **Comma-separated values (.csv)**, click **Publish**, and copy the link. Do this for `Shows`, `Texts` and `Artists`. You end up with three links.
4. In `index.html`, find this line near the top of the script and paste the three links:
   ```js
   const SHEET = {
     shows:   "https://docs.google.com/spreadsheets/d/e/.../pub?gid=0&single=true&output=csv",
     texts:   "https://docs.google.com/spreadsheets/d/e/.../pub?gid=123&single=true&output=csv",
     artists: "https://docs.google.com/spreadsheets/d/e/.../pub?gid=456&single=true&output=csv"
   };
   ```
5. Save and publish the site. Open it and check that your rows appear.

From now on, change a row in the sheet and the site updates within a few minutes (Google refreshes published sheets on its own schedule). **You never need to touch `index.html` for content again.**

If `SHEET` is left empty, the site uses the data that is inside `index.html` instead. You can use one mode or the other.

> **Published to the web means public.** Anyone with the link can read the published tabs. That is what you want here, since the content is public anyway. Never put emails, private notes or unreleased links you want to keep secret in these three tabs. Keep private notes in a separate, unpublished tab.

### Column guide

**Shows tab**

| Column | What to put |
|---|---|
| `id` | A number nobody else uses. Do not change it once the mix is live. |
| `date` | `YYYY-MM-DD` |
| `artist` | Artist name, spelled consistently |
| `title` | Mix title |
| `genre` | Several allowed, separated by commas: `Punk, Lo-fi` |
| `url` | One link ([section 7](#7-which-links-work)). Must start with `https://`. Empty = "demo mode". |
| `desc` | The description |
| `published` | `TRUE` to show, `FALSE` to keep as a draft |

**Texts tab**: `id`, `date`, `author`, `title`, `kind`, `tags`, `note`, `intro`, `body`, `published`. Same meaning as in [section 5](#5-add-a-text-to-the-read-shelf). In the `body` cell press **Alt+Enter** (Windows) or **Ctrl+Option+Enter** (Mac) for a new line, and twice for a new paragraph. `**words**` and `~~words~~` work as before. Backticks are no longer a problem in this mode.

**Artists tab**: `name`, `place`, `bio`, `links`. Write links as `Label|https://link` and separate several with semicolons:
`SoundCloud|https://soundcloud.com/you; Instagram|https://instagram.com/you`

### Good habits

- **Do not rename the tabs or the header row.** The site finds columns by their header names.
- **Dates:** the date columns are formatted as plain text. If Sheets turns a date into something else, retype it as `2026-10-14`.
- **Drafts:** put `FALSE` in `published` while you are still working on a row, then change it to `TRUE`.
- **Links must start with `http://` or `https://`.** Anything else is ignored for safety.
- **One row, one thing.** A row without a title (or without an artist or author) is skipped.

### Schedule content in advance

The `date` of every mix and text is also its **release date**. If the date is in the future, the item stays hidden. On that day it appears by itself, with no action from you.

This lets you load a whole month of content in one sitting. For example, put one mix per week with dates a week apart, and the station keeps releasing them while you are away.

- The date is compared with the visitor's own calendar date, so a mix can appear a few hours earlier or later depending on their time zone.
- To check what is scheduled, add `?preview=1` to the address, for example `https://your-domain.com/?preview=1`. Scheduled items then show, marked "(scheduled)", and a banner says preview mode is on. Do not share this link: the hidden items are not secret (see the warning above about published sheets), but the surprise is part of the point.
- This works the same way for the code version (`SHOWS`, `TEXTS` in `index.html`) and for the sheet version.
- It hides items on the website only. The data is still readable in the published sheet, so never schedule anything you need to keep private.

### What happens if Google is slow or down

The site waits up to 8 seconds for the sheet. If it cannot get it, it shows the last version it saved in the visitor's browser, with a small note on top. A first-time visitor with no saved copy sees the sample content inside `index.html`. That is why, if you run from the sheet, you should empty the built-in samples before launch (set `SHOWS` and `TEXTS` to `[]`).

### Submissions into the sheet

Form submissions arrive by email (section 12). When you accept one, copy the details into a new row. If you want submissions to land in a sheet automatically, Formspree can send them to a Google Sheet through its integrations. Keep that as a **separate, unpublished** sheet, because it contains email addresses.

---

## 7. Which links work

Put **one link** in the `url` field. The site recognises it automatically.

| Link type | Example | How it plays | Screen off? |
|---|---|---|---|
| Direct audio file | `https://.../mix.mp3` (also `.m4a`, `.ogg`, `.wav`) | Own player | **Yes** |
| Internet Archive | `https://archive.org/details/ITEM-NAME` | Own player, with a chapter list | **Yes** |
| SoundCloud | `https://soundcloud.com/artist/track` | Embedded SoundCloud player | Usually not |
| Mixcloud | `https://www.mixcloud.com/artist/mix/` | Embedded Mixcloud player | Usually not |
| Bandcamp | The `src` link from Share, then Embed | Embedded Bandcamp player | Usually not |
| Anything else | | Shows an "Open link" button | No |

### Bandcamp

Bandcamp does not allow guessing embed links. On the Bandcamp page, click **Share / Embed**, **Embed this album**, and copy only the address inside `src="..."`. It starts with `https://bandcamp.com/EmbeddedPlayer/`. Paste that as the `url`.

### Private or unlisted links

Private SoundCloud links and unlisted Mixcloud links work. Artists can send them through the submit form.

### Removed on purpose

Spotify, YouTube and Vimeo links are not supported. A link from one of those sites shows an "Open link" button.

---

## 8. Hosting your own audio files

Hosting your own files is the only way to be sure people can listen with the screen off, and it gives you full control over what is played.

### File format

- Use **mp3**. Music: 128 to 192 kbps. Speech (poetry, book readings): 64 to 96 kbps, mono, is plenty and uses much less data.
- Name files without spaces or accents: `rain-on-tin-roofs.mp3`.

### Where to store them

| Option | Cost | Good for |
|---|---|---|
| **Cloudflare R2** | Free tier, then a few cents per GB. No charge for bandwidth. | Best value for a growing station |
| **Bunny.net Storage** | A few euros per month | Simple, fast |
| **Internet Archive** | Free | Public-domain and permission-based material, book readings in chapters |
| The repo's `audio/` folder | Free | A handful of small files only. Do not use for dozens of mixes. |

### Cloudflare R2 in short

1. In Cloudflare, open **R2**, then **Create bucket**.
2. Upload your mp3.
3. Open the bucket **Settings** and enable **Public access** (use a custom domain or the `r2.dev` address).
4. Copy the file's public link and paste it into `url`.

### Internet Archive in short

1. Create an account at archive.org and click **Upload**.
2. Upload your mp3 files. For a book, upload one file per chapter and name them so they sort correctly (`01-chapter-one.mp3`, `02-chapter-two.mp3`).
3. Choose the **Audio** media type, add title and description, and publish.
4. Copy the page address (`https://archive.org/details/...`) into `url`. The site builds the chapter list itself.

> **Do not** switch on "hotlink protection" at your host in a way that blocks your own domain. It would stop the player from loading files.

---

## 9. Playing with the screen off

What works depends on the type of link.

- **Direct files and Internet Archive** are played by the site's own audio player. Phones keep playing when the screen locks, and the lock screen shows play, pause, next and previous, and seeking.
- **SoundCloud, Mixcloud and Bandcamp** are small pages from other companies shown inside the site. Phones normally pause those when the screen locks. The site cannot change this.

**Recommendation:** host your main mixes as mp3 files. Keep SoundCloud for artists who prefer it, and tell listeners the screen-off option exists on the mixes that support it.

Installing the site on the home screen makes background audio more reliable (section 17).

---

## 10. Artists and artist pages

Artist pages are created automatically. Every artist who appears in `SHOWS` or as an `author` in `TEXTS` gets a card on the **Artists** page and their own page, listing their mixes and their texts.

To add a bio, place and links, find this block near the top of the script:

```js
const ARTISTS = {
  "Dead Tapes": {
    place: "Leeds, UK",
    bio: "Three people, one four-track, zero patience.",
    links: [
      { label: "SoundCloud", url: "https://soundcloud.com/your-account" },
      { label: "Instagram",  url: "https://instagram.com/..." }
    ]
  },
  ...
};
```

To add a new artist, copy one block and change the values. For an artist with no links use `links: []`.

- The name **must match exactly** the `artist` field in `SHOWS` (or the `author` field in `TEXTS`).
- If someone has mixes or texts but no entry here, they still get a page, just without a bio.
- Rename an artist: change it in **every** place it appears.

The twelve sample artists are invented, and so are their bios. Before launch, delete them from `ARTISTS` (or from the Artists tab of the sheet) and add your own.

---

## 11. Genres

Genre filters are built from the `genre` fields of your mixes, so a new genre appears by itself the first time you use it.

Two audio genres are always offered, even before anyone uploads in them:

```js
const CORE_GENRES = ["Spoken word", "Book reading"];
```

Poetry is not in this list on purpose: poems are texts to be read, and they live on the Read shelf, with their own filter (`CORE_KINDS`, section 5). Add more names to this list to always show them. Keep spelling and capital letters consistent: `"Hip hop"` and `"Hip-Hop"` would become two separate filters.

---

## 12. Receiving submissions and messages

The site has three forms: **Submit a mix**, **Submit a text** and **Raise your hand** (volunteers, on the Contribute page). A static site has no server, so a form service receives them and emails you.

### Set it up (10 minutes)

1. Create a free account at **formspree.io** (alternatives: Getform, Basin).
2. Create a new form. Formspree gives you an address like `https://formspree.io/f/abcdwxyz`.
3. In `index.html` find:
   ```js
   const FORM_ENDPOINT = "";
   ```
   and put the address between the quotes:
   ```js
   const FORM_ENDPOINT = "https://formspree.io/f/abcdwxyz";
   ```
4. Save, publish, and send a test submission from the live site.

Until you do this, the forms run in **draft mode**: they look like they work but nothing is sent.

### What arrives

Each email contains all the fields. A field called `type` tells you which form it came from: `mix`, `text` or `volunteer`.

### Handling submissions

1. Listen to the mix and read the description.
2. Reply to the artist, either to accept, ask for changes, or decline kindly.
3. If accepted: upload or link the audio, add the block in `SHOWS` (section 4), add the artist if new (section 10).
4. Keep the original email. It is your record that the artist confirmed they own the rights.

### Spam

If spam appears, turn on Formspree's built-in spam filter and reCAPTCHA in the form settings. Both are free options.

### Changing the form fields

The form's HTML is in the `view-submit` section, and the checks are in the function called `validate()`. To change the minimum description length from 120 characters, search for `120` and change it in the check, the hint text and the error message.

---

## 13. Donate and membership links

Two settings at the top of the script:

```js
const MEMBER_URL = "https://liberapay.com/your-page";
const DONATE_URL = "https://ko-fi.com/your-page";
```

- `DONATE_URL` is used by every **Donate** button (menu, About, Contribute, footer).
- `MEMBER_URL` is used by every **Become a member** button.

### Which service

| Service | Good for |
|---|---|
| **Ko-fi** | One-off donations, memberships, no fee on donations |
| **Liberapay** | Recurring donations, non-profit friendly, open source |
| **Open Collective** | Transparent public budget, good if you become a collective |
| **Patreon** | Memberships with tiers, takes a larger cut |
| **PayPal.me** | Quick and simple one-off link |

Create the page on the service, copy its address, paste it into the setting. All buttons update at once.

### Member perks

The perks on the Contribute page (a vote on the monthly theme, credits, a seasonal zine, early access) are **suggestions**. Edit them in the section marked `<!-- CONTRIBUTE -->`, in the list with class `perks`. Only promise what you will actually deliver, and set the same perks on your Ko-fi or Liberapay page.

---

## 14. Editing text

Almost all text lives in the `<body>` part of `index.html`. Use your editor's **Find** (Ctrl+F or Cmd+F) to jump to it.

| To change | Search for |
|---|---|
| The About page essay | `<!-- ABOUT -->` |
| The Contribute page | `<!-- CONTRIBUTE -->` |
| The house rules and form text for mixes | `<!-- SUBMIT -->` |
| The house rules and form for texts | `<!-- SUBMIT A TEXT -->` |
| The Read page intro | `<!-- READ -->` |
| The scrolling top strip | `class="strip"` |
| The tagline under the logo | `class="tagline` |
| Footer | `<footer>` |
| Contact email | `hello@your-domain.com` (appears in a few places. Replace all.) |
| Site name in the browser tab | `<title>` |

### Changing the site name

The big logo is built from letters by script. Search for:

```js
const text="FREE NOISE RADIO"
```

and change the name. Also change `<title>`, the footer, and the `aria-label` on the `<h1>` (used by screen readers). The name `Free Noise Radio` is also used for lock-screen metadata. Search for it.

### Text rules

- Keep each text block inside its tags (`<p>...</p>`).
- To add a paragraph, copy a `<p>...</p>` line.
- The symbols `&`, `<` and `>` in normal text should be written `&amp;`, `&lt;` and `&gt;`.

---

## 15. Changing the look

All colours are set once, at the top of the `<style>` part:

```css
:root {
  --paper:  #ecebe4;   /* page background */
  --ink:    #151515;   /* text and borders */
  --pink:   #ff2e88;   /* main accent */
  --yellow: #f6e800;   /* highlighter accent */
}
```

Change a value and the whole site follows. A second block below it (`prefers-color-scheme: dark` and `data-theme="dark"`) sets the dark version used on phones in dark mode.

**Fonts** are loaded from Google Fonts in the `<head>`:

- **Anton** for big headlines
- **Special Elite** for body text (typewriter)
- **Permanent Marker** for handwritten bits

To swap one, pick a font at fonts.google.com, replace the name in the `<link href=...>` line, and replace the name in the CSS (`font-family:`).

**Logo colours:** the letters of the logo use four styles (`.logo .b`, `.p`, `.y`, `.w`). The random arrangement is fixed by a number called `seed`. Change `let seed=7` to another number for a different arrangement of the same letters.

**Photocopy grain:** the background texture is the long `background-image:url("data:image/svg+xml...` line on `body`. Delete that line for a flat paper look.

> If you change colours, keep enough contrast between text and background. Black on cream and cream on black are safe. Test pink text on light backgrounds especially.

---

## 16. Add a new page

The site has one navigation and several "views". Only one view shows at a time. Adding a page takes four small steps. This example adds a **Schedule** page.

### Step 1: Add the menu button

Find the `<nav class="tabs"` block and add a button. The `data-view` value is the page's short name:

```html
<button role="tab" aria-selected="false" data-view="schedule">Schedule</button>
```

### Step 2: Add the page

Add a new section next to the others (for example before `<!-- ABOUT -->`). The `id` must be `view-` plus the short name, and it must start hidden:

```html
<!-- SCHEDULE -->
<section class="view" id="view-schedule" hidden>
  <h2 class="display"><span class="hl">What's</span> on</h2>
  <div class="sheet tape">
    <p>Mondays 20:00: new mixes. Fridays 22:00: late-night spoken word.</p>
  </div>
</section>
```

### Step 3: Register the page

Search for `function route()`. In the list of known pages, add the new name:

```js
if(["listen","read","text","artists","artist","submit","submit-text","contribute","about","schedule"].includes(v)) show(v,p);
```

### Step 4: Check

Save, refresh, click the new button. The address in the browser becomes `#schedule`, so you can link directly to that page.

### Linking to a page from anywhere

Any button or link with `data-goto="schedule"` opens the page:

```html
<button class="linkbtn" type="button" data-goto="schedule">See the schedule</button>
```

### Ready-made building blocks (copy these)

| You want | Use |
|---|---|
| A page title with a black highlight | `<h2 class="display"><span class="hl">Word</span> rest of title</h2>` |
| A boxed, taped panel | `<div class="sheet tape"> ... </div>` |
| A grid of cards | `<div class="grid"> <article class="card"> ... </article> </div>` |
| A button | `<button class="play">Text</button>` |
| A yellow external link button | `<a class="donate" href="https://..." target="_blank" rel="noopener">Text</a>` |
| A handwritten highlighted line | `<p class="shout">Text</p>` inside a `<div class="essay">` |

---

## 17. Install on a phone like an app

This makes the site installable from the browser ("Add to Home Screen"), opens it without browser bars, and keeps background audio steadier.

### Step 1: Create `manifest.json` next to `index.html`

```json
{
  "name": "Free Noise Radio",
  "short_name": "Free Noise",
  "start_url": "/",
  "display": "standalone",
  "background_color": "#ecebe4",
  "theme_color": "#151515",
  "icons": [
    { "src": "icon-192.png", "sizes": "192x192", "type": "image/png" },
    { "src": "icon-512.png", "sizes": "512x512", "type": "image/png" }
  ]
}
```

### Step 2: Make two icons

Create square PNG pictures called `icon-192.png` (192 by 192 pixels) and `icon-512.png` (512 by 512). A black square with a pink radio emoji or your logo letters is enough. Put them next to `index.html`.

### Step 3: Add a tiny service worker, `sw.js`

```js
self.addEventListener("install", () => self.skipWaiting());
self.addEventListener("activate", e => e.waitUntil(self.clients.claim()));
self.addEventListener("fetch", () => {});
```

### Step 4: Link everything in `index.html`

Inside `<head>`:

```html
<link rel="manifest" href="manifest.json">
<meta name="theme-color" content="#151515">
<link rel="apple-touch-icon" href="icon-192.png">
```

At the end of the `<script>` part:

```js
if ("serviceWorker" in navigator) navigator.serviceWorker.register("sw.js");
```

### Step 5: Install

- **iPhone (Safari):** Share button, then **Add to Home Screen**.
- **Android (Chrome):** menu, then **Install app** or **Add to Home screen**.

The site needs to be on HTTPS (all hosts in section 3 do this).

---

## 18. Maintenance routine

### Each time you add a mix (5 minutes)
- [ ] Audio is uploaded and the link opens in a private browser window.
- [ ] Block added to `SHOWS`, `demo: false`. (For a text: block added to `TEXTS`, opened on a phone, no backticks inside the body.)
- [ ] Artist spelled exactly as before. New artist added to `ARTISTS`.
- [ ] Saved, waited a minute, opened the live site, pressed play on your phone.

### Weekly
- [ ] Read the submission inbox. Reply to everyone, even with a "not this time".
- [ ] Play the newest mix on a phone, once with the screen locked.

### Monthly
- [ ] Click through every mix. Replace or remove any that no longer play.
- [ ] Check your audio storage space and costs.
- [ ] Check that the donate and membership links open.
- [ ] Look at the form service's spam folder for real messages that got caught.

### Yearly
- [ ] Domain renews (auto-renew on, card not expired).
- [ ] Re-read the About, Contribute and house rules text. Does it still say what you mean?
- [ ] Review the rights policy (section 19).

### Backups
- **GitHub keeps the full history** of every change, so the code and data are backed up.
- **Audio files are not in GitHub** (unless you put them there). Keep a copy of your audio folder on a second drive or cloud storage. Artists should also keep their own originals.
- **Emails** from the form service are your record of submissions. Do not delete them.

### Updating the site safely
1. Edit.
2. Check on your computer (section 2).
3. Save to GitHub.
4. Check the live site.

If you want to experiment, create a **branch** on GitHub. Cloudflare Pages and Netlify give every branch its own test address, so you can look before it goes live.

---

## 19. Rights, privacy and takedowns

*This is practical guidance, not legal advice. Laws differ by country. If you grow, talk to a lawyer or a local arts organisation.*

### Rights
- Artists confirm in the form that they own the music or have permission to share it. **Keep those emails.**
- Playing mixes made of other people's recordings (DJ sets, covers, samples) can raise licensing questions, especially if you host the audio yourself. Embedded players from SoundCloud and Mixcloud handle licensing through their own platforms, which is one reason to use them for DJ mixes.
- **Book readings:** only share texts that are public domain, used with permission, or written by the reader. Internet Archive and LibriVox are good sources for public-domain texts.
- The About page has a **Rights and takedowns** section that says this in plain words. Edit it under `<!-- ABOUT -->` if your policy differs.

### Takedowns
Publish a contact address (the site has one on the About page). If someone asks you to remove their work, remove it quickly: delete the block in `SHOWS`, delete the audio file, and reply to confirm.

### Privacy
You collect names and emails through the forms. Use them only to reply. Do not add them to a mailing list without asking. If you serve readers in Europe, add a short privacy note saying what you collect, why, and how to ask you to delete it. The site itself has no analytics and no tracking cookies.

---

## 20. Troubleshooting

| Problem | Likely cause | Fix |
|---|---|---|
| Page is blank or no mixes show after an edit to `index.html` | Broken punctuation in the data | On GitHub open the file's **History**, find the last good version, and restore it. Or look for a missing comma or quote near your last change. |
| Press play: "Demo mode" message | `demo: true` | Set `demo: false` and add a real `url`. |
| Press play: "Open link" button appears | The link type is not supported | Use a direct mp3, Internet Archive, SoundCloud, Mixcloud or Bandcamp link. |
| Player says it could not load the file | Wrong link, file is private, or host blocks other sites | Open the link directly in a browser. It should start playing. Check public access and hotlink settings. |
| Music stops when the screen locks | The mix uses SoundCloud, Mixcloud or Bandcamp | Use a direct mp3 or Internet Archive link (sections 7 to 9). |
| Artist page has no bio | Name spelled differently in `ARTISTS` | Make both spellings identical. |
| A mix shows under the wrong artist or splits into two artists | Typo in the `artist` field | Fix the spelling in `SHOWS`. |
| Form says "Draft mode: nothing was sent" | `FORM_ENDPOINT` is empty | Section 10. |
| Form says "Something broke" | Wrong endpoint, or the form service is blocking the site's domain | Check the endpoint address. In Formspree, make sure your domain is allowed. |
| Genre filter shows two near-identical genres | Different spelling or capitals | Make every mix use the same spelling. |
| Sheet change does not show on the site | Google refreshes published sheets every few minutes, or the tab is not published, or the row has `published` set to `FALSE` | Wait five minutes and hard-refresh. Check the row, and check **File, Share, Publish to web** shows the tab as published. |
| Banner: "Could not reach the sheet" | Wrong link in `SHEET`, tab unpublished, or Google unreachable | Open each link in a browser. It should download or show comma-separated text. Re-publish the tab and paste the new link. |
| A row is missing from the site | No title, artist or author, or `published` is `FALSE`, or the date is unreadable | Fill the missing cell, and use `YYYY-MM-DD` dates. |
| Site did not update after saving | Deploy still running, or cached page | Wait two minutes, then hard-refresh (Ctrl+Shift+R, or Cmd+Shift+R). Check the **Deployments** tab at your host. |
| Date filter hides a mix | `date` is not in `YYYY-MM-DD` form | Fix the date, keeping the quotes. |
| Fonts look different | Google Fonts blocked or offline | The site falls back to system fonts. It is still usable. |

---

## 21. Growing later

The first version keeps all data in `index.html` because it is the simplest thing that works. When editing the file starts to feel annoying, here is the path:

1. **Move the built-in sample data to a separate file** (only if you do not use the sheet). Put `SHOWS`, `TEXTS` and `ARTISTS` into `data.js` and load it before the main script.
2. **Edit data from a spreadsheet.** Already built in: see section 6.
3. **Use a CMS.** Tools like Decap CMS, Sanity or Airtable give you a form-based editor, and the site reads from them.
4. **Let artists upload directly.** Needs a small backend (for example Cloudflare Workers with R2, or Supabase). Worth it once you receive many submissions.
5. **Real live radio.** This site plays recordings on demand. A true 24/7 stream with a schedule needs a streaming server such as AzuraCast, which is free and open source, running on a small rented server. The website can then embed that stream.
6. **More pages:** a schedule, a news or zine page, shop for printed zines, an RSS or podcast feed so mixes appear in podcast apps.

Each step is optional. If you run from the Google Sheet, step 1 does not matter. A site with 50 mixes in one file is perfectly fine.

---

*Make it. Explain it. Share it.*
