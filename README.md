## About

I wrote these scripts at various points for my own benefit. Recently it occurred to me that maybe someone else could also use them, so I wrapped them up as bookmarklets. Hence, this site.

## How to use these

[Bookmarklets](https://en.wikipedia.org/wiki/Bookmarklet) are an ancient bit of web technology which embeds Javascript in a URL. The idea is, you copy the bookmarklet to your bookmarks bar, and then click it on a site to execute the javascript embedded in it. In general, bookmarklets are [kind of dangerous](https://en.wikipedia.org/wiki/Self-XSS) and so not widely used anymore (Browser plugins are now preferred.) But it's a really fast and simple way to create custom code that you can kick off easily. I promise that these don't do anything nefarious! If you're paranoid and can speak Javascript, I've included the code below each bookmarklet, and you can [URL-decode](https://www.urldecoder.org/) the bookmarklet to confirm that they don't send data anywhere it isn't supposed to go.

Anyway, to use these:

1. [Show your bookmarks Bar.](https://www.computerhope.com/issues/ch001917.htm)
2. Find the bookmarklet that does the thing you want. The actual bookmarklets are the <a href="#" style="color:red;">underlined text shown in red</a> below.
3. Drag that bookmarklet to your bookmarks bar, and drop it there. It should now appear there, even if you surf away from this site.
4. Go to [Metaculus](https://metaculus.com/) (or the [specific subdomain](https://pandemic.metaculus.com/questions/) in which you're interested).
5. Go to the place where you can see the data you want. If you want a time-series of predictions on a single question, go to that question's page. If you want data on track records or all questions, you can probably do that most anywhere on the site. Bookmarklets with specific rules are noted in their descriptions.
6. Click your bookmarklet. Whatever should happen, uh, should happen. If it doesn't, [please let me know.](https://github.com/AABoyles/Metaculus-Hacks/issues/new/choose)

## Disclaimer

* These are very much *hacks*. I don't guarantee any of them will work.
* If anyone at Metaculus [requests that any or all of these be removed](https://github.com/AABoyles/Metaculus-Hacks/issues/new/choose), **I pre-commit to complying with their request.** (Metaculus Admins, allow me to suggest you take a concerned look at the [Danger Zone](#danger-zone).)

## For Forecasters

### Toggle the Community Estimate

Requested by Michal Dubrawski. Keep yourself epistemically honest! Make predictions *before* you see the community estimate. Click the bookmarklet to toggle the visibility of the community estimate distribution information.

<a style="color: red !important;" href="javascript:(function()%7B(self%20%3D%3E%20%7Bif(typeof%20metHackData%20%3D%3D%20%22undefined%22)%20metHackData%20%3D%20%7BcommunityDisplay%3A%20%22flex%22%7D%3Bif(metHackData.communityDisplay%20%3D%3D%20%22flex%22)%20metHackData.communityDisplay%20%3D%20%22none%22%3Belse%20metHackData.communityDisplay%20%3D%20%22flex%22%3B%5B'svg.ng-scope'%2C'.prediction-toggle'%2C'div.ng-scope%20%3E%20svg.metac-graph'%2C'%5Bng-if%3D%22question.communityPrediction()%22%5D'%2C'.handle.avg-guess'%2C'.label.avg-guess'%2C'.binary_stats'%5D.forEach(s%20%3D%3E%20%7Bdocument.querySelectorAll(s).forEach(el%20%3D%3E%20%7Bel.style.display%3DmetHackData.communityDisplay%3B%7D)%3B%7D)%3B%7D)(self)%7D)()">Toggle Community Estimates</a>

```javascript
(self => {
  if(typeof metHackData == "undefined") metHackData = {communityDisplay: "flex"};
  if(metHackData.communityDisplay == "flex") metHackData.communityDisplay = "none";
  else metHackData.communityDisplay = "flex";
  [
    'svg.ng-scope',
    '.prediction-toggle',
    'div.ng-scope > svg.metac-graph',
    '[ng-if="question.communityPrediction()"]',
    '.handle.avg-guess',
    '.label.avg-guess',
    '.binary_stats'
  ].forEach(s => {
  	document.querySelectorAll(s).forEach(el => {
  	  el.style.display=metHackData.communityDisplay;
  	});
  });
})(self);
```

### Predict the Community Estimate (binary questions only)

If you don't predict on everything, you'll fall off the leaderboard. But let's be honest, nobody's interested in every question. Sometimes you just want to peg your prediction to the community estimate and move on. This bookmarklet does it for you! On a Binary question page (only binary questions for now!), click this bookmarklet and you'll register a prediction equal to the community estimate.

<a style="color: red !important;" href="javascript:(function()%7B(()%20%3D%3E%20%7B%0A%20%20let%20p%20%3D%20metacData.question.prediction_timeseries%3B%0A%20%20fetch(%60%2Fapi2%2Fquestions%2F%24%7BmetacData.question.id%7D%2Fpredict%2F%60%2C%20%7B%0A%20%20%20%20method%3A%20'POST'%2C%0A%20%20%20%20headers%3A%20%7B%0A%20%20%20%20%20%20'Accept'%3A%20'application%2Fjson%2C%20text%2Fplain%2C%20*%2F*'%2C%0A%20%20%20%20%20%20'Content-Type'%3A%20'application%2Fjson%3Bcharset%3Dutf-8'%2C%0A%20%20%20%20%20%20'X-CSRFToken'%3A%20document.cookie.split('%3D')%5B1%5D%2C%0A%20%20%20%20%20%20'x-requested-with'%3A%20'XMLHttpRequest'%0A%20%20%20%20%7D%2C%0A%20%20%20%20body%3A%20JSON.stringify(%7Bprediction%3A%20p%5Bp.length-1%5D.community_prediction%2C%20void%3A%20false%7D)%0A%20%20%7D)%3B%0A%7D)()%3B%7D)()%3B">Predict Community Estimate</a>

```javascript
(() => {
  let p = metacData.question.prediction_timeseries;
  fetch(`/api2/questions/${metacData.question.id}/predict/`, {
    method: 'POST',
    headers: {
      'Accept': 'application/json, text/plain, */*',
      'Content-Type': 'application/json;charset=utf-8',
      'X-CSRFToken': document.cookie.split('=')[1],
      'x-requested-with': 'XMLHttpRequest'
    },
    body: JSON.stringify({prediction: p[p.length-1].community_prediction, void: false})
  });
})();
```

### Load Question in Elicit (Continuous, Linear Scale Questions only for now)

The incredible team at Ought has created an awesome tool called [Elicit](https://elicit.ought.org/) to help update continuous distributions. As part of their offering, they included a bookmarklet, and agreed to let me host a copy here. Thanks [@ought](https://ought.org/)!

<a style="color: red !important;" id="elicit-launcher">Launch in Elicit</a>

<pre><code id="elicit-code" class="language-javascript"></code></pre>

<script>
fetch('https://api.github.com/gists/57b8a798ea37b82f09a4ad940cf9d3d1').then(r => r.json().then(d => {
  let bookmarklet = d.files['elicitMetaculusBookmarklet.js'].content;
  document.getElementById('elicit-launcher').href = 'javascript:' + encodeURIComponent(bookmarklet);
  document.getElementById('elicit-code').textContent = bookmarklet;
}));
</script>

### Download Your Track Record (binary questions only, as csv)

This makes use of [the super-secret official-but-unsupported Metaculus API](https://www.metaculus.com/questions/935/discussion-topic-what-features-should-metaculus-add-may-2018-edition/#comment-6452), which only reports binary questions. If you want your track record across all questions, there's [a bookmarklet for that too](#download-your-track-record-all-questions-as-csv) (but please note that it's in [the danger zone](#danger-zone)!)

<a style="color: red !important;" href="javascript:(function()%7B(()%20%3D%3E%20%7Basync%20function%20loadScript(url)%7Blet%20script%20%20%20%3D%20document.createElement(%22script%22)%3Bscript.type%20%20%3D%20%22text%2Fjavascript%22%3Bawait%20fetch(url).then(r%20%3D%3E%20r.text().then(s%20%3D%3E%20script.innerHTML%20%3D%20s))%3Bdocument.body.appendChild(script)%3B%7DloadScript(%22https%3A%2F%2Fcdnjs.cloudflare.com%2Fajax%2Flibs%2FPapaParse%2F5.1.0%2Fpapaparse.min.js%22).then(()%20%3D%3E%20%7BloadScript(%22https%3A%2F%2Fcdnjs.cloudflare.com%2Fajax%2Flibs%2FFileSaver.js%2F1.3.8%2FFileSaver.min.js%22).then(()%20%3D%3E%20%7Bfetch(%60https%3A%2F%2Fwww.metaculus.com%2Faccounts%2Fprofile%2F%24%7BmetacData.user.id%7D%2Ftrack-record-export%2F%60).then(data1%20%3D%3E%20data1.json().then(predictions%20%3D%3E%20%7BsaveAs(new%20Blob(%5BPapa.unparse(predictions)%5D%2C%20%7Btype%3A%20%22text%2Fcsv%3Bcharset%3Dutf-8%22%7D)%2C%20%22track-record.csv%22)%3B%7D))%3B%7D)%3B%7D)%3B%7D)()%7D)()">Download My Track Record</a>

```javascript
(() => {
  const loadScript = async function(url){
    let script   = document.createElement("script");
    script.type  = "text/javascript";
    await fetch(url).then(r => r.text().then(s => script.innerHTML = s));
    document.body.appendChild(script);
  };

  Promise.all([
    loadScript("https://cdnjs.cloudflare.com/ajax/libs/PapaParse/5.1.0/papaparse.min.js"),
    loadScript("https://cdnjs.cloudflare.com/ajax/libs/FileSaver.js/1.3.8/FileSaver.min.js")
  ]).then(() => {
      fetch(`https://www.metaculus.com/accounts/profile/${metacData.user.id}/track-record-export/`).then(data1 => data1.json().then(predictions => {
        saveAs(new Blob([Papa.unparse(predictions)], {type: "text/csv;charset=utf-8"}), "track-record.csv");
      }));
    });
})();
```

### Download My Predictions From a Question (as CSV)

For binary questions, will work about how you expect. For range questions, it's a little more complex--each prediction includes an array components (one for each peak in a multimodal prediction), normalized to a 0-1 scale (or something close to it). I haven't yet figured out how to translate the component values onto the question's actual range, but I'm pretty sure I've got the values that correspond to the mean, spread, and weight figured out. Those are reported (though not translated into the actual question scale) as `component{i}mean`, `component{i}weight`, and `component{i}spread` (where `{i}` is the component's 0-based ordinal index in the array). (Inspired by [this feature request](https://www.metaculus.com/questions/935/discussion-topic-what-features-should-metaculus-add/#comment-27321).)

<a style="color: red !important;" href="javascript:(function()%7B(()%20%3D%3E%20%7Basync%20function%20loadScript(url)%7Blet%20script%20%20%20%3D%20document.createElement(%22script%22)%3Bscript.type%20%20%3D%20%22text%2Fjavascript%22%3Bawait%20fetch(url).then(r%20%3D%3E%20r.text().then(s%20%3D%3E%20script.innerHTML%20%3D%20s))%3Bdocument.body.appendChild(script)%3B%7DloadScript(%22https%3A%2F%2Fcdnjs.cloudflare.com%2Fajax%2Flibs%2FPapaParse%2F5.1.0%2Fpapaparse.min.js%22).then(()%20%3D%3E%20%7BloadScript(%22https%3A%2F%2Fcdnjs.cloudflare.com%2Fajax%2Flibs%2FFileSaver.js%2F1.3.8%2FFileSaver.min.js%22).then(()%20%3D%3E%20%7Blet%20myPredictions%3Bif(metacData.question.possibilities.type%20%3D%3D%20'binary')%7BmyPredictions%20%3D%20metacData.question.my_predictions.predictions.map(p%20%3D%3E%20(%7Btime%3A%20new%20Date(p.t*1000).toISOString()%2Cprediction%3A%20p.x%7D))%3B%7D%20else%20%7BmyPredictions%20%3D%20metacData.question.my_predictions.predictions.map(p%20%3D%3E%20%7Blet%20prediction%20%3D%20%7B%20time%3A%20new%20Date(p.t*1000).toISOString()%20%7D%3Bp.d.forEach((c%2C%20i)%20%3D%3E%20%7Bprediction%5B%60component%24%7Bi%7Dmean%60%5D%20%3D%20c.x0%3Bprediction%5B%60component%24%7Bi%7Dspread%60%5D%20%3D%20c.s%3Bprediction%5B%60component%24%7Bi%7Dweight%60%5D%20%3D%20c.w%3B%7D)%3Breturn%20prediction%3B%7D)%3B%7DsaveAs(new%20Blob(%5BPapa.unparse(myPredictions)%5D%2C%20%7Btype%3A%20%22text%2Fcsv%3Bcharset%3Dutf-8%22%7D)%2C%20%22my-predictions.csv%22)%3B%7D)%3B%7D)%3B%7D)()%7D)()">Download My Predictions on this Question</a>

```javascript
(() => {
  const loadScript = async function(url){
    let script   = document.createElement("script");
    script.type  = "text/javascript";
    await fetch(url).then(r => r.text().then(s => script.innerHTML = s));
    document.body.appendChild(script);
  };

  Promise.all([
    loadScript("https://cdnjs.cloudflare.com/ajax/libs/PapaParse/5.1.0/papaparse.min.js"),
    loadScript("https://cdnjs.cloudflare.com/ajax/libs/FileSaver.js/1.3.8/FileSaver.min.js")
  ]).then(() => {
      let myPredictions;
      if(metacData.question.possibilities.type == 'binary'){
        myPredictions = metacData.question.my_predictions.predictions.map(p => ({
          time: new Date(p.t*1000).toISOString(),
          prediction: p.x
        }));
      } else {
        myPredictions = metacData.question.my_predictions.predictions.map(p => {
          let prediction = { time: new Date(p.t*1000).toISOString() };
          p.d.forEach((c, i) => {
            prediction[`component${i}mean`] = c.x0;
            prediction[`component${i}spread`] = c.s;
            prediction[`component${i}weight`] = c.w;
          });
          return prediction;
        });
      }
      saveAs(new Blob([Papa.unparse(myPredictions)], {type: "text/csv;charset=utf-8"}), "my-predictions.csv");
    });
})();
```

## For Question Authors

### Show my Pending Questions

Suppose in addition to forecasting, you write a lot of questions. Sometimes, some of those will get stuck in moderation for awhile. In times like these, it would be helpful to be able to set the status filter to "Pending", but that isn't an option. [Sad!](https://knowyourmeme.com/memes/donald-trumps-sad-tweets) Fortunately, we can do this ourselves.

<a style="color: red !important;" href="javascript:(function()%7Bdocument.querySelectorAll('question-table-row').forEach(x%20%3D%3E%20%7Bif(x.innerText.indexOf('Pending')%20%3D%3D%20-1)%20x.remove()%3B%7D)%7D)()">Pending Only, Please!</a>

```javascript
document.querySelectorAll('question-table-row').forEach(x => {
  if(x.innerText.indexOf('Pending') == -1) x.remove();
});
```

## For Data Junkies

### Download the Metaculus Track Record (binary questions only, as csv)

If you want to analyze Metaculus' Track record (again, on Binary questions only), you can get that data (also from [the super-secret official-but-unsupported Metaculus API](https://www.metaculus.com/questions/935/discussion-topic-what-features-should-metaculus-add-may-2018-edition/#comment-6452)) by running this Bookmarklet.

<a style="color: red !important;" href="javascript:(function()%7B(()%20%3D%3E%20%7Basync%20function%20loadScript(url)%7Blet%20script%20%20%20%3D%20document.createElement(%22script%22)%3Bscript.type%20%20%3D%20%22text%2Fjavascript%22%3Bawait%20fetch(url).then(r%20%3D%3E%20r.text().then(s%20%3D%3E%20script.innerHTML%20%3D%20s))%3Bdocument.body.appendChild(script)%3B%7DloadScript(%22https%3A%2F%2Fcdnjs.cloudflare.com%2Fajax%2Flibs%2FPapaParse%2F5.1.0%2Fpapaparse.min.js%22).then(()%20%3D%3E%20%7BloadScript(%22https%3A%2F%2Fcdnjs.cloudflare.com%2Fajax%2Flibs%2FFileSaver.js%2F1.3.8%2FFileSaver.min.js%22).then(()%20%3D%3E%20%7Bfetch(%60https%3A%2F%2Fwww.metaculus.com%2Fquestions%2Ftrack-record%2Fexport%2F%60).then(data1%20%3D%3E%20data1.json().then(predictions%20%3D%3E%20%7BsaveAs(new%20Blob(%5BPapa.unparse(predictions)%5D%2C%20%7Btype%3A%20%22text%2Fcsv%3Bcharset%3Dutf-8%22%7D)%2C%20%22metaculus-track-record.csv%22)%3B%7D))%3B%7D)%3B%7D)%3B%7D)()%7D)()">Download Metaculus Track Record</a>

```javascript
(() => {
  const loadScript = async function(url){
    let script   = document.createElement("script");
    script.type  = "text/javascript";
    await fetch(url).then(r => r.text().then(s => script.innerHTML = s));
    document.body.appendChild(script);
  };

  Promise.all([
    loadScript("https://cdnjs.cloudflare.com/ajax/libs/PapaParse/5.1.0/papaparse.min.js"),
    loadScript("https://cdnjs.cloudflare.com/ajax/libs/FileSaver.js/1.3.8/FileSaver.min.js")
  ]).then(() => {
      fetch(`https://www.metaculus.com/questions/track-record/export/`).then(data1 => data1.json().then(predictions => {
        saveAs(new Blob([Papa.unparse(predictions)], {type: "text/csv;charset=utf-8"}), "metaculus-track-record.csv");
      }));
    });
})();
```

### Download a Question's Prediction Data

Metaculus exposes a time-series of a question's distribution of predictions in order to render the question's line plot. Using this bookmarklet, we can extract the raw data from the question page and export it to CSV. 

<a style="color: red !important;" href="javascript:(function()%7B(()%20%3D%3E%20%7Bconst%20loadScript%20%3D%20async%20function(url)%7Blet%20script%20%20%20%3D%20document.createElement(%22script%22)%3Bscript.type%20%20%3D%20%22text%2Fjavascript%22%3Bawait%20fetch(url).then(r%20%3D%3E%20r.text().then(s%20%3D%3E%20script.innerHTML%20%3D%20s))%3Bdocument.body.appendChild(script)%3B%7D%3BPromise.all(%5BloadScript(%22https%3A%2F%2Fcdnjs.cloudflare.com%2Fajax%2Flibs%2FPapaParse%2F5.1.0%2Fpapaparse.min.js%22)%2CloadScript(%22https%3A%2F%2Fcdnjs.cloudflare.com%2Fajax%2Flibs%2FFileSaver.js%2F1.3.8%2FFileSaver.min.js%22)%5D).then(()%20%3D%3E%20saveAs(new%20Blob(%5BPapa.unparse(metacData.question.community_prediction.history.map(p%20%3D%3E%20Object.assign(p.x1%2C%20%7Bnp%3A%20p.np%2C%20nu%3A%20p.nu%2C%20time%3A%20p.time.toISOString()%7D)))%5D%2C%20%7Btype%3A%20%22text%2Fcsv%3Bcharset%3Dutf-8%22%7D)%2C%20%22predictions.csv%22))%3B%7D)()%7D)()">Download Prediction Time-series</a>

```javascript
(() => {
  const loadScript = async function(url){
    let script   = document.createElement("script");
    script.type  = "text/javascript";
    await fetch(url).then(r => r.text().then(s => script.innerHTML = s));
    document.body.appendChild(script);
  };

  Promise.all([
    loadScript("https://cdnjs.cloudflare.com/ajax/libs/PapaParse/5.1.0/papaparse.min.js"),
    loadScript("https://cdnjs.cloudflare.com/ajax/libs/FileSaver.js/1.3.8/FileSaver.min.js")
  ]).then(() => saveAs(new Blob([Papa.unparse(metacData.question.community_prediction.history.map(p => Object.assign(p.x1, {np: p.np, nu: p.nu, time: p.time.toISOString()})))], {type: "text/csv;charset=utf-8"}), "predictions.csv"));
})();
```

### Download a Community Prediction Plot (as PNG)

The graphs on Metaculus are rendered using SVG, which is awesome for two things: 1. the web and 2. print. Not so good for exporting (e.g. into a document). Luckily it can be converted into PNG without too much trouble. This means we can write a hack to download a PNG of a graph.

Things to be aware of:

* This doesn't bring the full stylesheet, so at present it just ends up looking like a big dark smear across a transparent background.
* The graphs on Metaculus are *tiny*, so this blows it up 10-fold.

<a style="color: red !important;" href="javascript:(function()%7B(()%20%3D%3E%20%7Basync%20function%20loadScript(url)%7Blet%20script%20%20%20%3D%20document.createElement(%22script%22)%3Bscript.type%20%20%3D%20%22text%2Fjavascript%22%3Bawait%20fetch(url).then(r%20%3D%3E%20r.text().then(s%20%3D%3E%20script.innerHTML%20%3D%20s))%3Bdocument.body.appendChild(script)%3B%7DloadScript(%22https%3A%2F%2Funpkg.com%2Fsave-svg-as-png%401.4.17%2Flib%2FsaveSvgAsPng.js%22).then(()%20%3D%3E%20%7BsaveSvgAsPng(document.querySelectorAll(%22svg.metac-graph%22)%5B0%5D%2C%20%22diagram.png%22%2C%20%7Bscale%3A%2010%7D)%3B%7D)%3B%7D)()%7D)()">Export Graph</a>

```javascript
(() => {
  const loadScript = async function(url){
    let script   = document.createElement("script");
    script.type  = "text/javascript";
    await fetch(url).then(r => r.text().then(s => script.innerHTML = s));
    document.body.appendChild(script);
  };

  loadScript("https://unpkg.com/save-svg-as-png@1.4.17/lib/saveSvgAsPng.js").then(() => {
    saveSvgAsPng(document.querySelectorAll("svg.metac-graph")[0], "diagram.png", {scale: 10});
  });
})();
```

## Danger Zone

**Please don't abuse bookmarklets below this. They rely on repeated calls to the Metaculus API, meaning that hammering them could hurt the Metaculus Server.**

### Disclaimer Addendum

* You assume responsibility for the consequences of using these. If you somehow get in trouble for using these, feel free to explain to the Metaculus admins that these were involved, but **the fault is not 100% mine.** You have been amply warned, you know the risks.

### Load All Questions

Ever get annoyed that you have to click "Load More" a bunch to browse down to older questions? Just click this bookmarklet to automatically click it until all questions are loaded. Loads a maximum of 300 questions per second (plus additional latency), so it won't happen immediately, but it's still faster than clicking it yourself.

<a style="color: red !important;" href="javascript:(function()%7B(()%20%3D%3E%20setInterval(()%20%3D%3E%20document.querySelector('._load-more').click()%2C%20100)%7D)()%7D)()">Load All Questions</a>

```javascript
(() => setInterval(() => document.querySelector('._load-more').click(), 100)})();
```

### Download Your Track Record (All questions, as csv)

Same idea [as above](#download-your-track-record-binary-questions-only-as-csv), only this one side-steps the API to get all question types, not just binary questions. Makes a bunch of fetch calls, so it can take a minute. (Inspired by [this feature request](https://www.metaculus.com/questions/935/discussion-topic-what-features-should-metaculus-add-may-2018-edition/#comment-20248).)

<a style="color: red !important;" href="javascript:(function()%7B(()%20%3D%3E%20%7Basync%20function%20loadScript(url)%7Blet%20script%20%20%20%3D%20document.createElement(%22script%22)%3Bscript.type%20%20%3D%20%22text%2Fjavascript%22%3Bawait%20fetch(url).then(r%20%3D%3E%20r.text().then(s%20%3D%3E%20script.innerHTML%20%3D%20s))%3Bdocument.body.appendChild(script)%3B%7DloadScript(%22https%3A%2F%2Fcdnjs.cloudflare.com%2Fajax%2Flibs%2FPapaParse%2F5.1.0%2Fpapaparse.min.js%22).then(()%20%3D%3E%20%7BloadScript(%22https%3A%2F%2Fcdnjs.cloudflare.com%2Fajax%2Flibs%2FFileSaver.js%2F1.3.8%2FFileSaver.min.js%22).then(()%20%3D%3E%20%7Bfetch(%60https%3A%2F%2Fwww.metaculus.com%2Fapi2%2Fquestions%2F%3Fguessed_by%3D%24%7BmetacData.user.id%7D%26order_by%3D-activity%26page%3D1%60).then(data1%20%3D%3E%20data1.json().then(predictions%20%3D%3E%20%7Blet%20n%20%3D%20Math.ceil(predictions.count%2F20)%3Blet%20requests%20%3D%20%5B%5D%3Bfor(let%20i%20%3D%202%3B%20i%20%3C%3D%20n%3B%20i%2B%2B)%7Brequests.push(fetch(%60https%3A%2F%2Fwww.metaculus.com%2Fapi2%2Fquestions%2F%3Fguessed_by%3D%24%7BmetacData.user.id%7D%26order_by%3D-activity%26page%3D%24%7Bi%7D%60).then(data2%20%3D%3E%20data2.json().then(page%20%3D%3E%20%7Bpredictions.results%20%3D%20predictions.results.concat(page.results)%3B%7D)))%3B%7DPromise.all(requests).then(()%20%3D%3E%20saveAs(new%20Blob(%5BPapa.unparse(predictions.results)%5D%2C%20%7Btype%3A%20%22text%2Fcsv%3Bcharset%3Dutf-8%22%7D)%2C%20%22track-record.csv%22))%3B%7D))%3B%7D)%3B%7D)%3B%7D)()%7D)()">Download My Full Track Record</a>

```javascript
(() => {
  const loadScript = async function(url){
    let script   = document.createElement("script");
    script.type  = "text/javascript";
    await fetch(url).then(r => r.text().then(s => script.innerHTML = s));
    document.body.appendChild(script);
  };

  Promise.all([
    loadScript("https://cdnjs.cloudflare.com/ajax/libs/PapaParse/5.1.0/papaparse.min.js"),
    loadScript("https://cdnjs.cloudflare.com/ajax/libs/FileSaver.js/1.3.8/FileSaver.min.js")
  ]).then(() => {
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
})();
```

### Download all my predictions (as JSON)

Runs from your user profile page. Queries the metaculus server for every question page on which you've ever predicted. It will take a few seconds (at least) to assemble all your data. DO NOT ABUSE IT. Run it once and save the output. This one could seriously kill the server. (Inspired by [this feature request](https://www.metaculus.com/questions/935/discussion-topic-what-features-should-metaculus-add/#comment-27329).)

<a style="color: red !important;" href="javascript:(function()%7B(()%20%3D%3E%20%7Basync%20function%20loadScript(url)%7Blet%20script%20%20%20%3D%20document.createElement(%22script%22)%3Bscript.type%20%20%3D%20%22text%2Fjavascript%22%3Bawait%20fetch(url).then(r%20%3D%3E%20r.text().then(s%20%3D%3E%20script.innerHTML%20%3D%20s))%3Bdocument.body.appendChild(script)%3B%7DloadScript(%22https%3A%2F%2Fcdnjs.cloudflare.com%2Fajax%2Flibs%2FFileSaver.js%2F1.3.8%2FFileSaver.min.js%22).then(()%20%3D%3E%20%7BallRequests%20%3D%20%5B%5D%3BallQuestions%20%3D%20%5B%5D%3Bfor(let%20i%20%3D%200%3B%20i%20%3C%20metacData.trackRecord.length%3B%20i%2B%2B)%7BallRequests.push(fetch(metacData.trackRecord%5Bi%5D.url).then(r%20%3D%3E%20r.text().then(t%20%3D%3E%20%7BallQuestions.push(JSON.parse(%2Fwindow.metacData.question%20%3D%20(.*)%3B%2F.exec(t)%5B1%5D))%3B%7D)))%3B%7DPromise.all(allRequests).then(()%20%3D%3E%20saveAs(new%20Blob(%5BJSON.stringify(allQuestions)%5D%2C%20%7Btype%3A%20%22application%2Fjson%3Bcharset%3Dutf-8%22%7D)%2C%20%22predictions.json%22))%3B%7D)%3B%7D)()%7D)()">Make me a big JSON of every prediction I've ever made</a>

```javascript
(() => {
  const loadScript = async function(url){
    let script   = document.createElement("script");
    script.type  = "text/javascript";
    await fetch(url).then(r => r.text().then(s => script.innerHTML = s));
    document.body.appendChild(script);
  };

  loadScript("https://cdnjs.cloudflare.com/ajax/libs/FileSaver.js/1.3.8/FileSaver.min.js").then(() => {
    let allRequests = [];
    let allQuestions = [];
    for(let i = 0; i < metacData.trackRecord.length; i++){
      allRequests.push(fetch(metacData.trackRecord[i].url).then(r => r.text().then(t => {
        allQuestions.push(JSON.parse(/window.metacData.question = (.*);/.exec(t)[1]));
      })));
    }
    Promise.all(allRequests).then(() => saveAs(new Blob([JSON.stringify(allQuestions)], {type: "application/json;charset=utf-8"}), "predictions.json"));
  });
})();
```

## Suggest a Hack

Think I might be able to make Metaculus work a little better for you? [Suggest a new hack](https://github.com/AABoyles/Metaculus-Hacks/issues/new/choose) and I'll look into implementing it!

<link rel="stylesheet" href="https://unpkg.com/prismjs@1.20.0/themes/prism-tomorrow.css">
<script src="https://unpkg.com/prismjs@1.20.0/components/prism-core.min.js"></script>
<script src="https://unpkg.com/prismjs@1.20.0/plugins/autoloader/prism-autoloader.min.js"></script>
