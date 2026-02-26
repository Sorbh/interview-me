---
name: spec-preview-style
description: HTML template and styling reference for interview-me spec preview generation. This is NOT a skill — it is a resource file read by the interview-me skill during HTML preview generation.
---

# Spec Preview: HTML Template & Style Reference

Claude reads this file when generating an interactive HTML spec preview. Follow the template structure exactly, replacing `{{PLACEHOLDER}}` markers with interview content.

## Design Tokens

```css
:root {
  /* Background */
  --bg-primary: #0a0f1c;
  --bg-section: #111827;
  --bg-card: #1a2236;
  --bg-input: #0d1424;

  /* Text */
  --text-primary: #f1f5f9;
  --text-secondary: #94a3b8;
  --text-muted: #64748b;

  /* Accent */
  --accent: #00d4aa;
  --accent-hover: #00f0c0;
  --accent-muted: rgba(0, 212, 170, 0.15);

  /* Status */
  --status-approved: #22c55e;
  --status-approved-bg: rgba(34, 197, 94, 0.1);
  --status-changes: #eab308;
  --status-changes-bg: rgba(234, 179, 8, 0.1);
  --status-pending: #64748b;

  /* Layout */
  --section-max-width: 900px;
  --border-radius: 12px;
  --transition: 0.3s ease;

  /* Typography */
  --font-body: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif;
  --font-mono: 'JetBrains Mono', 'Fira Code', 'Cascadia Code', monospace;
}
```

## Complete HTML Template

