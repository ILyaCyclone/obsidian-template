<%*
const dateFormat = "YYYY-MM-DD";
const monday = moment(tp.date.weekday(dateFormat, 0), dateFormat);
const previousMonday = moment(monday).subtract(1, "week");
const nextMonday = moment(monday).add(1, "week");
const previousMondayFormat = previousMonday.format(dateFormat);
const nextMondayFormat = nextMonday.format(dateFormat);
const filename = monday.format(dateFormat) + " week";
await tp.file.rename(filename);
_%>
[[<% previousMondayFormat %> week|← <% previousMondayFormat %> week]] | [[<% nextMondayFormat %> week|<% nextMondayFormat %> week →]]
#weekly 

<%*
for(let i = 0; i < 5; i++) {
  const day = moment(monday).add(i, "days");
  const date = day.format(dateFormat);
  const dateUrl = date.replace(/ /g, "%20") + ".md";
  const dayName = day.format("dddd");
  const dayNameCapitalized = dayName.charAt(0).toUpperCase() + dayName.slice(1);
  tR += `# ${dayNameCapitalized}`;
  tR += `\n![${date}](${dateUrl})`;
  tR += "\n\n\n";
}
%>