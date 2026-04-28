# WordPress

WordPress is the easiest platform for AI files because of the plugin ecosystem. Most files can be added with a plugin click; the rest with a small upload.

## `robots.txt`

**Easy way:** Yoast SEO or Rank Math. Both manage robots.txt through a UI.

- Yoast: SEO → Tools → File editor → robots.txt
- Rank Math: Rank Math → General Settings → Edit robots.txt

**Manual way:** Upload via FTP/cPanel to your site root. WordPress generates a virtual robots.txt by default; uploading a real file overrides it.

## `llms.txt`

**Plugins that auto-generate:**

- **Yoast SEO** (4M+ installs) — settings panel includes llms.txt generation, refreshes weekly via cron
- **Rank Math SEO** — similar built-in support
- **Website LLMs.txt** (30,000+ installs) — dedicated plugin, also tracks AI crawler hits

**Manual way:**

1. Generate the file (use the [`llms-txt` prompt](../../prompts/llms-txt.md))
2. Upload to your site root via FTP, cPanel File Manager, or hosting dashboard
3. Verify at `yoursite.com/llms.txt`

## `llms-full.txt`

No plugin reliably handles this yet. Generate manually:

1. Use a tool like [Firecrawl](https://llmstxt.firecrawl.dev/) to crawl your site and produce a single markdown blob
2. Upload to root as `llms-full.txt`
3. Re-run quarterly or after major content updates

## `.md` routes for every page

Two approaches:

**Plugin-based:** As of 2026 there's no widely-adopted plugin doing this well. Watch the space.

**Manual via theme function:** Add to `functions.php`:

```php
add_action('init', function() {
    add_rewrite_rule('^(.+)\.md$', 'index.php?markdown=1&pagename=$1', 'top');
});

add_filter('query_vars', function($vars) {
    $vars[] = 'markdown';
    return $vars;
});

add_action('template_redirect', function() {
    if (get_query_var('markdown')) {
        $post = get_post();
        if ($post) {
            header('Content-Type: text/markdown; charset=utf-8');
            echo "# {$post->post_title}\n\n";
            echo wp_strip_all_tags($post->post_content);
            exit;
        }
    }
});
```

Run **Settings → Permalinks → Save** after adding to flush rewrite rules.

## `schema.org` JSON-LD

**Plugins:** Yoast SEO and Rank Math both add it automatically. They cover most use cases (Article, BreadcrumbList, Organization). For specific types (Recipe, Product, Event), you may need a dedicated plugin like **Schema Pro** or **WP SEO Structured Data Schema**.

## `ai.txt` (Spawning)

Manual upload only — no plugin yet. Generate with the [`ai-txt-spawning` prompt](../../prompts/ai-txt-spawning.md), upload to root.

## `tdmrep.json`

Manual upload to `/.well-known/tdmrep.json`. WordPress doesn't expose `.well-known/` by default — you may need to:

1. Create `.well-known/` directory in your site root via FTP
2. Add `.htaccess` rules if Apache blocks dotfile directories:

```apache
<Directory ".well-known">
    Allow from all
    Require all granted
</Directory>
```

3. Verify at `yoursite.com/.well-known/tdmrep.json`

## `AGENTS.md`

Not for WordPress sites — `AGENTS.md` lives in code repositories, not on websites.

## Verification

```bash
# Run these against your site
curl -I https://yoursite.com/robots.txt
curl -I https://yoursite.com/llms.txt
curl -I https://yoursite.com/.well-known/tdmrep.json
```

All should return `200`.

## Heads up

- **Caching plugins** (WP Rocket, W3 Total Cache, etc.) can cache the wrong content type. Test in incognito.
- **Cloudflare default settings** now block AI bots. Check your Cloudflare dashboard separately from your `robots.txt`.
- **Hosting providers** sometimes block `.well-known/` paths. If `tdmrep.json` 404s, contact support.
