```{=html}
<%
  // This is a Quarto EJS template: it turns each person's profile (every people/<name>/index.qmd) into the HTML cards shown on the "Meet the group" page, grouped under the right heading. Quarto gives it a ready-made "items" array, one entry per person found via the "contents:" setting in people/index.qmd, with whatever fields that person's YAML header defines (item.title, item.image, item.people_group...).
  // Three kinds of tags are used below: a code tag that runs JavaScript and prints nothing (loops, ifs, variables), a safe-print tag that prints a value as escaped text, and a raw-print tag that prints a value as-is, unescaped (used for links, image paths, or text that already contains HTML). Their real symbols can't be written here in a comment, because the template engine looks for those symbols everywhere, even inside comments — see the real tags just below for what they look like.

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

          <!-- education_card is a short version of "education" meant just for this compact card, with the degree title in bold (see people/_people-template.qmd); it's optional, so only print this block if the person filled it in -->
          <% if (item.education_card) { %>
            <div class="person-card-education"><%= item.education_card %></div>
          <% } %>
        </div>
      </a>
    <% } %>

  <% } %>

<% } %>
</div>
```
