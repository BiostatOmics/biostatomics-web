```{=html}
<%
  // This is a Quarto EJS template: it turns each person's profile (every people/<name>/index.qmd) into the HTML cards shown on the "Meet the group" page, grouped under the right heading. Quarto gives it a ready-made "items" array, one entry per person found via the "contents:" setting in people/index.qmd, with whatever fields that person's YAML header defines (item.title, item.image, item.people_group...).
  // Three kinds of tags are used below: a code tag that runs JavaScript and prints nothing (loops, ifs, variables), a raw-print tag that prints a value as-is, unescaped (used for text that already contains HTML, like the education line below), and an escaping-print tag that HTML-escapes a value first (used for links and image paths, so stray "&" or similar characters in a URL can't break the page). Their real symbols can't be written here in a comment, because the template engine looks for those symbols everywhere, even inside comments — see the real tags just below for what they look like. Note this template engine is Lodash's, not the more common EJS engine, so its escaping tag and raw tag are the reverse of what you may see documented elsewhere for ".ejs" files.

  // The groups, in the exact order they should appear. "key" must match the "people_group" value written in each person's YAML header exactly. "label" is the heading text shown on the page. "cols" is how many cards should fit per row for that group on a normal-width screen (see the ".person-card-cols-*" rules in styles.css) — groups with a lot of members (like alumni) look better with more, smaller cards. To add a new group, add a line here.
  const groups = [
    { key: "pi",                 label: "Principal Investigator",  cols: 3 },
    { key: "research_scientist", label: "Research Scientists",     cols: 3 },
    { key: "postdoc",            label: "Postdoctoral Fellows",    cols: 3 },
    { key: "phd",                label: "PhD Students",            cols: 3 },
    { key: "visitor",            label: "Visiting Researchers",    cols: 3 },
    { key: "student",            label: "Students",                cols: 3 },
    { key: "alumni",             label: "Alumni",                  cols: 5 },
  ];

  // The card shows a short, one-line version of "education", built automatically from the full "education" list in the person's YAML header (a "ul"/"li" HTML list) — so contributors only have to write their degrees once. This pulls out the text of every "li" entry and joins them with a line break.
  function educationCard(educationHtml) {
    if (!educationHtml) return "";
    const entries = [...educationHtml.matchAll(/<li>([\s\S]*?)<\/li>/gi)].map((match) => match[1].trim());
    return entries.join(" <br> ");
  }
%>

<div class="people-grid list">
<% for (const group of groups) { %>

  <% const groupItems = items.filter((item) => item.people_group === group.key); %>

  <!-- only print the heading (and the cards) if this group actually has someone in it — this is what makes empty groups disappear automatically -->
  <% if (groupItems.length > 0) { %>

    <h2 class="people-section-title"><%= group.label %></h2>

    <% for (const item of groupItems) { %>
      <!-- the whole card is a link to that person's own page; item.path is given automatically by Quarto. metadataAttrs(item) is a helper Quarto provides for free: it adds hidden attributes this card needs so search/sort/filter keep working. Always keep it here. "person-card-cols-<N>" sets this card's width so exactly "cols" of them fit per row (see the group definition above). -->
      <a href="<%- item.path %>" class="person-card person-card-cols-<%= group.cols %>" <%= metadataAttrs(item) %>>
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

  <% } %>

<% } %>
</div>
```