Claude MUST use this exact structure. Replace `{{PLACEHOLDER}}` markers with generated content.

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="Cache-Control" content="no-cache, no-store, must-revalidate">
  <title>{{SPEC_TITLE}} — Spec Preview</title>
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&family=JetBrains+Mono:wght@400;500&display=swap" rel="stylesheet">
  <style>
    /* ============ RESET ============ */
    *, *::before, *::after { margin: 0; padding: 0; box-sizing: border-box; }
    html { scroll-behavior: smooth; }

    /* ============ DESIGN TOKENS ============ */
    /* Paste the :root block from Design Tokens above */

    /* ============ BASE ============ */
    body {
      background: var(--bg-primary);
      color: var(--text-primary);
      font-family: var(--font-body);
      font-size: 1rem;
      line-height: 1.7;
      min-height: 100vh;
      padding: 2rem 1rem 6rem;
    }

    /* ============ PROGRESS BAR ============ */
    .progress-bar {
      position: fixed;
      top: 0;
      left: 0;
      right: 0;
      display: flex;
      align-items: center;
      justify-content: center;
      gap: 8px;
      padding: 14px 16px;
      background: rgba(10, 15, 28, 0.95);
      backdrop-filter: blur(12px);
      z-index: 100;
      border-bottom: 1px solid rgba(255, 255, 255, 0.06);
    }
    .progress-dot {
      width: 10px;
      height: 10px;
      border-radius: 50%;
      background: var(--status-pending);
      cursor: pointer;
      transition: all var(--transition);
      border: none;
      padding: 0;
    }
    .progress-dot:hover { transform: scale(1.3); background: var(--text-secondary); }
    .progress-dot.active { background: var(--accent); transform: scale(1.4); }
    .progress-dot.has-comments { background: var(--status-changes); }

    /* ============ SECTIONS ============ */
    .section {
      max-width: var(--section-max-width);
      margin: 2.5rem auto;
      padding: 2.5rem;
      background: var(--bg-section);
      border-radius: var(--border-radius);
      border: 1px solid rgba(255, 255, 255, 0.06);
      opacity: 0;
      transform: translateY(20px);
      transition: opacity 0.5s ease, transform 0.5s ease;
    }
    .section.visible { opacity: 1; transform: translateY(0); }
    .section:first-child { margin-top: 5rem; }
    .section h2 {
      font-size: 1.6rem;
      font-weight: 700;
      margin-bottom: 1.25rem;
      color: var(--accent);
      letter-spacing: -0.01em;
    }
    .section h3 {
      font-size: 1.15rem;
      font-weight: 600;
      margin: 1.5rem 0 0.75rem;
      color: var(--text-primary);
    }

    /* ============ COMMENTABLE CONTENT ============ */
    .commentable {
      position: relative;
      padding-left: 12px;
      border-left: 3px solid transparent;
      transition: border-color var(--transition), background var(--transition);
      cursor: pointer;
      border-radius: 0 4px 4px 0;
    }
    .commentable:hover {
      border-left-color: var(--accent-muted);
      background: rgba(0, 212, 170, 0.03);
    }
    .commentable.has-comment {
      border-left-color: var(--status-changes);
      background: var(--status-changes-bg);
    }
    .commentable.has-comment::after {
      content: '\1F4AC';
      position: absolute;
      top: 2px;
      right: 8px;
      font-size: 0.75rem;
      opacity: 0.7;
    }

    /* ============ COMMENT BOX ============ */
    .comment-box {
      margin: 8px 0 12px 12px;
      padding: 12px;
      background: var(--bg-input);
      border: 1px solid rgba(234, 179, 8, 0.3);
      border-radius: 8px;
      display: none;
    }
    .comment-box.open { display: block; }
    .comment-box textarea {
      width: 100%;
      min-height: 60px;
      padding: 8px;
      background: var(--bg-card);
      border: 1px solid rgba(255, 255, 255, 0.1);
      border-radius: 6px;
      color: var(--text-primary);
      font-family: var(--font-body);
      font-size: 0.875rem;
      line-height: 1.5;
      resize: vertical;
    }
    .comment-box textarea:focus { outline: none; border-color: var(--status-changes); }
    .comment-box-actions {
      display: flex;
      gap: 8px;
      margin-top: 8px;
      justify-content: flex-end;
    }
    .comment-box-actions button {
      padding: 4px 14px;
      border-radius: 6px;
      border: none;
      font-size: 0.8rem;
      font-weight: 500;
      cursor: pointer;
      transition: all var(--transition);
    }
    .btn-save-comment {
      background: var(--status-changes);
      color: var(--bg-primary);
    }
    .btn-save-comment:hover { filter: brightness(1.1); }
    .btn-remove-comment {
      background: transparent;
      color: var(--text-muted);
      border: 1px solid rgba(255, 255, 255, 0.1) !important;
    }
    .btn-remove-comment:hover { color: var(--text-secondary); }

    /* ============ CONTENT TYPOGRAPHY ============ */
    .section-content p { margin: 0.75rem 0; color: var(--text-secondary); }
    .section-content ul, .section-content ol { padding-left: 1.5rem; margin: 0.75rem 0; }
    .section-content li { margin: 0.4rem 0; color: var(--text-secondary); }
    .section-content code {
      background: var(--bg-card);
      padding: 2px 6px;
      border-radius: 4px;
      font-family: var(--font-mono);
      font-size: 0.88rem;
      color: var(--accent);
    }
    .section-content pre {
      background: var(--bg-input);
      padding: 1rem 1.25rem;
      border-radius: 8px;
      overflow-x: auto;
      margin: 1rem 0;
      font-family: var(--font-mono);
      font-size: 0.85rem;
      line-height: 1.6;
      color: var(--text-secondary);
      border: 1px solid rgba(255, 255, 255, 0.04);
    }
    .section-content blockquote {
      border-left: 3px solid var(--accent);
      padding: 0.5rem 1rem;
      margin: 1rem 0;
      background: var(--accent-muted);
      border-radius: 0 8px 8px 0;
      color: var(--text-secondary);
    }

    /* ============ TABLES ============ */
    .section-content table { width: 100%; border-collapse: collapse; margin: 1rem 0; }
    .section-content th, .section-content td {
      padding: 0.7rem 1rem;
      text-align: left;
      border-bottom: 1px solid rgba(255, 255, 255, 0.06);
    }
    .section-content th {
      color: var(--accent);
      font-weight: 600;
      font-size: 0.8rem;
      text-transform: uppercase;
      letter-spacing: 0.05em;
    }
    .section-content td { color: var(--text-secondary); }
    .section-content tr:hover td { background: rgba(255, 255, 255, 0.02); }

    /* ============ COLLAPSIBLE (Decisions Log) ============ */
    .collapsible-toggle {
      display: flex;
      align-items: center;
      gap: 8px;
      cursor: pointer;
      color: var(--accent);
      font-weight: 600;
      font-size: 1.15rem;
      margin: 1.5rem 0 0.75rem;
      background: none;
      border: none;
      font-family: var(--font-body);
    }
    .collapsible-toggle:hover { color: var(--accent-hover); }
    .collapsible-toggle .arrow {
      transition: transform var(--transition);
      font-size: 0.75rem;
    }
    .collapsible-toggle.expanded .arrow { transform: rotate(90deg); }
    .collapsible-content {
      max-height: 0;
      overflow: hidden;
      transition: max-height 0.4s ease;
    }
    .collapsible-content.expanded { max-height: 2000px; }

    /* ============ ACTION BAR ============ */
    .action-bar {
      position: fixed;
      bottom: 0;
      left: 0;
      right: 0;
      display: flex;
      justify-content: center;
      align-items: center;
      gap: 16px;
      padding: 16px 20px;
      background: rgba(10, 15, 28, 0.95);
      backdrop-filter: blur(12px);
      border-top: 1px solid rgba(255, 255, 255, 0.06);
      z-index: 100;
    }
    .comment-count {
      font-size: 0.85rem;
      color: var(--text-muted);
      margin-right: 8px;
    }
    .comment-count strong { color: var(--status-changes); }
    .btn {
      padding: 10px 28px;
      border: none;
      border-radius: 8px;
      font-size: 0.95rem;
      font-weight: 600;
      cursor: pointer;
      transition: all var(--transition);
      font-family: var(--font-body);
    }
    .btn:hover { transform: translateY(-1px); }
    .btn-revise {
      background: var(--status-changes);
      color: var(--bg-primary);
    }
    .btn-revise:hover { filter: brightness(1.1); }
    .btn-revise:disabled { opacity: 0.4; cursor: not-allowed; transform: none; }
    .btn-approve {
      background: transparent;
      border: 2px solid var(--status-approved);
      color: var(--status-approved);
    }
    .btn-approve:hover { background: var(--status-approved-bg); }

    /* ============ TOAST ============ */
    .toast {
      position: fixed;
      bottom: 80px;
      left: 50%;
      transform: translateX(-50%) translateY(20px);
      background: var(--bg-card);
      border: 1px solid rgba(255, 255, 255, 0.1);
      color: var(--accent);
      padding: 12px 24px;
      border-radius: 8px;
      font-weight: 500;
      font-size: 0.9rem;
      opacity: 0;
      transition: all 0.3s ease;
      pointer-events: none;
      z-index: 200;
    }
    .toast.show { opacity: 1; transform: translateX(-50%) translateY(0); }

    /* ============ ANIMATIONS ============ */
    .section:nth-child(1) { transition-delay: 0s; }
    .section:nth-child(2) { transition-delay: 0.08s; }
    .section:nth-child(3) { transition-delay: 0.16s; }
    .section:nth-child(4) { transition-delay: 0.24s; }
    .section:nth-child(5) { transition-delay: 0.32s; }
    .section:nth-child(6) { transition-delay: 0.4s; }
    .section:nth-child(7) { transition-delay: 0.48s; }
    .section:nth-child(8) { transition-delay: 0.56s; }

    /* ============ REDUCED MOTION ============ */
    @media (prefers-reduced-motion: reduce) {
      *, *::before, *::after {
        animation-duration: 0.01ms !important;
        transition-duration: 0.1s !important;
        scroll-behavior: auto !important;
      }
      .section { opacity: 1; transform: none; }
    }

    /* ============ RESPONSIVE ============ */
    @media (max-width: 768px) {
      body { padding: 1rem 0.5rem 5rem; }
      .section { padding: 1.5rem; margin: 1.5rem auto; }
      .section h2 { font-size: 1.3rem; }
      .action-bar { flex-wrap: wrap; padding: 12px 16px; }
      .btn { padding: 8px 20px; font-size: 0.88rem; }
    }
  </style>
