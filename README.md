## About

I wrote these scripts at various points for my own benefit. Recently it occurred to me that maybe someone else could also use them, so I wrapped them up as bookmarklets. Hence, this site.

## How to use these

[Bookmarklets](https://en.wikipedia.org/wiki/Bookmarklet) are an ancient bit of web technology which embeds Javascript in a URL. The idea is, you copy the bookmarklet to your bookmarks bar, and then click it on a site to execute the javascript embedded in it. In general, bookmarklets are [kind of dangerous](https://en.wikipedia.org/wiki/Self-XSS) and so not widely used anymore (Browser plugins are now preferred.) But it's a really fast and simple way to create custom code that you can kick off easily. I promise that these don't do anything nefarious! If you're paranoid and can speak Javascript, I've included the code below each bookmarklet, and you can [URL-decode](https://www.urldecoder.org/) the bookmarklet to confirm that they don't send data anywhere it isn't supposed to go.

Anyway, to use these:

1. [Show your bookmarks Bar.](https://www.computerhope.com/issues/ch001917.htm)
2. Find the bookmarklet that does the thing you want. The actual bookmarklets are the <a href="#" style="color:red;">underlined text shown in red</a> below.
3. Drag that bookmarklet to your bookmarks bar, and drop it there. It should now appear there, even if you surf away from this site.
4. Go to https://metaculus.com/ (or the [specific subdomain](https://pandemic.metaculus.com/questions/) in which you're interested).
5. Go to the place where you can see the data you want. If you want a time-series of predictions on a single question, go to that question's page. If you want data on track records or all questions, you can probably do that most anywhere on the site. Bookmarklets with specific rules are noted in their descriptions.
6. Click your bookmarklet. Whatever should happen, uh, should happen. If it doesn't, [please let me know.](https://github.com/AABoyles/Metaculus-Hacks/issues/new/choose)

## Disclaimer

* These are very much *hacks*, so I don't guarantee they'll work.
* If anyone at Metaculus [requests that any or all of these be removed](https://github.com/AABoyles/Metaculus-Hacks/issues/new/choose), **I pre-commit to complying with their request.**

### Download Your Track Record (binary questions only, as csv)

This makes use of [the super-secret official-but-unsupported Metaculus API](https://www.metaculus.com/questions/935/discussion-topic-what-features-should-metaculus-add-may-2018-edition/#comment-6452), which only reports binary questions. If you want your track record across all questions, there's [a bookmarklet for that too](#download-your-track-record-all-questions-as-csv) (but please note that it's in [the danger zone](#danger-zone)!)

<a style="color: red !important;" href="javascript:(function()%7B(()%20%3D%3E%20%7Basync%20function%20loadScript(url)%7Blet%20script%20%20%20%3D%20document.createElement(%22script%22)%3Bscript.type%20%20%3D%20%22text%2Fjavascript%22%3Bscript.src%20%20%20%3D%20url%3Bdocument.body.appendChild(script)%3B%7DloadScript(%22https%3A%2F%2Fcdnjs.cloudflare.com%2Fajax%2Flibs%2FPapaParse%2F5.1.0%2Fpapaparse.min.js%22).then(()%20%3D%3E%20%7BloadScript(%22https%3A%2F%2Fcdnjs.cloudflare.com%2Fajax%2Flibs%2FFileSaver.js%2F1.3.8%2FFileSaver.min.js%22).then(()%20%3D%3E%20%7Bfetch(%60https%3A%2F%2Fwww.metaculus.com%2Faccounts%2Fprofile%2F%24%7BmetacData.user.id%7D%2Ftrack-record-export%2F%60).then(data1%20%3D%3E%20data1.json().then(predictions%20%3D%3E%20%7BsaveAs(new%20Blob(%5BPapa.unparse(predictions)%5D%2C%20%7Btype%3A%20%22text%2Fcsv%3Bcharset%3Dutf-8%22%7D)%2C%20%22track-record.csv%22)%3B%7D))%3B%7D)%3B%7D)%3B%7D)()%7D)()">Download My Track Record</a>

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

If you want to analyze Metaculus' Track record (again, on Binary questions only), you can get that data (also from [the super-secret official-but-unsupported Metaculus API](https://www.metaculus.com/questions/935/discussion-topic-what-features-should-metaculus-add-may-2018-edition/#comment-6452)) by running this Bookmarklet.

<a style="color:red!important" href="javascript:(function()%7B(()%20%3D%3E%20%7Basync%20function%20loadScript(url)%7Blet%20script%20%20%20%3D%20document.createElement(%22script%22)%3Bscript.type%20%20%3D%20%22text%2Fjavascript%22%3Bscript.src%20%20%20%3D%20url%3Bdocument.body.appendChild(script)%3B%7DloadScript(%22https%3A%2F%2Fcdnjs.cloudflare.com%2Fajax%2Flibs%2FPapaParse%2F5.1.0%2Fpapaparse.min.js%22).then(()%20%3D%3E%20%7BloadScript(%22https%3A%2F%2Fcdnjs.cloudflare.com%2Fajax%2Flibs%2FFileSaver.js%2F1.3.8%2FFileSaver.min.js%22).then(()%20%3D%3E%20%7Bfetch(%60https%3A%2F%2Fwww.metaculus.com%2Fquestions%2Ftrack-record%2Fexport%2F%60).then(data1%20%3D%3E%20data1.json().then(predictions%20%3D%3E%20%7BsaveAs(new%20Blob(%5BPapa.unparse(predictions)%5D%2C%20%7Btype%3A%20%22text%2Fcsv%3Bcharset%3Dutf-8%22%7D)%2C%20%22metaculus-track-record.csv%22)%3B%7D))%3B%7D)%3B%7D)%3B%7D)()%7D)()">Download Metaculus Track Record</a>

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

<a style="color: red !important;" href="javascript:(function()%7B(()%20%3D%3E%20%7Basync%20function%20loadScript(url)%7Blet%20script%20%20%20%3D%20document.createElement(%22script%22)%3Bscript.type%20%20%3D%20%22text%2Fjavascript%22%3Bscript.src%20%20%20%3D%20url%3Bdocument.body.appendChild(script)%3B%7DloadScript(%22https%3A%2F%2Fcdnjs.cloudflare.com%2Fajax%2Flibs%2FPapaParse%2F5.1.0%2Fpapaparse.min.js%22).then(()%20%3D%3E%20%7BloadScript(%22https%3A%2F%2Fcdnjs.cloudflare.com%2Fajax%2Flibs%2FFileSaver.js%2F1.3.8%2FFileSaver.min.js%22).then(()%20%3D%3E%20%7BsaveAs(new%20Blob(%5BPapa.unparse(metacData.question.prediction_timeseries.map(t%20%3D%3E%20Object.assign(t%2C%20t.distribution)))%5D%2C%20%7Btype%3A%20%22text%2Fcsv%3Bcharset%3Dutf-8%22%7D)%2C%20%22predictions.csv%22)%3B%7D)%3B%7D)%3B%7D)()%7D)()">Download Prediction Time-series</a>

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

### Download A Community Prediction Plot (as PNG)

The graphs on Metaculus are rendered using SVG, which is awesome for two things: 1. the web and 2. print. Not so good for exporting (e.g. into a document). Luckily it can be converted into PNG without too much trouble. This means we can write a hack to download a PNG of a graph.

Things to be aware of:

* You may need to run this one twice, as the external library doesn't load quickly enough to work correctly the first time. In a proper setting I'd fix the properly, but [I did warn you](#disclaimer) these are just hacks!
* This doesn't bring the full stylesheet, so at present it just ends up looking like a big dark smear across a transparent background.
* The graphs on Metaculus are *tiny*, so this blows it up 10-fold.

<a style="color:red" href="javascript:(function()%7B(()%20%3D%3E%20%7Basync%20function%20loadScript(url)%7Blet%20script%20%20%20%3D%20document.createElement(%22script%22)%3Bscript.type%20%20%3D%20%22text%2Fjavascript%22%3Bscript.src%20%20%20%3D%20url%3Bdocument.body.appendChild(script)%3B%7DloadScript(%22https%3A%2F%2Funpkg.com%2Fsave-svg-as-png%401.4.17%2Flib%2FsaveSvgAsPng.js%22).then(()%20%3D%3E%20%7BsaveSvgAsPng(document.querySelectorAll(%22svg.metac-graph%22)%5B0%5D%2C%20%22diagram.png%22%2C%20%7Bscale%3A%2010%7D)%3B%7D)%3B%7D)()%7D)()">Export Graph</a>

```javascript
(() => {
  async function loadScript(url){
    let script   = document.createElement("script");
    script.type  = "text/javascript";
    script.src   = url;
    document.body.appendChild(script);
  }

  loadScript("https://unpkg.com/save-svg-as-png@1.4.17/lib/saveSvgAsPng.js").then(() => {
    saveSvgAsPng(document.querySelectorAll("svg.metac-graph")[0], "diagram.png", {scale: 10});
  });
})();
```

## Danger Zone

**Please don't abuse bookmarklets below this. They rely on repeated calls to the Metaculus API, meaning that hammering them could hurt the Metaculus Server.**

### Download Your Track Record (All questions, as csv)

Same idea [as above](#download-your-track-record-binary-questions-only-as-csv), only this one side-steps the API to get all question types, not just binary questions. Makes a bunch of fetch calls, so it can take a minute. (Based on [this comment](https://www.metaculus.com/questions/935/discussion-topic-what-features-should-metaculus-add-may-2018-edition/#comment-20248).)

<a style="color: red !important;" href="javascript:(function()%7B(()%20%3D%3E%20%7Basync%20function%20loadScript(url)%7Blet%20script%20%20%20%3D%20document.createElement(%22script%22)%3Bscript.type%20%20%3D%20%22text%2Fjavascript%22%3Bscript.src%20%20%20%3D%20url%3Bdocument.body.appendChild(script)%3B%7DloadScript(%22https%3A%2F%2Fcdnjs.cloudflare.com%2Fajax%2Flibs%2FPapaParse%2F5.1.0%2Fpapaparse.min.js%22).then(()%20%3D%3E%20%7BloadScript(%22https%3A%2F%2Fcdnjs.cloudflare.com%2Fajax%2Flibs%2FFileSaver.js%2F1.3.8%2FFileSaver.min.js%22).then(()%20%3D%3E%20%7Bfetch(%60https%3A%2F%2Fwww.metaculus.com%2Fapi2%2Fquestions%2F%3Fguessed_by%3D%24%7BmetacData.user.id%7D%26order_by%3D-activity%26page%3D1%60).then(data1%20%3D%3E%20data1.json().then(predictions%20%3D%3E%20%7Blet%20n%20%3D%20Math.ceil(predictions.count%2F20)%3Blet%20requests%20%3D%20%5B%5D%3Bfor(let%20i%20%3D%202%3B%20i%20%3C%3D%20n%3B%20i%2B%2B)%7Brequests.push(fetch(%60https%3A%2F%2Fwww.metaculus.com%2Fapi2%2Fquestions%2F%3Fguessed_by%3D%24%7BmetacData.user.id%7D%26order_by%3D-activity%26page%3D%24%7Bi%7D%60).then(data2%20%3D%3E%20data2.json().then(page%20%3D%3E%20%7Bpredictions.results%20%3D%20predictions.results.concat(page.results)%3B%7D)))%3B%7DPromise.all(requests).then(()%20%3D%3E%20saveAs(new%20Blob(%5BPapa.unparse(predictions.results)%5D%2C%20%7Btype%3A%20%22text%2Fcsv%3Bcharset%3Dutf-8%22%7D)%2C%20%22track-record.csv%22))%3B%7D))%3B%7D)%3B%7D)%3B%7D)()%7D)()">Download My Full Track Record</a>

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

### Load All Questions

Ever get annoyed that you have to click "Load More" a bunch to browse down to older questions? Just click this bookmarklet to automatically click it until all questions are loaded. Loads a maximum of 300 questions per second (plus additional latency), so it won't happen immediately, but it's still faster than clicking it yourself.

<a style="color: red !important;" href="javascript:(function()%7B(()%20%3D%3E%20%7B%0Alet%20cycler%20%3D%20setInterval(()%20%3D%3E%20%7B%0A%20%20if(document.querySelectorAll('question-table-row').length%20%3C%20metacData.initialQList.count)%7B%0A%20%20%20%20document.querySelector('._load-more').click()%3B%0A%20%20%7D%20else%20%7B%0A%20%20%20%20clearInterval(cycler)%3B%0A%20%20%7D%0A%7D%2C%20100)%3B%0A%7D)()%3B%7D)()%3B">Load All Questions</a>

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

## Suggest a Hack

Think I might be able to make Metaculus work a little better for you? [Suggest a new hack](https://github.com/AABoyles/Metaculus-Hacks/issues/new/choose) and I'll look into implementing it!
