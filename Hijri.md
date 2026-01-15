<%*
const hijri = await tp.user.hijri();
await tp.file.rename(hijri.trim());
-%>
---
date: <% hijri %>
created: <% tp.date.now("YYYY-MM-DD HH:mm") %>
---

# <% hijri %>

