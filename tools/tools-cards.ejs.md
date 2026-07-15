```{=html}
<% for (const group of items) { %>

<h2 class="tools-section-title"><%= group.category %></h2>

<div class="tool-grid">
<% for (const item of group.tiles) { %>
  <div class="tool-card">
    <% if (item.image) { %>
      <img src="<%- item.image %>" class="tool-card-img" alt="<%- item.title %> logo">
    <% } %>
    <div class="tool-card-body">
      <h5 class="tool-card-title"><%= item.title %></h5>
      <div class="tool-card-tags">
        <% (item.tags || []).forEach(function(tag) { %>
          <span class="badge tool-badge"><%= tag %></span>
        <% }) %>
      </div>
      <p class="tool-card-text"><%= item.description %></p>
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
</div>

<% } %>
```
