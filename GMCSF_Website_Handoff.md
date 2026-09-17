# GMCSF website — build handoff

Everything needed to build greenmeadowsf.org. Written September 2026 for Tom
Brown, President, Greenmeadow Community Scholarship Fund.

## Why this exists and when it ships

A printed direct-mail appeal goes to roughly 300 households between mid-October
and mid-November 2026. The mailer carries a QR code pointing at
`greenmeadowsf.org/give`. **That redirect has to resolve before the piece goes
to the printer in early October.** Everything else can follow.

The fund currently has no web presence of its own. Its only online footprint is
a section inside the community association's site at
`greenmeadow.org/community/#scholarship`, which nobody at GMCSF controls.

## Audiences, in priority order

1. **Arrived from the mailer with a phone.** Scanned the QR, wants to give in
   under a minute. This is the only audience that matters before November.
2. **Heard about the fund from a neighbor or the mailer.** Needs to understand
   the program and believe it's real before giving. Includes new residents and
   long-time neighbors who never knew this existed.
3. **A Greenmeadow senior or their parent.** Needs eligibility, award amounts,
   what to submit, and dates. Seasonal: matters January through March.
4. **Someone who wants to get involved.** Two sentences and an email address.

Menlo-Atherton recruiting runs through the school's College and Career Center,
so the site does not need to serve M-A applicants.

**Phase one is audiences 1, 2 and 4 on a single page. Phase two is the apply
page for audience 3, needed by January.**

## Technical shape

Plain static HTML and CSS. No framework, no build step, no dependencies.

This is deliberate. The site will be maintained by rotating volunteers, and in
five years somebody needs to be able to open a file and change a date. A build
pipeline nobody remembers how to run is a liability, not an asset.

- Repo on GitHub, deployed on Vercel
- Two files to start: `index.html` and `styles.css`
- `vercel.json` for the redirect
- Domain DNS stays on DreamHost nameservers because live email depends on it
  (MX to mailchannels.net, plus SPF, DKIM, autoconfig, autodiscover). Only the
  apex A record and the `www` record change.

### vercel.json

```json
{
  "redirects": [
    { "source": "/give", "destination": "https://www.paypal.com/fundraiser/charity/1591128", "permanent": false }
  ]
}
```

`permanent: false` is intentional. A 302 keeps the destination changeable
forever. A printed QR code is permanent; the page behind it must not be.

**Open decision before this ships:** there are two live PayPal paths. The
hosted button (`paypal.com/donate/?hosted_button_id=432CMSML8WSC6`) charges
roughly 2.2% plus 30 cents. The PayPal Giving Fund charity page
(`paypal.com/fundraiser/charity/1591128`) charges nothing but disburses
monthly. On $13,000 the difference is about $300, which is most of the mailing
budget. Steve Fung, Treasurer, decides. The config above assumes Giving Fund.

## Design

Match the printed mailer. Brand green sampled from the logo.

```css
:root{
  --green:  #4CAF5F;   /* brand, primary */
  --forest: #1F5730;   /* dark bands, headings */
  --ink:    #1A2119;   /* body text */
  --paper:  #F3F2EA;   /* background */
}
```

Type: Jost for display and labels, Source Serif 4 for body. Both on Google
Fonts. Give each a real fallback stack.

Visual direction is mid-century modern, which is Greenmeadow's own vernacular
as a Joseph Eichler tract on the National Register. Flat color bands, hard
edges, geometric display type, generous white space. No rounded corners, no
drop shadows, no gradients, no stock photography.

Assets needed: the green GREENMEADOW wordmark (get the vector original from
whoever produced the Awards Reception invitation) and the 2026 finalists group
photo from Greenmeadow Park.

---

# Home page copy

## Hero

**Every June since 1964, this neighborhood has bet on somebody's kid.**

The Greenmeadow Community Scholarship Fund awards cash scholarships and
reference books to graduating seniors from Greenmeadow and from Menlo-Atherton
across town. It is run entirely by volunteers who live on these streets.

