<%*
const clipboard = await tp.system.clipboard();
let url = await tp.system.prompt("Youtube video URL", clipboard);
let page = await tp.obsidian.request({url});
let doc = new DOMParser().parseFromString(page, "text/html");
let $ = s => doc.querySelector(s);

let title = $("meta[property='og:title']").content;
const illegalFilenameChars = /[<>"/\\|?*\x00-\x1F]/g; // excluding colon ':'
let filename = title.replace(/:/g, '.') // replace colon with dot
    .replace(illegalFilenameChars, '') // remove other illegal characters
    .replace(/[. ]+$/, ''); // trim spaces and periods from the end

await tp.file.rename(filename);

//let duration = $("meta[itemprop='duration']").content.slice(2);
//duration = duration.replace(/M/gi, " мин ");
//let duration = duration1.replace(/S/gi, " Sec ");

const durationString = $("meta[itemprop='duration']").content;
const durationPattern = /PT(?:(\d+)H)?(?:(\d+)M)?(?:(\d+)S)?/;
const durationMatches = durationString.match(durationPattern);
const hours = durationMatches[1] ? parseInt(durationMatches[1], 10) : 0;
const minutes = durationMatches[2] ? parseInt(durationMatches[2], 10) : 0;
const seconds = durationMatches[3] ? parseInt(durationMatches[3], 10) : 0;

let duration = "";
if(hours > 0) {
  duration = `${hours} ч `;
}
duration += `${minutes} мин`;

const uploadDateString = $("meta[itemprop='uploadDate']").content; // 2022-10-14T04:49:32-07:00 (ISO8601)
//const uploadDate = moment(uploadDateString, "YYYY-MM-DD[T]HH:mm:ssZ");
const uploadDate = moment(uploadDateString);

const videoId = $("meta[itemprop='identifier']").content;
//const shortUrl = $("link[rel='shortLinkUrl']").href; // https://youtu.be/..
const channelUrl = $("link[itemprop='url']").href;
const channelName = $("link[itemprop='name']").getAttribute("content");
const description = $("meta[itemprop='description']").content;
_%>

---
date: <%tp.date.now()%>
youtube.url: <% url %>
youtube.videoId: <% videoId %>
youtube.channel: <% channelName %>
youtube.videoDate: <% uploadDate.format("YYYY-MM-DD") %>
status: watching
---
#youtube

# <% title %>
- [<% channelName %>](<% channelUrl %>)
- <% uploadDate.format("DD MMMM YYYY") %>
- продолжительность: <% duration %>

>[!Описание]-
> <% description %>

<center><iframe width="560" height="315" src="https://www.youtube.com/embed/<% videoId %>" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe></center>

<% tp.file.cursor(1) %>