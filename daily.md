---
date: "{{date}}"
dayOfWeek: "{{date:dddd}}"
---
# <% moment(tp.file.title,'YYYY-MM-DD').format("DD MMMM YYYY, dddd") %>
[[<% fileDate = moment(tp.file.title, 'YYYY-MM-DD').subtract(1, 'd').format('YYYY-MM-DD') %>|←]] | [[<% moment(tp.file.title,'YYYY-MM-DD').subtract(moment(tp.file.title,'YYYY-MM-DD').isoWeekday()-1, 'days').format('YYYY-MM-DD')+' week' %>|Неделя]] | [[<% fileDate = moment(tp.file.title, 'YYYY-MM-DD').add(1, 'd').format('YYYY-MM-DD') %>|→]]

---

<% tp.file.cursor(1) %>