</head>
<body>

  <!-- Progress Bar -->
  <nav class="progress-bar" role="navigation" aria-label="Section navigation">
    {{PROGRESS_DOTS}}
    <!-- Generate one per section:
    <button class="progress-dot" data-index="0" title="Section Title" aria-label="Go to Section Title"></button>
    -->
  </nav>

  <!-- Sections -->
  <main>
    {{SECTIONS}}
    <!-- Generate one per coverage area. Structure for each section:

    <section class="section" id="section-0" data-index="0">
      <h2>Section Title</h2>
      <div class="section-content">
        <p class="commentable" data-id="0-0">Paragraph text here...</p>
        <ul>
          <li class="commentable" data-id="0-1">Bullet point text...</li>
          <li class="commentable" data-id="0-2">Another bullet...</li>
        </ul>
        <pre class="commentable" data-id="0-3"><code>code block here</code></pre>
        <table>
          <thead><tr><th>Col 1</th><th>Col 2</th></tr></thead>
          <tbody>
            <tr class="commentable" data-id="0-4"><td>Cell</td><td>Cell</td></tr>
          </tbody>
        </table>
      </div>
    </section>

    For the Decisions Log section, use collapsible:

    <section class="section" id="section-N" data-index="N">
      <h2>Decisions Log</h2>
      <div class="section-content">
        <button class="collapsible-toggle" onclick="toggleCollapse(this)">
          <span class="arrow">&#9654;</span> Show Decisions (X entries)
        </button>
        <div class="collapsible-content">
          <table>...</table>
        </div>
      </div>
    </section>
    -->
  </main>

  <!-- Action Bar -->
  <footer class="action-bar">
    <span class="comment-count"><strong id="comment-count">0</strong> comments</span>
    <button class="btn btn-revise" id="btn-revise" onclick="revise()" disabled>Revise</button>
    <button class="btn btn-approve" onclick="approve()">Approved</button>
  </footer>

  <!-- Toast -->
  <div class="toast" id="toast"></div>

  <script>
    // ============ STATE ============
    const comments = new Map(); // key: data-id, value: { text, snippet }
    const sections = document.querySelectorAll('.section');
    const dots = document.querySelectorAll('.progress-dot');
    let currentSection = 0;

    // ============ INTERSECTION OBSERVER ============
    const observer = new IntersectionObserver((entries) => {
      entries.forEach(entry => {
        // Fade-in animation
        if (entry.isIntersecting) {
          entry.target.classList.add('visible');
        }
        // Track current section for progress dots
        if (entry.isIntersecting && entry.intersectionRatio > 0.3) {
          const idx = [...sections].indexOf(entry.target);
          if (idx !== -1) {
            currentSection = idx;
            updateDots();
          }
        }
      });
    }, { threshold: [0, 0.3] });
    sections.forEach(s => observer.observe(s));

    function updateDots() {
      dots.forEach((dot, i) => {
        dot.classList.toggle('active', i === currentSection);
      });
    }

    // ============ NAVIGATION ============
    dots.forEach((dot, i) => {
      dot.addEventListener('click', () => {
        sections[i].scrollIntoView({ behavior: 'smooth', block: 'start' });
      });
    });

    document.addEventListener('keydown', (e) => {
      if (e.target.tagName === 'TEXTAREA') return;
      if (e.key === 'ArrowDown' || e.key === 'ArrowRight') {
        e.preventDefault();
        if (currentSection < sections.length - 1) {
          sections[currentSection + 1].scrollIntoView({ behavior: 'smooth', block: 'start' });
        }
      }
      if (e.key === 'ArrowUp' || e.key === 'ArrowLeft') {
        e.preventDefault();
        if (currentSection > 0) {
          sections[currentSection - 1].scrollIntoView({ behavior: 'smooth', block: 'start' });
        }
      }
    });

    // ============ INLINE COMMENTING ============
    document.querySelectorAll('.commentable').forEach(el => {
      el.addEventListener('click', (e) => {
        e.stopPropagation();
        const id = el.dataset.id;
        let box = el.nextElementSibling;
        if (!box || !box.classList.contains('comment-box')) {
          box = document.createElement('div');
          box.className = 'comment-box';
          box.innerHTML = `
            <textarea placeholder="What should change here?">${comments.has(id) ? comments.get(id).text : ''}</textarea>
            <div class="comment-box-actions">
              <button class="btn-remove-comment" onclick="removeComment('${id}', this)">Remove</button>
              <button class="btn-save-comment" onclick="saveComment('${id}', this)">Save</button>
            </div>
          `;
          el.after(box);
        }
        box.classList.toggle('open');
        if (box.classList.contains('open')) {
          box.querySelector('textarea').focus();
        }
      });
    });

    function saveComment(id, btnEl) {
      const box = btnEl.closest('.comment-box');
      const textarea = box.querySelector('textarea');
      const text = textarea.value.trim();
      if (!text) return removeComment(id, btnEl);

      const el = document.querySelector(`[data-id="${id}"]`);
      const snippet = el.textContent.substring(0, 80).trim();
      comments.set(id, { text, snippet });
      el.classList.add('has-comment');
      box.classList.remove('open');
      updateCommentCount();
      updateDotComments();
    }

    function removeComment(id, btnEl) {
      const box = btnEl.closest('.comment-box');
      const el = document.querySelector(`[data-id="${id}"]`);
      comments.delete(id);
      el.classList.remove('has-comment');
      box.classList.remove('open');
      box.querySelector('textarea').value = '';
      updateCommentCount();
      updateDotComments();
    }

    function updateCommentCount() {
      const count = comments.size;
      document.getElementById('comment-count').textContent = count;
      document.getElementById('btn-revise').disabled = count === 0;
    }

    function updateDotComments() {
      dots.forEach((dot, i) => {
        const section = sections[i];
        const sectionId = section.dataset.index;
        const hasComments = [...comments.keys()].some(k => k.startsWith(sectionId + '-'));
        dot.classList.toggle('has-comments', hasComments);
      });
    }

    // ============ COLLAPSIBLE ============
    function toggleCollapse(btn) {
      btn.classList.toggle('expanded');
      const content = btn.nextElementSibling;
      content.classList.toggle('expanded');
    }

    // ============ FEEDBACK COLLECTION ============
    function collectFeedback() {
      const sectionMap = {};
      comments.forEach((val, key) => {
        const sectionIdx = key.split('-')[0];
        const section = document.getElementById('section-' + sectionIdx);
        const title = section ? section.querySelector('h2').textContent : 'Unknown';
        if (!sectionMap[title]) sectionMap[title] = [];
        sectionMap[title].push(val);
      });

      let feedback = '## Spec Feedback\n\n';
      document.querySelectorAll('.section h2').forEach(h2 => {
        const title = h2.textContent;
        if (sectionMap[title]) {
          feedback += `### Section: ${title}\n`;
          sectionMap[title].forEach(c => {
            feedback += `- "${c.text}" (on: "${c.snippet}...")\n`;
          });
          feedback += '\n';
        }
      });
      return feedback;
    }

    // ============ ACTIONS ============
    function revise() {
      const feedback = collectFeedback();
      copyToClipboard(feedback, 'Comments copied! Paste in Claude Code to revise.');
    }

    function approve() {
      copyToClipboard(
        'All sections approved — generate final spec.',
        '"Approved" copied! Paste in Claude Code.'
      );
    }

    function copyToClipboard(text, message) {
      if (navigator.clipboard && navigator.clipboard.writeText) {
        navigator.clipboard.writeText(text).then(() => showToast(message)).catch(() => fallbackCopy(text, message));
      } else {
        fallbackCopy(text, message);
      }
    }

    function fallbackCopy(text, message) {
      const ta = document.createElement('textarea');
      ta.value = text;
      ta.style.cssText = 'position:fixed;left:-9999px';
      document.body.appendChild(ta);
      ta.select();
      try { document.execCommand('copy'); showToast(message); }
      catch { showToast('Copy failed — please select and copy manually.'); }
      document.body.removeChild(ta);
    }

    function showToast(msg) {
      const t = document.getElementById('toast');
      t.textContent = msg;
      t.classList.add('show');
      setTimeout(() => t.classList.remove('show'), 3000);
    }

    // ============ INIT ============
    updateDots();
    updateCommentCount();
  </script>
