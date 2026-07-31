# Arun & Saritha — Wedding Invitation · Setup Guide

Your invitation is the single file **`index.html`**. Double-click it to open in any browser, or host it (Netlify, GitHub Pages, or the same host as the reference sites) to share a link with guests.

Everything works out of the box **except** three optional things you may want to personalise: the RSVP-to-Excel connection, your photos, and background music. Steps below.

---

## 1. RSVP → Google Sheet (opens in Excel)

The RSVP form is ready. To make responses land in a Google Sheet you can open/export as Excel, connect it to a free Google Apps Script (about 2 minutes, no coding):

1. Go to **sheets.google.com** and create a new blank sheet. Name it e.g. *Arun & Saritha RSVP*.
2. In the sheet menu: **Extensions → Apps Script**. Delete any code shown and paste this:

```javascript
function doPost(e) {
  var sheet = SpreadsheetApp.getActiveSpreadsheet().getSheets()[0];
  if (sheet.getLastRow() === 0) {
    sheet.appendRow(['Timestamp','Name','Phone','Guests','Attending','Event','Message']);
  }
  var d = JSON.parse(e.postData.contents);
  sheet.appendRow([d.timestamp, d.name, d.phone, d.guests, d.attending, d.event, d.message]);
  return ContentService.createTextOutput('OK');
}
```

3. Click **Deploy → New deployment**. For type choose **Web app**.
   - *Execute as:* **Me**
   - *Who has access:* **Anyone**
   - Click **Deploy**, authorise when prompted, and **copy the Web app URL** (ends in `/exec`).
4. Open **`index.html`** in a text editor. Near the bottom find this line:

```javascript
const RSVP_ENDPOINT = "";
```

   Paste your URL between the quotes:

```javascript
const RSVP_ENDPOINT = "https://script.google.com/macros/s/AKfy....../exec";
```

5. Save. Done — every RSVP now appears as a new row in your sheet. In Google Sheets use **File → Download → Microsoft Excel (.xlsx)** anytime.

> Until you add the URL, RSVPs are saved safely in the guest's own browser as a fallback and the form still shows a thank-you message.

---

## 2. Photos — already added ✓

Your photos are wired in:

- **Groom & Bride portraits** use `images/web/groom.jpg` and `images/web/bride.jpg`.
- **Gallery** shows all 25 of your photos (from the `images` folder) with a tap-to-enlarge lightbox, plus the **View Full Album** button linking to your Google Photos: `https://photos.app.goo.gl/DcrpwVftKNWugptaA`
- The **welcome reveal** and hero use a cover photo (`images/web/cover.jpg`).

Web-optimised copies live in **`images/web/`** (resized so the page loads fast on phones); your full-size originals stay untouched in `images/`.

To add or swap photos: drop a new image into `images/web/` and add its filename to the `PHOTOS` list near the bottom of `index.html`. To change the cover/portrait photos, just replace `images/web/cover.jpg`, `groom.jpg`, or `bride.jpg`.

---

## 3. Background music — already added ✓

Your uploaded track is set as **`assets/music.mp3`** and starts when a guest taps **Open Invitation**. The gold button at the bottom-right pauses/plays it anytime. To change the song, replace that file.

*(Browsers block auto-play until the guest interacts, which is why music begins on the opening tap.)*

---

## Quick reference — what's already filled in

| Item | Detail |
|------|--------|
| Couple | Arun S weds Saritha Mani |
| Groom | Arun, s/o Satheesan P.D. & Ashalatha V.K. — "Arun Nivas", Puthiya Road, Eroor |
| Bride | Saritha, d/o T. R. Mani & Vilasini Mani — Thekkeveettil House, Thekkinethu Nirappu, Chottanikkara |
| Wedding | Friday, 4 September 2026 · **Chingam 19, 1202** · Muhurtham 12:00–12:21 PM · Chottanikkara Devi Temple |
| Lunch | Chottanikkara NSS Auditorium |
| Reception | Sunday, 6 September 2026 · **Chingam 21, 1202** · 11:30 AM–1:30 PM · Thykoodam Parish Hall |
| Countdowns | Live to both the wedding and the reception |
| Maps | Embedded for temple & reception, plus "Open in Google Maps" buttons |
| Album | Linked to your Google Photos |

To change any wording, open `index.html` in a text editor and edit the text — it's all plain HTML.
