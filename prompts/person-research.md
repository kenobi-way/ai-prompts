# Guidance for conducting web research on individuals of interest

Proceed as follows:

1. Try to find each person’s Google Scholar profile; if found, see if they included their Homepage on their Google Scholar profile -- which may be a link to their lab/homepage, their LinkedIn, their GitHub profile, and so on.

2. if you have found their homepage on their Google Scholar profile, look on their homepage for links to additional online profiles. If the homepage was not included on their Google Scholar profile, do a web search to see if you can find their homepage. In many cases, this will be a GitHub profile; e.g. https://zswang666.github.io/. In many other cases, this will be a vanity URL derived from their name or handle, such as https://hariri.dev/, If you find their homepage, look there for additional online profiles.

3. In general, whenever you find and confirm a valid profile/link, look on that page for additional linked profiles -- github, linkedin, blog/medium/substack, X/twitter, bluesky, and so on -- as this is the best way to find confirmed correct related profiles (online entity resolution is challenging). Links may not always appear in the usual places, as in the case of this engineer who links to his github from the Projects sections of his LinkedIn: https://www.linkedin.com/in/pulpdrew/

4. Also look for a resume, which may be linked to from their homepage, their LinkedIn profile, or within one of the GitHub repos. If you find it, save a link to their resume and open it to look for their email address and additional links.

5. Find their GitHub profile, or open it if you have already found it, and look there for additional links.

6. Find their LinkedIn profile, or open it if you have already found it, and look there for additional links.

7. Find their X/Twitter profile, or open it if you have already found it, and look there for additional links.

8. As you proceed, double-check any links you find. Do not include any broken or incorrect links; instead, indicate that you could not find the correct link. If a link is broken or incorrect, try harder to find a correct link.

Some tips to find additional online profiles and/or confirm that two profiles are for the same person (entity resolution):

  - Look at URLs to see if the person has a custom profile handle, e.g. “zswang666” for Johnson Wang (https://github.com/zswang666) and consider searching on that handle as it may be re-used across different online platforms.
  - If we only have someone’s last initial, not their full last name, you can often find their full name by searching the web with their first + last initial + additional background information, such as their high school or lab advisor… then you can find them mentioned by full name on their lab homepage or a blog post or news article from their high school days.
  - You can compare profile pictures to see if the profiles correspond to the same person.

Summarize your findings in a table ready for CSV export, with the following columns:

**Source:** Come up with a reasonable name to identify the source material for each person we are researching. For instance, if we are looking up the authors of a research paper, put the name of the paper in this column. If we are researching everyone found in a startup pitch deck, come up with an appropriate Source name like "[Startup] pitch deck" (assuming we know the name of the startup).

**Person:** Full name of the individual

**Role:** Current role/title, e.g., "Co-founder & CEO" or "Research Scientist at DeepMind"

**Location:** Where this person is based (lives), empty if not known

**Email:** Email address found for this person; if more than one likely email is found, this should be a comma-separated list with the most likely email listed first; empty if none found

**Tags:** Comma-separated list of 1-5 tags that could be used to classify or categorize the type of work this person does, their areas of expertise, their notable skills. For instance, "AI Research Scientist" or "Systems Software Engineer" or "Graphics Research Engineer" OR "Front-end Engineer" OR "Reinforcement Learning" and "Foundation Models" or "Physical AI" and so on.

**Notable Accomplishments & Background:** A 1-line summary of the person’s background and key accomplishments.
  - Example 1: "led the Genesis embodied AI research initiative, PhD CMU Robotics"
  - Example 2: "led Mistral’s first multimodal model, founding researcher at Skild AI, PhD CMU Machine Learning"
  - Example 3: "designed Taichi simulation core, assistant professor at CMU Graphics Lab"
  - Example 4: "developed dexterous manipulation techniques at NVIDIA, PhD in Robotics from Stanford"

**LinkedIn:** LinkedIn profile URL, empty if none found

**Homepage:** Personal or academic homepage URL, empty if none found

**Publications:** Prefer Google Scholar URL, fallback to DBLP, Semantic Scholar, etc., empty if none found

**GitHub:** GitHub profile URL, empty if none found

**X/Twitter:** X (twitter) profile URL, empty if none found

**Other Links:** any other URLs found, empty if none found