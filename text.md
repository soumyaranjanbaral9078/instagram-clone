# Instagram-Style Clone — Project Guide

## 1. What this project is

This project is a responsive, front-end-only Instagram-style social photo feed. The page is implemented in one file: [`index.html`](./index.html). It uses standard HTML, CSS, and browser JavaScript; there is no framework, package manager, build step, or server-side code.

The interface is a learning/demo project and is not affiliated with Instagram or Meta. It does not log in to Instagram or connect to Instagram's services.

## 2. Run the project

1. Keep `index.html` and this guide in the same project folder.
2. Open `index.html` in a modern browser, or open the project folder in an editor and use its HTML preview/live-server feature.
3. An internet connection is needed for the example photos, which are loaded from Unsplash. The page itself is local and does not need a network connection to open.

No `npm install` or other dependency installation is required.

## 3. Page features

| Area | What it contains |
| --- | --- |
| Desktop navigation | Fixed sidebar with home, search, explore, reels, messages, notifications, create, profile, and more controls. |
| Stories | Horizontally scrollable row of example profiles with gradient story rings. |
| Feed | Three example photo posts, each with an account, location, photo, caption, likes, comments, and action buttons. |
| Suggestions | Signed-in demo account, suggested profiles, follow buttons, and footer links. |
| Mobile layout | Compact top bar, feed-first layout, and fixed bottom navigation on narrow screens. |
| Create dialog | Opens a dialog where the user can choose an image file. It does not upload or publish the image. |

The navigation/sidebar collapses at tablet widths. At mobile widths, the suggestions and desktop sidebar are hidden and mobile navigation is shown.

## 4. Demo interactions

These actions work locally in the browser:

- **Like:** Click the heart to toggle a like and increment or decrement the displayed count. Double-clicking a post photo likes it and plays a small animation.
- **Comment:** Enter text and choose **Post** to add a comment to that post. Comments are created as text nodes, not interpreted as HTML.
- **View comments:** Toggle the comments area and focus the comment field.
- **Save:** Toggle a post's saved appearance and show a confirmation message.
- **Share:** Try to copy the current page URL to the clipboard. Clipboard access depends on browser permissions and page context.
- **Follow:** Toggle a suggested account between **Follow** and **Following**.
- **Story and navigation buttons:** Show demo notifications; they do not navigate to real pages or open real stories.
- **Create:** Open the image picker. Selecting a file only shows its name; the page explicitly does not publish or upload it.
- **Dismiss the create dialog:** Use its close button, click outside the dialog, or press Escape.

The temporary like, comment, follow, and save changes live only in the current page session. Reloading the page resets them.

## 5. Code layout inside `index.html`

The document is organized in this order:

1. **`<head>`:** Page title, viewport settings, description, and the page's CSS.
2. **CSS `<style>` block:** Theme variables, shared controls, sidebar, feed, cards, modal, notifications, and responsive breakpoints.
3. **`<body>` app markup:** Sidebar navigation, stories, posts, suggestions, and mobile bottom navigation.
4. **Create dialog and notification element:** Shared UI elements used by the JavaScript interactions.
5. **`<script>` block:** Event handlers for likes, comments, saves, sharing, follows, navigation, and the create dialog.

The JavaScript is wrapped in an immediately invoked function, so its helper functions and temporary variables do not become global variables.

## 6. Customize the page

### Change the theme

At the top of the CSS, edit the custom properties in `:root`:

```css
:root {
  --text: #262626;
  --muted: #737373;
  --line: #dbdbdb;
  --blue: #0095f6;
  --page: #fafafa;
  --sidebar-width: 245px;
}
```

These values control the main text, muted text, borders, accent color, page background, and desktop sidebar width.

### Change example photos

Search `index.html` for `images.unsplash.com`. Replace each `src` URL with an image URL you are allowed to use. Update each image's `alt` text so it accurately describes the replacement. The images are remote; for a fully offline version, save permitted image files in the project and use relative paths, for example `images/profile.jpg`.

### Change account names, captions, and locations

The sample accounts and post content are ordinary HTML in the stories, post headers, captions, and suggestion list. Edit the visible text there. Each post's starting like count is stored in the matching likes element's `data-likes` attribute as a number; update both that number and its displayed text when replacing the sample content.

### Add another feed post

Copy one complete `<article class="post" ...>` element and insert it inside `<section class="feed">`. When copying:

- Give it a unique `data-post-id`.
- Give its comment input a unique `id` and update the associated `<label for="...">`.
- Replace the username, location, photo, alternative text, caption, and initial like count.
- Keep the existing button class names (`like-button`, `comment-focus`, `share-button`, `save-button`, `more-button`) and comment form structure. The script attaches behavior to these classes when the page loads.

### Add or remove a story

Copy or remove a `<button class="story">` in the stories container. Set its `data-user`, profile image, alternative text, and visible story name.

## 7. Current limitations

This is a static UI demo, not a complete social network:

- There is no registration, login, authentication, or account switching.
- Posts, comments, likes, follows, and saved state are sample/local browser state only; no data is persisted.
- The image picker does not upload, process, or publish an image.
- Search, explore, reels, messages, and notifications are visual demo controls and do not implement those features.
- There is no database, API, moderation, privacy control, or real-time update system.
- The sample Unsplash photos require an internet connection and may be subject to their provider's availability and terms.

## 8. What a real full-stack version would need

To turn this prototype into a multi-user application, add and test these capabilities on a server:

1. **Authentication and authorization:** Secure sign-up, login, logout, session management, and access checks.
2. **Data storage:** Persistent models for users, posts, comments, likes, follows, and saved posts, with timestamps and relationships.
3. **Post and image API:** Validate uploads, enforce file size/type limits, store images in object storage, and return safe image URLs.
4. **Feed and profile APIs:** Fetch paginated posts and profile information for the signed-in user.
5. **Interaction APIs:** Create/delete comments, likes, follows, and saved-post records; validate permissions and prevent duplicate actions.
6. **Production safeguards:** Input validation, output encoding, rate limits, abuse reporting/moderation, secure secrets, logging, and automated tests.

Do not put database credentials, private API keys, or other secrets in `index.html`. Browser code is visible to every visitor; sensitive operations and credentials belong on the server.