</body>
</html>
```

## Section Content Generation Rules

Claude maps interview content to sections as follows:

1. **Title / Overview** — First section. Generated from input requirement + pre-analysis summary. Include `data-index="0"`. Comment box targets on paragraphs.
2. **Goals & Non-Goals** — From interview Q&A about scope. Goals as `<ul>`, Non-Goals as separate `<ul>` under `<h3>`.
3. **One section per coverage area** — Technical Design, API Design, Data Model, Error Handling, etc. Each gets its own section card. Content from Q&A pairs synthesized into prose with `<p>`, `<ul>`, `<code>`, `<pre>` as appropriate.
4. **Decisions Log** — Use the collapsible pattern. Render as an HTML `<table>` with columns: #, Topic, Decision, Rationale. Collapsed by default.
5. **Implementation Order** — Styled `<ol>` with nested structure showing dependency chain.

### data-id Convention

Every `.commentable` element gets a `data-id` of `{sectionIndex}-{elementIndex}` (e.g., `"2-0"`, `"2-1"`, `"3-0"`). This lets the JS map comments back to their section for the feedback format.

## Progress Dot Template

Generate one `<button>` per section inside `.progress-bar`:

```html
<button class="progress-dot" data-index="{{INDEX}}" title="{{SECTION_TITLE}}" aria-label="Go to {{SECTION_TITLE}}"></button>
```
