A collection of bookmarklets for doing stuff on Metaculus.

## Load All Questions

Ever get annoyed that you have to click "Load More" a bunch to browse down to older questions? Just click this bookmarklet to automatically click it until all questions are loaded. Loads 300 questions per second, so it won't happen immediately.

Please don't abuse this one. It could hurt the Metaculus Server.

<a href="javascript:(function()%7B(()%20%3D%3E%20%7B%0Alet%20cycler%20%3D%20setInterval(()%20%3D%3E%20%7B%0A%20%20if(document.querySelectorAll('question-table-row').length%20%3C%20metacData.initialQList.count)%7B%0A%20%20%20%20document.querySelector('._load-more').click()%3B%0A%20%20%7D%20else%20%7B%0A%20%20%20%20clearInterval(cycler)%3B%0A%20%20%7D%0A%7D%2C%20100)%3B%0A%7D)()%3B%7D)()%3B">Load All Questions</a>
