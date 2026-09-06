```{=html}
<%
  // Quarto EJS (Lodash) template: turns each entry of publications.yml into one compact row on publications.qmd.
  // same custom-listing mechanism as tools/ and people/. IMPORTANT: do not write the raw print-tag symbols anywhere in this file, not even in a comment, because the template engine scans for them everywhere (see the same warning in tools/_tools-cards.ejs.md). Lodash also reverses the two print tags vs. the usual .ejs docs: the plain "equals" print tag emits raw/unescaped HTML (used below for item.authors, which already contains a strong element), and the "dash" print tag HTML-escapes its value first (used for the title, venue and the DOI in the URL).
%>

<ol class="pub-list">
<% for (const item of items) { %>
  <!-- metadataAttrs(item) keeps Quarto search/sort working; always keep it. -->
  <li class="pub-item" <%= metadataAttrs(item) %>>
    <span class="pub-authors"><%= item.authors %></span>
    <span class="pub-title">
      <% if (item.doi) { %>
        <a href="https://doi.org/<%- item.doi %>" target="_blank" rel="noopener"><%- item.title %></a>
      <% } else { %>
        <%- item.title %>
      <% } %>
    </span>
    <span class="pub-venue"><em><%- item.venue %></em> · <%- item.year %></span>
  </li>
<% } %>
</ol>
```
