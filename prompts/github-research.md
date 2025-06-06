# Guidance for synthesizing information about open source projects on GitHub.

Then, collect and synthesize information from github in a table ready for CSV export, with the following columns:

**Name:** Name of the project

**Tags:** Comma-separated list of 1-5 tags that could be used to classify or categorize this github project, based on project's primary fields of technology and research. For instance, "Database Systems" or "Runtimes" or "ML Compilers" or "Front-end Frameworks" or "Front-end Libraries" or "Physics Simulation Engine" or "Physical AI" and so on.

**Description:** Concise description of what this project is about, what exactly it is and does. Base this on the repo's About description and the README.

**Top Contributes:** Comma-separated list of the top human (non-automated) contributors to this project (up to 5)

**Sponsors:** Organizations that sponsor this project, commercial or academic (up to 5)

**Stars:** How many github stars this project has, formatted as an integer (for sorting within a spreadsheet)

**Languages:** The primary programming langauge(s) used to implement this project.

**GitHub:** GitHub profile URL, empty if none found

**Homepage:** Personal or academic homepage URL, empty if none found

**Docs**: Link to docs, e.g. https://dart.readthedocs.io/en/latest/, empty if none found

**Created:** the date on which this repo was created -- use the GitHub API https://api.github.com/repos/{:owner}/{:repository} and look for the "created_at" field in the response to find the repo creation date
