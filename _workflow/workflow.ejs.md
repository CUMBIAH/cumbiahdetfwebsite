<%
// Workflow listing for the homepage.
//
// Each analysis document places itself on the grid and declares what it feeds
// into with a `workflow` block in its front matter, e.g.
//
//   workflow:
//     row: 2
//     col: 1
//     outputs:
//       - to: multitrack          # file name of the target document, no .qmd
//         label: "PAMGuard data"  # text shown on the arrow
//         icon: pamguard          # optional: matlab (.mat files) or pamguard,
//                                 # any images/icons/<name>.png
//
// The arrows are drawn by workflow.js from these outputs. On narrow screens the
// arrows are hidden and each card lists its outputs as text instead.

const stem = (item) => (item.filename || "").replace(/\.qmd$/, "");
const titles = {};
for (const item of items) { titles[stem(item)] = item.title; }

let nRows = 1, nCols = 1;
for (const item of items) {
  const wf = item.workflow || {};
  if (wf.row > nRows) nRows = wf.row;
  if (wf.col > nCols) nCols = wf.col;
}

const edges = [];
for (const item of items) {
  for (const out of ((item.workflow || {}).outputs || [])) {
    edges.push({ from: stem(item), to: out.to, label: out.label || "", icon: out.icon || "" });
  }
}
%>

```{=html}
<div class="workflow" style="--wf-cols: <%- nCols %>; --wf-rows: <%- nRows %>;" data-edges="<%- encodeURIComponent(JSON.stringify(edges)) %>">
<svg class="workflow-arrows" aria-hidden="true"></svg>
<div class="workflow-labels" aria-hidden="true"></div>
```

<% for (const item of items) {
  const wf = item.workflow || {};
  const pos = (wf.row && wf.col) ? `grid-row: ${wf.row}; grid-column: ${wf.col};` : "";
  const outputs = wf.outputs || [];
%>

```{=html}
<div class="workflow-node" data-wf-id="<%- stem(item) %>" style="<%- pos %>">
<a href="<%- item.path %>" class="quarto-grid-link">
<div class="quarto-grid-item card h-100 card-left">
```

```{=html}
<p class="card-img-top"><img src="<%- item.image %>" class="thumbnail-image card-img" alt="<%= item['image-alt'] || '' %>" loading="lazy"></p>
```

```{=html}
<div class="card-body post-contents">
<h5 class="no-anchor card-title listing-title"><%= item.title %></h5>
<div class="card-text listing-description delink"><%= item.description %></div>
</div>
</div>
</a>
<% if (outputs.length > 0) { %>
<ul class="workflow-outputs">
<% for (const out of outputs) { %>
<li><% if (out.icon) { %><img class="workflow-icon" src="images/icons/<%- out.icon %>.png" alt="<%- out.icon %>"> <% } %><span class="workflow-outputs-to">&rarr; <%= titles[out.to] || out.to %></span>: <%= out.label %></li>
<% } %>
</ul>
<% } %>
</div>
```

<% } %>

```{=html}
</div>
```
