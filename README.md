## How to use these

[Bookmarklets](https://en.wikipedia.org/wiki/Bookmarklet) are an ancient bit of web technology which embeds Javascript in a URL. The idea is, you copy the bookmarklet to your bookmarks bar, and then click it on a site to execute the javascript embedded in it. In general, bookmarklets are [kind of dangerous](https://en.wikipedia.org/wiki/Self-XSS) and so not widely used anymore (Browser plugins are now preferred.) But it's a really fast and simple way to create custom code that you can kick off easily. I promise that these don't do anything nefarious! (But why on Earth would you trust me?)

### Download Your Track Record (binary questions only, as csv)

```javascript
(() => {
  async function loadScript(url){
    let script   = document.createElement("script");
    script.type  = "text/javascript";
    script.src   = url;
    document.body.appendChild(script);
  }

  loadScript("https://cdnjs.cloudflare.com/ajax/libs/PapaParse/5.1.0/papaparse.min.js").then(() => {
    loadScript("https://cdnjs.cloudflare.com/ajax/libs/FileSaver.js/1.3.8/FileSaver.min.js").then(() => {
      fetch(`https://www.metaculus.com/accounts/profile/${metacData.user.id}/track-record-export/`).then(data1 => data1.json().then(predictions => {
        saveAs(new Blob([Papa.unparse(predictions)], {type: "text/csv;charset=utf-8"}), "track-record.csv");
      }));
    });
  });
})();
```

### Download the Metaculus Track Record (binary questions only, as csv)

```javascript
(() => {
  async function loadScript(url){
    let script   = document.createElement("script");
    script.type  = "text/javascript";
    script.src   = url;
    document.body.appendChild(script);
  }

  loadScript("https://cdnjs.cloudflare.com/ajax/libs/PapaParse/5.1.0/papaparse.min.js").then(() => {
    loadScript("https://cdnjs.cloudflare.com/ajax/libs/FileSaver.js/1.3.8/FileSaver.min.js").then(() => {
      fetch(`https://www.metaculus.com/questions/track-record/export/`).then(data1 => data1.json().then(predictions => {
        saveAs(new Blob([Papa.unparse(predictions)], {type: "text/csv;charset=utf-8"}), "metaculus-track-record.csv");
      }));
    });
  });
})();
```

### Download a Question's Prediction Data

Metaculus exposes a time-series of a question's distribution of predictions in order to render the question's line plot. Using this bookmarklet, we can extract the raw data from the question page and export it to CSV. 

<a href="javascript:(function()%7B(()%20%3D%3E%20%7Basync%20function%20loadScript(url)%7Blet%20script%20%20%20%3D%20document.createElement(%22script%22)%3Bscript.type%20%20%3D%20%22text%2Fjavascript%22%3Bscript.src%20%20%20%3D%20url%3Bdocument.body.appendChild(script)%3B%7DloadScript(%22https%3A%2F%2Fcdnjs.cloudflare.com%2Fajax%2Flibs%2FPapaParse%2F5.1.0%2Fpapaparse.min.js%22).then(()%20%3D%3E%20%7BloadScript(%22https%3A%2F%2Fcdnjs.cloudflare.com%2Fajax%2Flibs%2FFileSaver.js%2F1.3.8%2FFileSaver.min.js%22).then(()%20%3D%3E%20%7BsaveAs(new%20Blob(%5BPapa.unparse(metacData.question.prediction_timeseries.map(t%20%3D%3E%20Object.assign(t%2C%20t.distribution)))%5D%2C%20%7Btype%3A%20%22text%2Fcsv%3Bcharset%3Dutf-8%22%7D)%2C%20%22predictions.csv%22)%3B%7D)%3B%7D)%3B%7D)()%7D)()">Download Prediction Time-series</a>

Here's the underlying code:

```javascript
(() => {
  async function loadScript(url){
    let script   = document.createElement("script");
    script.type  = "text/javascript";
    script.src   = url;
    document.body.appendChild(script);
  }

  loadScript("https://cdnjs.cloudflare.com/ajax/libs/PapaParse/5.1.0/papaparse.min.js").then(() => {
    loadScript("https://cdnjs.cloudflare.com/ajax/libs/FileSaver.js/1.3.8/FileSaver.min.js").then(() => {
      saveAs(new Blob([Papa.unparse(metacData.question.prediction_timeseries.map(t => Object.assign(t, t.distribution)))], {type: "text/csv;charset=utf-8"}), "predictions.csv");
    });
  });
})();
```

## Danger Zone

**Please don't abuse bookmarklets below this. They rely on repeated calls to the Metaculus API, meaning that hammering them could hurt the Metaculus Server.**

### Download Your Track Record (All questions, as csv)

(Based on [this comment](https://www.metaculus.com/questions/935/discussion-topic-what-features-should-metaculus-add-may-2018-edition/#comment-20248).)

```javascript
(() => {
  async function loadScript(url){
    let script   = document.createElement("script");
    script.type  = "text/javascript";
    script.src   = url;
    document.body.appendChild(script);
  }

  loadScript("https://cdnjs.cloudflare.com/ajax/libs/PapaParse/5.1.0/papaparse.min.js").then(() => {
    loadScript("https://cdnjs.cloudflare.com/ajax/libs/FileSaver.js/1.3.8/FileSaver.min.js").then(() => {
      fetch(`https://www.metaculus.com/api2/questions/?guessed_by=${metacData.user.id}&order_by=-activity&page=1`).then(data1 => data1.json().then(predictions => {
        let n = Math.ceil(predictions.count/20);
        let requests = [];
        for(let i = 2; i <= n; i++){
          requests.push(
            fetch(`https://www.metaculus.com/api2/questions/?guessed_by=${metacData.user.id}&order_by=-activity&page=${i}`).then(data2 => data2.json().then(page => {
              predictions.results = predictions.results.concat(page.results);
            }))
          );
        }
        Promise.all(requests).then(() => saveAs(new Blob([Papa.unparse(predictions.results)], {type: "text/csv;charset=utf-8"}), "track-record.csv"));
      }));
    });
  });
})();
```

## Load All Questions

Ever get annoyed that you have to click "Load More" a bunch to browse down to older questions? Just click this bookmarklet to automatically click it until all questions are loaded. Loads 300 questions per second, so it won't happen immediately.

<a href="javascript:(function()%7B(()%20%3D%3E%20%7B%0Alet%20cycler%20%3D%20setInterval(()%20%3D%3E%20%7B%0A%20%20if(document.querySelectorAll('question-table-row').length%20%3C%20metacData.initialQList.count)%7B%0A%20%20%20%20document.querySelector('._load-more').click()%3B%0A%20%20%7D%20else%20%7B%0A%20%20%20%20clearInterval(cycler)%3B%0A%20%20%7D%0A%7D%2C%20100)%3B%0A%7D)()%3B%7D)()%3B">Load All Questions</a>

And here's the code: 

```javascript
(() => {
let cycler = setInterval(() => {
  if(document.querySelectorAll('question-table-row').length < metacData.initialQList.count){
    document.querySelector('._load-more').click();
  } else {
    clearInterval(cycler);
  }
}, 100);
})();
```