[**Donate**] — primary button, above the fold, unmissable on mobile.

Beneath, small: A 501(c)(3) nonprofit. Tax ID 23-7037165. Your gift may be tax
deductible as allowed by law.

## What a scholarship is

Each year the board selects finalists from Greenmeadow and from
Menlo-Atherton, interviews every one of them, and awards scholarships. Finalists
who do not receive a scholarship receive a textbook stipend instead. And every
finalist, win or not, chooses a gift book of their own.

Last spring fifteen finalists came through our two committees. Six received
scholarships. All fifteen went home with something.

*No dollar figures anywhere on this site. The number and size of awards is set
by the board each January and changes year to year. Publishing an amount would
either go stale or commit the board to something it has not voted on.*

## How finalists are chosen

Volunteers read every application, then sit down with every finalist for a real
interview. Four things matter:

- **Academic performance**, in the context of what a student was handed.
- **Community involvement**, which usually means the unpaid, unglamorous kind.
- **Work experience**, including the jobs taken to help a family stay afloat.
- **Worthy character.** We weigh the interview most heavily of all.

## Three from the class of 2026

*Condensed from the 2026 finalist write-ups. Roughly 150 words each.*

**Jack Spitzer** is headed to Berkeley or Dartmouth to study chemistry, a path
shaped by his grandfather's death from pancreatic cancer and his family's
discovery that they carry the BRCA-1 mutation. When his drone crashed into the
Pacific off Santa Cruz, he taught himself to solder, program in Python, and
wire hardware, built a better one, and founded the Drone Club at school. After
breaking his hand playing water polo, he watched from the bench, noticed how
error-prone manual stat-tracking was, and built a voice-input stats app now
used by more than thirty high schools and clubs across California. He is a math
TA, an Eagle Scout mentor, and a Greenmeadow pool lifeguard who teaches Splash
Ball to sixty kids. The accomplishment he is proudest of is planning,
budgeting, and refurbishing the fire pits in Greenmeadow Park.

**Katherine Vera Yarleque** arrived at Menlo-Atherton from Peru as a sophomore,
stepping into a new country, a new school, and a new language at once. She
found her footing through a summer bridge program, then built a strong academic
record entirely in her second language while adapting to a new educational
system. She joined a club where English and Spanish speakers practice both
languages together, and played volleyball, drawing on her own experience of
feeling new to encourage struggling teammates. Through one program she helped
inform immigrant parents about citizenship resources; through another, connected
to Stanford, she sat in sessions with attorneys working to reach young
immigrants. One of her lessons from that work was recognizing the privilege she
carries relative to others in similar situations. She plans to study
criminology with a focus on immigration advocacy.

**Lukas Chen** is studying mechanical engineering at Purdue, which edged out
Berkeley partly because it sits an hour from the Indianapolis Motor Speedway.
He is proudest of assembling a go-kart start to finish, solving problems as
they came, including relocating a part so the seat would fit. He started
playing AYSO soccer in second grade and wanted others to have that. Because his
mother spoke only Spanish to him growing up, he was positioned to help
Spanish-speaking families navigate AYSO's scholarship applications so they
would not miss out. After hearing parents lament how fast kids outgrow cleats
and shin guards, he launched a community gear exchange with monthly collection
days, catalogued nearly three crates of lightly used equipment, and outfitted
more than twenty families on his first distribution day.

## How this started

In 1964 this neighborhood began awarding a scholarship to a graduating senior,
in memory of a high school senior from these streets who had died the year
before. It has happened every June since.

Five years later the founders decided the circle should not stop at the edge of
the tract, and added scholarships for seniors at Ravenswood High School in East
Palo Alto. When Ravenswood closed in 1976 the program moved to Menlo-Atherton,
where it has stayed. That award is now the George Ebey Scholarship, named for a
founder who guided this program for decades.

*The scholarship fund has operated continuously since 1964. Every dollar comes
from this community.*

## Ways to give

**Online** — [Donate button]

