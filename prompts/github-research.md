# Guidance for synthesizing information about open source projects on GitHub.

Then, collect and synthesize information from github in a table ready for CSV export, with the following columns:

**Category:** Come up with a reasonable category or bucket name corresponding to the project's primary area/field of technology or research. For instance, "databases" or "runtimes" or "compilers" OR "front-end frameworks" OR "front-end libraries" and so on.

**Name:** Name of the project

**Description:** Concise description of what this project is about, what exactly it is and does. Base this on the repo's About description and the README.

**Stars:** How many github stars this project has.

**Languages:** The primary programming langauge(s) used to implement this project.

**GitHub:** GitHub profile URL, empty if none found

**Homepage:** Personal or academic homepage URL, empty if none found

**Top Contributes:** Names of the top contributors to this project (up to 5)

**Sponsors:** Organizations that sponsor this project, commercial or academic (up to 5)

**Created:** the date on which this repo was created

> use the GitHub API https://api.github.com/repos/{:owner}/{:repository} and look for the "created_at" field in the response to find the repo creation date
