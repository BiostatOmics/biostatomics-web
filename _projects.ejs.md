```{=html}
<%
  // Quarto EJS (Lodash) template: turns each entry of projects.yml into one compact block on projects.qmd.
  // same custom-listing mechanism as tools/, people/ and publications. IMPORTANT: do not write the raw print-tag symbols anywhere in this file, not even in a comment, because the template engine scans for them everywhere (see the same warning in tools/_tools-cards.ejs.md). Lodash also reverses the two print tags vs. the usual .ejs docs: the plain "equals" print tag emits raw/unescaped HTML (used below for item.pis, which already contains markup), and the "dash" print tag HTML-escapes its value first (title, reference, funder, dates).
%>

<ul class="project-list">
<% for (const item of items) { %>
  <!-- metadataAttrs(item) keeps Quarto search/sort working; always keep it. -->
  <li class="project-item" <%= metadataAttrs(item) %>>
    <span class="project-title"><%- item.title %></span>
    <span class="project-meta">
      <span class="project-ref"><%- item.reference %></span>
      <span class="project-dates"><%- item.start %> &ndash; <%- item.end %></span>
    </span>
    <span class="project-pis"><%= item.pis %></span>
    <span class="project-funder"><i class="bi bi-bank"></i> <%- item.funder %></span>
  </li>
<% } %>
</ul>
```
