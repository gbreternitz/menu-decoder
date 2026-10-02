# Menu Decoder

Scan a restaurant menu with your phone. Each dish gets its own card with its ingredients as tappable chips, and the ones you might not know are highlighted. Tap any chip for a short description and photo from Wikipedia.

Everything runs in the browser. There's no server, no account system, and nothing to pay for.

## How scanning works

1. **Clean-up.** The app evens out shadows and uneven lighting, straightens a tilted photo, flips light-on-dark menus to dark-on-light, and removes things that aren't text, such as a book's spine, drawings, and paper edges.
2. **Boxes.** The app draws a box around each block of text, which is usually one dish (its name, price, and description) or a section heading. On the **Check the boxes** screen, tap a box to select it, then drag it to move it or pull a corner to resize it. Use **Remove box** for anything that isn't menu text and **Add a box** for anything that was missed. The **+** and **−** buttons zoom in for precise edits. The reading order is worked out from where the boxes sit, so you don't need to worry about it.
3. **Reading.** Each box is enlarged and read on its own, which is much more accurate than reading the whole page at once.
4. **Word fixing.** Words the reader wasn't sure about are compared against a vocabulary of food and menu words (plus every term in the glossary). If one is a close match, allowing for common misreads like "rn" for "m" or "0" for "o", it's swapped in, so "darkon" becomes "daikon". Common English words are never changed, and unsure fragments that match nothing are dropped.
5. **Dishes.** The app sorts the text into sections, dish names, prices, ingredients, and add-ons ("add egg +3"), and shows a card for each dish.

If the scan misreads something, tap **Fix the text** under the results, correct it, and decode it again. When typing or pasting a menu, put each dish name on its own line with its ingredients on the line below.

## What's in this folder

- `index.html` is the whole app.
- `glossary.csv` is the list of terms the app highlights. Edit it to add or fix terms.
- `README.md` is this file.

## Try it on your computer first

Double-click `index.html` to open it in your browser. You can paste menu text, tap ingredients, and try photos.

One thing to know: when the page is opened straight from your computer, browsers won't let it read `glossary.csv`, so it uses a built-in copy of the starter glossary instead. Your edits to the CSV will show up once the site is online.

## Put it online with GitHub Pages

You only do this once. It takes about ten minutes.

1. Go to [github.com](https://github.com) and create a free account.
2. Click the **+** in the top-right corner and choose **New repository**.
3. Name it `menu-decoder`, leave it set to **Public**, and click **Create repository**. (Free GitHub Pages sites need a public repository. That's fine here, because nothing in it is private.)
4. On the next page, click the link that says **uploading an existing file**.
5. Drag `index.html`, `glossary.csv`, and `README.md` onto the page, then click **Commit changes**.
6. Click **Settings** at the top of the repository, then **Pages** in the left sidebar.
7. Under **Build and deployment**, set **Source** to **Deploy from a branch**. Set the branch to **main** and the folder to **/ (root)**, then click **Save**.
8. Wait a minute or two and refresh the page. A message appears with your site's address, which looks like `https://YOUR-USERNAME.github.io/menu-decoder/`.

Open that address on your phone to use the app.

## Updating the app or glossary

1. Open your repository on github.com.
2. Click **Add file**, then **Upload files**.
3. Drag in the changed file. A file with the same name replaces the old one.
4. Click **Commit changes**.

Changes usually appear on the site within a couple of minutes. If you don't see them, refresh the page.

You can also edit `glossary.csv` right on GitHub. Click the file, click the pencil icon, make your change, and click **Commit changes**.

Every upload is saved as a version. If something breaks, click **History** on a file to see earlier versions.

## Editing the glossary

`glossary.csv` opens in Excel, Numbers, or Google Sheets. It has four columns:

| Column | What it's for | Example |
| --- | --- | --- |
| `term` | The word or phrase as it appears on menus | `yuzu kosho` |
| `wikipedia` | The exact Wikipedia article title. Leave it blank if the term itself is the right title. | `Yuzukoshō` |
| `aliases` | Other spellings, separated by semicolons | `yuzukosho;yuzu koshō` |
| `cuisine` | Optional label shown on the card | `Japanese` |

A few tips:

- Phrases work, such as `black garlic` or `fish sauce`. Terms are highlighted both in dish names and in ingredient chips.
- Capitals and accents don't matter for matching, and simple plurals like `morels` are matched automatically.
- To find the right Wikipedia title, open the article and copy the title from the top of the page. Use the `wikipedia` column for words with several meanings. For example, `mole` points to `Mole (sauce)` so it doesn't show the animal.
- When saving from a spreadsheet app, choose **CSV UTF-8** so accented letters survive.

If a highlighted term shows the wrong article, set its `wikipedia` column to the right title.

## Add it to your home screen

- **iPhone (Safari):** tap the Share button, then **Add to Home Screen**.
- **Android (Chrome):** tap the menu, then **Add to Home screen**.

## Privacy

Menu photos are read on the phone itself and never uploaded. The text reader downloads once, about 10 to 15 MB, the first time someone scans. Words you look up are sent to Wikipedia to fetch descriptions and photos. The "Terms I know" list is saved only in each person's own browser.

## Later: usage stats

The app already sends every lookup through one function, `recordLookup`, which does nothing until you turn it on. To log lookups to a Google Sheet:

1. Create a Google Sheet, then click **Extensions** and choose **Apps Script**.
2. Replace the code with the following, and save:

   ```javascript
   function doPost(e) {
     const data = JSON.parse(e.postData.contents);
     SpreadsheetApp.getActiveSpreadsheet().getSheets()[0]
       .appendRow([data.at, data.term, data.inGlossary, data.matched, data.source]);
     return ContentService.createTextOutput('ok');
   }
   ```

3. Click **Deploy**, then **New deployment**. Choose **Web app**, set **Who has access** to **Anyone**, and click **Deploy**. Copy the web app URL.
4. In `index.html`, find `statsEndpoint: ''` near the top of the script and paste the URL between the quotes. Then upload the updated file.

Each lookup then becomes a row with the time, the term, whether it was in the glossary, which Wikipedia article matched, and how it was looked up. Terms marked as not in the glossary make a ready-made list of terms to add. If you turn this on, let your friends know that lookups are logged.

## Credits

Descriptions and photos come from [Wikipedia](https://www.wikipedia.org) (text under CC BY-SA; photo licenses vary). Text recognition uses [Tesseract.js](https://github.com/naptha/tesseract.js). The English word list used for word fixing comes from [SCOWL](http://wordlist.aspell.net/), Copyright 2000-2016 by Kevin Atkinson, used under its permissive license.
