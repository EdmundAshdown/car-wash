# Car Wash — membership manager

A single-file, white-label web app for running a car wash's membership club:
check members in by name, phone or plate, manage plans and status, and export
the book.

No build step, no dependencies, no server. Open `index.html` directly, or serve
the folder with any static server.

## White-labelling

Every shop-specific string lives in one `BRAND` object, declared near the top of
`index.html`:

| Field | Controls |
| --- | --- |
| `name` | The shop name shown at the top of every page |
| `fullName` | The browser tab title |
| `dbName` | The IndexedDB database name — **changing this starts a fresh, empty database** |
| `exportPrefix` | The prefix of exported backup filenames |
| `plateRegion` | Optional line on the plate chip, e.g. `'OREGON'`. Blank (the default) hides the block |
| `plateMotto` | Optional second line under it, e.g. `'The Beaver State'` |

Reskinning for a different customer means editing those values and pushing.
Nothing else in the file names a shop, a state or a region.

## Data

Members, check-ins and membership plans are stored in the browser's IndexedDB,
per origin. The first run seeds four synthetic demo members — fictional names,
`555` phone numbers and `example.com` addresses — so the screens are not empty.

Membership plans are records rather than constants, managed on the **Plans**
page. Each carries a name, a description of what it includes and a monthly
price. The Add Member form only ever offers plans that exist there, so the two
cannot drift apart, and a plan still in use cannot be deleted.

Members are created and edited on the **Add** page. The same form serves both:
opening a member and choosing *Edit* fills it in and switches the button to
*Save Changes*, so there is one place a member is written and one set of
validations. A member's id and join date are preserved across an edit, which is
what keeps their check-in history attached.

From a member's detail view they can also be deactivated or deleted. Deleting a
member deletes their check-in history with them, so the Dashboard's totals stay
honest.

The Dashboard exports the whole book — members, check-ins and plans — as JSON,
and can restore it. Restoring an older backup that has no plans falls back to
the defaults and adopts whatever the restored members are on.

## Live

GitHub Pages serves the repo root from `main`:
<https://edmundashdown.github.io/car-wash/>
