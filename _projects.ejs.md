```{=html}
<%
  // Quarto EJS (Lodash) template: turns each entry of projects.yml into one compact block on projects.qmd.
  // same custom-listing mechanism as tools/, people/ and publications. NOTE Lodash reverses the print tags vs. the usual .ejs docs: "<%= %>" prints raw/unescaped HTML (used for item.pis, which already contains markup), and "<%- %>" HTML-escapes its value (title, reference, funder, dates).
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
