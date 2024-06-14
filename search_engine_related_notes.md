What happens if you render content (list of links) using javascript? Will it get indexed properly by google search?

Link: https://developers.google.com/search/docs/crawling-indexing/javascript/javascript-seo-basics#:~:text=Once%20Google's%20resources%20allow%2C%20a,HTML%20to%20index%20the%20page.

Crawling a URL and parsing the HTML response works well for classical websites or server-side rendered pages where the HTML in the HTTP response contains all content.
Some JavaScript sites may use the app shell model where the initial HTML does not contain the actual content and Google needs to execute JavaScript before being able to see the actual page content that JavaScript generates.

Googlebot queues all pages for rendering, unless a robots meta tag or header tells Google not to index the page. The page may stay on this queue for a few seconds, 
but it can take longer than that. Once Google's resources allow, a headless Chromium renders the page and executes the JavaScript. 
Googlebot parses the rendered HTML for links again and queues the URLs it finds for crawling. Google also uses the rendered HTML to index the page.

Keep in mind that server-side or pre-rendering is still a great idea because it makes your website faster for users and crawlers, and not all bots can run JavaScript.

Google and Bing can run javacript but it takes longer than ususal.

https://webmasters.stackexchange.com/questions/140250/do-search-engines-perform-js-rendering-while-crawling
