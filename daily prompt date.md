<%*
const promptedDate = await tp.system.prompt("Date as DD or MM-DD or MM.DD or YYYY-MM-DD or YYYY.MM.DD", moment().format("YYYY-MM-DD"));
const promptedDateDashed = promptedDate.replace(/\./g, '-');

const today = moment();
const todayYear = today.year();
const todayMonth = today.month() + 1; // month() is zero-indexed
const todayDate = today.date();

const parts = promptedDateDashed.split('-').map(part => parseInt(part, 10));
let date;
switch (parts.length) {
    case 1: // DD
        date = moment(today).date(parts[0]);
        break;
    case 2: // MM-DD
        date = moment(today).month(parts[0] - 1).date(parts[1]);
        break;
    case 3: // YYYY-MM-DD
        date = moment({ year: parts[0], month: parts[1] - 1, day: parts[2] });
        break;
    default:
        alert('Invalid date format');
        return;
}

await tp.file.rename(date.format("YYYY-MM-DD"));

const previousDate = moment(date).subtract(1, "days");
const nextDate = moment(date).add(1, "days");
const mondayOfWeek = moment(date).subtract(date.isoWeekday()-1, 'days')
_%>

---
date: <% date.format("YYYY-MM-DD") %>
dayOfWeek: <% date.format("dddd") %>
---
# <% date.format("DD MMMM YYYY, dddd") %>
[[<% previousDate.format('YYYY-MM-DD') %>|←]] | [[<% mondayOfWeek.format('YYYY-MM-DD')+' week' %>|Неделя]] | [[<% nextDate.format('YYYY-MM-DD') %>|→]]

---

<% tp.file.cursor(1) %>