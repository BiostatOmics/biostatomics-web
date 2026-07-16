```{=html}
<%
  // This is a Quarto EJS template: it turns the tools listed in tools.yml into the HTML cards shown on the Tools page. Quarto gives it a ready-made "items" array, one entry per tool, with whatever fields you wrote in the YAML (item.title, item.image, item.categories...).
  // Three kinds of tags are used below: a code tag that runs JavaScript and prints nothing (loops, ifs, variables), a safe-print tag that prints a value as escaped text, and a raw-print tag that prints a value as-is, unescaped (used for links, image paths, or text that already contains HTML). Their real symbols can't be written here in a comment, because the template engine looks for those symbols everywhere, even inside comments — see the real tags just below for what they look like.

  // Group the flat list of tools by their "section" field (e.g. "Tools", "Web apps") so we can print one heading per group further down.
  const sections = [];
  for (const item of items) {
    const name = item.section || "Other";
    let bucket = sections.find((s) => s.name === name);
    if (!bucket) {
      bucket = { name: name, items: [] };
      sections.push(bucket);
    }
    bucket.items.push(item);
  }

  // Turns a date like "2026-05" into "May 2026".
  const formatDate = (value) => {
    if (!value) return "";
    const d = new Date(value);
    return d.toLocaleDateString('en-US', { month: 'short', year: 'numeric' });
  };
%>

<div class="tool-grid list">
<% for (const section of sections) { %>

  <h2 class="tools-section-title"><%= section.name %></h2>

  <% for (const item of section.items) { %>
    <!-- metadataAttrs(item) is a helper Quarto provides for free: it adds hidden attributes this card needs so search/sort/filter keep working. Always keep it here. -->
    <div class="tool-card" <%= metadataAttrs(item) %>>
      <% if (item.image) { %>
        <img src="<%- item.image %>" class="tool-card-img" alt="<%- item.title %> logo">
      <% } %>
      <div class="tool-card-body">
        <div class="tool-card-heading">
          <h5 class="tool-card-title listing-title"><%= item.title %></h5>
          <span class="tool-card-date listing-date"><%= formatDate(item.date) %></span>
        </div>

        <!-- one badge per category; "|| []" avoids an error if a tool has no categories defined -->
        <div class="tool-card-tags listing-categories">
          <% (item.categories || []).forEach(function(cat) { %>
            <span class="badge tool-badge"><%= cat %></span>
          <% }) %>
        </div>

        <p class="tool-card-text listing-description"><%= item.description %></p>

        <!-- one button per link; each entry in item.links needs a label, an icon (a Bootstrap Icons name) and a url -->
        <div class="tool-card-links">
          <% (item.links || []).forEach(function(link) { %>
            <a href="<%- link.url %>" class="tool-link" target="_blank" rel="noopener">
              <i class="bi bi-<%= link.icon %>"></i> <%= link.label %>
            </a>
          <% }) %>
        </div>
      </div>
    </div>
  <% } %>

<% } %>
</div>
```
