---
title: src
author: chz-cn
license: CC BY 4.0
create_at: 2026-09-01

_layout: landing
---

<!-- markdownlint-disable MD033-->
<style>
  [data-bs-theme="light"] {
    --card-bg: #F2F6FA;
    --card-border: #d0d7de;
    --card-border-hover: #58a6ff;
    --card-text: #24292e;
    --card-meta: #57606a;
    --card-link: #0969da;

    --input-valid: #93dde7;
    --input-invalid: #ff7a7a;
    --input-form: #E8F0FE;
  }

  [data-bs-theme="dark"] {
    --card-bg: #2d333b;
    --card-border: #444c56;
    --card-border-hover: #0969da;
    --card-text: #cdd9e5;
    --card-meta: #8b949e;
    --card-link: #58a6ff;

    --input-valid: #6c4492;
    --input-invalid: #ad0000;
    --input-form: #0C0C0C;
  }

  #project-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(320px, 1fr));
    gap: 1.25rem;
    margin: 1.25rem 0;
  }

  #project-grid>article {
    background: var(--card-bg);
    border: 1px solid var(--card-border);
    border-radius: 0.75rem;
    padding: 1.125rem 1.25rem 1rem;
    transition: transform 0.35s, border-color 0.35s;
    color: var(--card-text);

    &:hover {
      transform: translateY(-0.25rem);
      border-color: var(--card-border-hover);
    }

    &>a {
      margin-bottom: 0.25rem;
      font-size: 1.125rem;
      font-weight: 600;

      color: var(--card-link);
      text-decoration: none;
    }

    &>p {
      margin: 0.375rem 0 0.75rem;
      font-size: 0.875rem;
      color: var(--card-meta);
      line-height: 1.5;
    }

    &>div {
      font-size: 0.875rem;
      color: var(--card-meta);
      border-top: 2px solid var(--card-border);
      padding-top: 0.75rem;
      display: flex;
      flex-wrap: wrap;
      gap: 0.375rem 1rem;

      &>time::before {
        content: "📅 " attr(datetime);
      }

      &>span::before {
        content: "📄 " attr(data-c) " files";
      }
    }
  }

  [debug] #project-grid>article:not(:has(
    > a:first-child[href] + p + div:last-child
    > time:first-child[datetime]:empty + span:last-child[data-c]:empty)) {
    opacity: 0.5;
    filter: grayscale(1);
  }

  #uuid-form {
    display: flex;
    gap: 0.5rem;
    align-items: center;
    margin: 0 auto 1.5rem;
    width: fit-content;
  }

  #input {
    width: 40ch;
    padding: 0.5rem 0.75rem;
    border: 1px solid var(--card-border);
    border-radius: 0.5rem;
    background: var(--input-form);
    color: var(--card-text);
    font-size: 0.825rem;
    outline: none;
    transition: border-color 0.35s;

    &:focus {
      border-color: var(--input-valid);
    }

    &:not(:placeholder-shown):invalid {
      border-color: var(--input-invalid);
    }
  }
</style>

<form id="uuid-form">
  <input id="input" type="text" required maxlength="36" minlength="36"
    pattern="[0-9a-f]{8}-[0-9a-f]{4}-4[0-9a-f]{3}-[89ab][0-9a-f]{3}-[0-9a-f]{12}"
    placeholder="xxxxxxxx-xxxx-4xxx-89ab-xxxxxxxxxxxx">
</form>

<section id="project-grid">

  <article>
    <a href="/00">00</a>
    <p>the pice 00</p>
    <div>
      <time datetime="2026-09-01"></time>
      <span data-c="5"></span>
    </div>
  </article>

</section>

<script type="module">
  function IsValidUUIDv4(uuid) {
    return /^[0-9a-f]{8}-[0-9a-f]{4}-4[0-9a-f]{3}-[89ab][0-9a-f]{3}-[0-9a-f]{12}$/.test(uuid)
  }

  const uuid = new URLSearchParams(window.location.search).get("uuid")
  const input = document.getElementById('input')
  if (uuid)
    if (IsValidUUIDv4(uuid))
      Go(uuid)
    else {
      input.value = uuid
      input.reportValidity()
    }

  input.addEventListener('focus', async function () {
    try {
      const text = (await navigator.clipboard.readText()).trim()

      if (IsValidUUIDv4(text)) {
        this.value = text
        this.dispatchEvent(new Event('input', { bubbles: true }))
      }
    } catch (_) { }
  })

  const form = document.getElementById('uuid-form')
  form.addEventListener('submit', function Jump(e) {
    e.preventDefault()
    Go(input.value)
  })

  function Go(uuid) {
    const dir1 = uuid.substring(0, 2)
    const dir2 = uuid.substring(2, 4)
    const name = uuid.substring(4)
    window.location.replace(`./${dir1}/${dir2}/${name}.html`)
  }
</script>
