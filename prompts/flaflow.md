# FlaFlow research wishlist

Here's a roughly prioritized list of the kinds of research I'd love to automate with FF:

(what is the "deep research” capability" ?)

### People

Research and summarize key information about people of interest, enriching whatever information we may already have.

1. LinkedIn Recruiter search saved page
   - would a PDF download work as well as a full HTML page download?

1. CSV lists of candidate
   - Future Founder [list](https://docs.google.com/spreadsheets/d/1GHU7lIb8Ty7jMYYLSbOw8vHdq0FzGxqp_n61ArlYLAM/edit?gid=0#gid=0)
   - would be nice if I could just provide a link to a google sheet

1. LinkedIn post - everyone mentioned, everyone who liked/reacted, everyone who commented
   - can just send a link or need to download the HTML?
   - e.g. MIT entrepreneurship contest [finalists](https://www.linkedin.com/posts/mit-100k-entrepreneurship-competition_mit-100k-entrepreneurship-competition-activity-7322818669871300608-fxFi?utm_source=share&utm_medium=member_desktop&rcm=ACoAAAAAI2EBBm5-5UmgY4ckMPzEh-e8N-7UNxc)

### Companies & Organizations

Research and summarize key information about organizations (mostly companies) of interest, enriching whatever information we may already have. Then, create a list of key people associated with each organization -- founders, executives, angels, board members, key / notable / famous employees + advisors.

1. All companies mentioned in a PDF
   - daily tech newsletter from The Information
   - pages from our Notion technology ecosystem

1. CSV lists of companies

1. Arxiv / BioArxiv / Google Scholar Link to individual paper, like the [Goedel-Prover](https://arxiv.org/abs/2502.07640) paper
   - start with arxiv links only if that’s easier

2. CSV list of research papers, partially filled out
   - [Bio research papers](https://docs.google.com/spreadsheets/d/1o99HjaduCeh_WdWQ2MWZhBtoDFRB8E0q1z1jVhXz8HQ/edit?gid=1249854002#gid=1249854002) for Chai / Axiom / Achira

### Publications

Start with Goedel-Prover paper...

### GitHub Projects

Research and summarize key information about github repos of interest, enriching whatever information we may already have. Then, create a list of top contributors, with [>= 10] contributions.

1. Link to a github repo

2. CSV lists of repos
   - all these [data viz](https://github.com/hal9ai/awesome-dataviz) projects ➝ find all the creators + top contributors

### Labs & Research Institutes


### Events & Conferences


### Universities

`

### List Management

- can FF help me "merge" lists, consolidating duplicate entries without losing information? e.g. I want to keep adding to a master list of candidates without potentially adding duplicate rows for the same person as we add new research output.



### Entity Resolution Capabilities

- look up candidates via LinkedIn url in hireez to see if we can find more info (links, emails)
- add an extra “entity resolution” check — look at profile pictures
- enhance how we find more profile matches — using their handle/username
- try to find someone's last name if they only show their last initial on LinkedI (e.g. find them mentioned in the context of their lab or high school) and then do a web search
- try to find a github profile match via the user search within github (https://github.com/search?type=users&q=%s) rather than via google search