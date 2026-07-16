```{=html}
<%
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
    <div class="tool-card" <%= metadataAttrs(item) %>>
      <% if (item.image) { %>
        <img src="<%- item.image %>" class="tool-card-img" alt="<%- item.title %> logo">
      <% } %>
      <div class="tool-card-body">
        <div class="tool-card-heading">
          <h5 class="tool-card-title listing-title"><%= item.title %></h5>
          <span class="tool-card-date listing-date"><%= formatDate(item.date) %></span>
        </div>
        <div class="tool-card-tags listing-categories">
          <% (item.categories || []).forEach(function(cat) { %>
            <span class="badge tool-badge"><%= cat %></span>
          <% }) %>
        </div>
        <p class="tool-card-text listing-description"><%= item.description %></p>
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
