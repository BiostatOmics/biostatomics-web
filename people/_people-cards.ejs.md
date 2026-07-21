```{=html}
<%
  // This is a Quarto EJS template: it turns each person's profile (every people/<name>/index.qmd) into the HTML cards shown on the "Meet the group" page. Which people show up here is decided in people/index.qmd (one listing block per group, filtered by "people_group" via "include") — this template only has to know how to draw a card for whatever "items" it's given.
  // Three kinds of tags are used below: a code tag that runs JavaScript and prints nothing (loops, ifs, variables), a raw-print tag that prints a value as-is, unescaped (used for text that already contains HTML, like the education line below), and an escaping-print tag that HTML-escapes a value first (used for links and image paths, so stray "&" or similar characters in a URL can't break the page). Their real symbols can't be written here in a comment, because the template engine looks for those symbols everywhere, even inside comments — see the real tags just below for what they look like. Note this template engine is Lodash's, not the more common EJS engine, so its escaping tag and raw tag are the reverse of what you may see documented elsewhere for ".ejs" files.

  // The card shows a short, one-line version of "education", built automatically from the full "education" list in the person's YAML header (a "ul"/"li" HTML list) — so contributors only have to write their degrees once. This pulls out the text of every "li" entry and joins them with a line break.
  function educationCard(educationHtml) {
    if (!educationHtml) return "";
    const entries = [...educationHtml.matchAll(/<li>([\s\S]*?)<\/li>/gi)].map((match) => match[1].trim());
    return entries.join(" <br> ");
  }
%>

<div class="people-grid list">
<% for (const item of items) { %>
  <!-- the whole card is a link to that person's own page; item.path is given automatically by Quarto. metadataAttrs(item) is a helper Quarto provides for free: it adds hidden attributes this card needs so search/sort/filter keep working. Always keep it here. -->
  <a href="<%- item.path %>" class="person-card" <%= metadataAttrs(item) %>>
    <% if (item.image) { %>
      <img src="<%- item.image %>" class="person-card-img" alt="<%= item.title %>">
    <% } %>
    <%
      // "description" often has two parts separated by a literal "<br>", e.g. "PhD Student <br> Sparse regression for genomics".
      // On the card we only want the first part (the role); the full text is still used elsewhere (e.g. as this page's meta description).
      const role = (item.description || "").split("<br>")[0].trim();
    %>
    <div class="person-card-body">
      <h5 class="person-card-title listing-title"><%= item.title %></h5>
      <p class="person-card-role listing-description"><%= role %></p>

      <% const education = educationCard(item.education); %>
      <% if (education) { %>
        <div class="person-card-education"><%= education %></div>
      <% } %>
    </div>
  </a>
<% } %>
</div>
```