**By check** — Payable to Greenmeadow Community Scholarship Fund, Inc.
Mail to: GMCSF, Inc., c/o Tom Brown, 410 Adobe Place, Palo Alto, CA 94306

**Donor-advised fund** — Search "Greenmeadow" in your DAF account.

**Corporate matching** — Many local employers match through Benevity or
YourCause. A match doubles your gift at no cost to you.

**IRA distribution** — At 70½ or older you can give directly from an IRA, tax
free. At 73 and older it also counts toward your required minimum distribution.

**Appreciated stock** — May earn a deduction at full market value with no
capital gains tax. Contact Steve Fung, Treasurer.

## Who runs this

A volunteer board of directors with roots in the Greenmeadow community. Nobody
is paid. Close to a hundred cents of every dollar reaches a student.

Maria Brown · Phyllis Brown · Craig Chalmers · Willa Chalmers, Secretary ·
Steve Fung, Treasurer · Stu Greene · Chris Hetterley · Stu MacPherson ·
Moe McNally · Carmen Rodwell · Tom Brown, President

## Get involved

The board looks for neighbors willing to read applications, interview
finalists, and help run the April reception. It is a few evenings between
January and April. If that sounds like something you would enjoy, get in touch.

---

# Apply page copy — phase two, needed by January

Not required for the October launch. Build the page, leave it unlinked or
marked as opening in February.

**Eligibility.** Graduating high school seniors who live in Greenmeadow or
whose families belong to the Greenmeadow Community Association, pool, or swim
team.

**Awards.** Up to eight finalists are selected for interviews. Some receive a
scholarship; the rest receive a textbook stipend. Every finalist chooses a gift
book of their own. The number and size of awards is set by the board each
January and is included in the application sent to seniors in February.

**What to submit.** High school transcript. A list of school and community
service and paid work during high school. One essay from your college
applications, whichever best reveals you as a person. And a brief paragraph on
what you have learned or how you have changed through your activities or
employment.

Applications from college materials are welcome and encouraged. There is no
separate essay to write.

**Dates.** Applications open in February. The deadline is early March.
Interviews follow in late March, and the Awards Reception is a Sunday
afternoon in April in Greenmeadow Park. Exact dates are set each January.

**Where to send it.** [email address — see open items]

---

# Open items

- [ ] **Which PayPal destination.** Steve Fung decides. Affects `vercel.json`.
- [ ] **Vector logo.** The raster version will not enlarge cleanly.
- [ ] **Finalist photo.** The 2026 group shot, with family permission.
- [ ] **Menlo-Atherton award amounts.** Not documented anywhere we have. Not
      needed for the site, but the board should have them written down.
- [ ] **Role-based email addresses.** Applications currently go to a personal
      Gmail, and the President's contact is also personal. There is live email
      on greenmeadowsf.org already. Set up `apply@` and `info@` forwarders so
      the printed application, the website, and the mailer point at addresses
      that survive board turnover.
- [ ] **The year count.** The community association calls the April 2026 event
      the 62nd annual reception, and a 2014 newspaper article called that year
      the 50th. Counting award years in the fund's own records gives 63 for
      2026. Both published numbers look like subtraction from 1964 rather than
      a count. Confirm with Stu Greene before publishing any ordinal. The copy
      above avoids the number entirely.
- [ ] **George Ebey renaming year.** Stu Greene is the best source.
- [ ] **California Secretary of State filing is stale.** It still lists Stu
      Greene as CEO and Mike Blanchette as Secretary and registered agent. Mike
      is leaving the board. A Statement of Information needs filing regardless
      of the website.

# One thing that is not a website problem

The Greenmeadow application promises up to three scholarships at $3,500 plus
$500 stipends to remaining finalists. Six scholarships were awarded in 2026
across both committees, against $12,968 raised in the appeal. Even allowing for
different M-A amounts, disbursements appear to exceed donations by a wide
margin, funded by investment income or the reserve.

In 2014 the award was $2,500. It has grown roughly 40% while donations fell
22%, and more awards are being made than five years ago.

That is a Treasurer conversation, and it may matter more than any of the above.
