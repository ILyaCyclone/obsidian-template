#weekly 

<%*
const dateFormat = "YYYY-MM-DD";
const monday = moment(tp.date.weekday(dateFormat, 0), dateFormat);
const filename = monday.format(dateFormat) + " week";
await tp.file.rename(filename);

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