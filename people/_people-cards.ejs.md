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

  // "thesis_directors" is written once in the YAML header (as HTML, so a director's name can link to their own page — see _people-template.qmd) and reused on the person's own page via "{{< meta thesis_directors >}}", same trick as "education" above. That shortcode logs a harmless "Unknown meta key thesis_directors" warning at render time (something about how listing generation resolves shortcodes — it isn't about the underscore, a hyphenated version warns identically); the actual output is unaffected either way, so it's not worth chasing further. The card's hover tooltip, though, can't reuse that HTML as-is: it already sits inside the "Directors" link itself, and a "<a>" can't contain another "<a>". This strips any tags back out for that one spot, leaving plain text.
  function stripTags(html) {
    if (!html) return "";
    return html.replace(/<[^>]+>/g, "");
  }
%>

<div class="people-grid list">
<% for (const item of items) { %>
  <!-- unlike a plain link-card, this one needs a *second*, independently clickable link in its footer (the "thesis directors" one below) — an anchor can't contain another anchor, so instead of the whole card being one big "<a>", it's a "<div>" with a "stretched-link" (a Bootstrap utility) on the title. That stretches an invisible layer over the whole card, making it clickable everywhere the same as before, while still leaving room for the footer's own real link — same trick already used for the tool cards in tools/_tools-cards.ejs.md. metadataAttrs(item) is a helper Quarto provides for free: it adds hidden attributes this card needs so search/sort/filter keep working. Always keep it here. -->
  <div class="person-card" <%= metadataAttrs(item) %>>
    <% if (item.image) { %>
      <img src="<%- item.image %>" class="person-card-img" alt="<%= item.title %>">
    <% } %>
    <div class="person-card-body">
      <h5 class="person-card-title listing-title"><a href="<%- item.path %>" class="stretched-link"><%= item.title %></a></h5>
      <p class="person-card-role listing-description"><%- item.description %></p>
      <% const education = educationCard(item.education); %>
      <% if (education) { %>
        <div class="person-card-education"><%= education %></div>
      <% } %>
    </div>
    <!-- "institution" (eg. "UPV", "CSIC-UPV") and "thesis_directors" are both optional and only used for active predoctoral researchers. When either is set, they share a footer bar beneath the main card — a link to the directors on the left, the institution badge on the right. -->
    <% if (item.institution || item.thesis_directors) { %>
      <div class="person-card-footer">
        <% if (item.thesis_directors) { %>
          <!-- links to the "## Thesis directors" section on the person's own page (that section is just "{{< meta thesis_directors >}}" — see _people-template.qmd). The names themselves don't fit compactly on the card, so instead of always showing them, they're tucked into a hover tooltip on this link — run through stripTags() since the tooltip sits inside this very link and so can't itself contain the director's own "<a>" link. "position: relative" keeps this individually clickable above the title's "stretched-link" overlay, same fix as ".tool-card-links" in the tools template. -->
          <a href="<%- item.path %>#thesis-directors" class="person-card-directors-link">
            <i class="bi bi-mortarboard-fill"></i> Directors
            <span class="person-card-directors-tooltip"><%- stripTags(item.thesis_directors) %></span>
          </a>
        <% } %>
        <% if (item.institution) { %>
          <!-- raw-printed (not escaped) so a person can bold the institution they're physically based at, eg. "<strong>UPV</strong>-CSIC" for someone co-affiliated but physically at UPV. -->
          <span class="person-card-institution"><%= item.institution %></span>
        <% } %>
      </div>
    <% } %>
  </div>
<% } %>
</div>
```
