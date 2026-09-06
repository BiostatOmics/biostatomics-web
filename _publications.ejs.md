```{=html}
<%
  // Quarto EJS (Lodash) template: turns each entry of publications.yml into one compact row on publications.qmd.
  // this is the same custom-listing mechanism used by tools/ and people/. NOTE Lodash reverses the print tags vs. the usual .ejs docs: the "<%= %>" tag here prints raw/unescaped HTML (used for item.authors, which already contains <strong>), and "<%- %>" HTML-escapes its value (used for the title, venue and the DOI in the URL).
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
