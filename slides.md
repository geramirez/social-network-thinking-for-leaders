---
# try also 'default' to start simple
theme: default
# background: https://cover.sli.dev
# some information about your slides (markdown enabled)
title: What Engineering Leaders Can Learn from Social Network Analysis
info: |
  Presentation slides for What Engineering Leaders Can Learn from Social Network Analysis.

class: text-center
drawings:
  persist: false
# slide transition: https://sli.dev/guide/animations.html#slide-transitions
transition: slide-left
# enable Comark Syntax: https://comark.dev/syntax/markdown
comark: true
# duration of the presentation
duration: 20min
---

# Social Network Analysis for Engineering Leaders


---
transition: fade-out
---

# Who am I and why Social Network Analysis?
<br>
<br>
<div v-click>

### - Became a Senior Engineering Manager at GitHub in 2024
### - Software Engineer in various leadership roles for over a decade. 

</div>

<div v-click>
<br>
<br>

## Also...

</div>

<div v-click>
<br>
<br>

### - Sociologist and Anthropologist 
### - Researched Social Networks in Social Media and Community Language Usage
### - Arabic Linguist (لسا بقدر احكي)

</div>


---
transition: fade-out
---

# What we'll cover

1. Using Social Network Analysis to understand teams and organizations
2. Practical team management insights derived from Social Network Analysis
3. Explore the limits of Social Network Analysis

---
transition: fade-out
layout: center
class: text-center
---

# Social Network Analysis for Understanding 
# Teams and Organizations


---
transition: fade-out
---

# Understanding Teams and Organizations

<br>


```mermaid 
flowchart TD
    CEO["CEO"]

    CTO["CTO"]
    CFO["CFO"]
    COO["COO"]
    CPO["CPO"]

    EngDir["Engineering Director"]
    ITMgr["IT Manager"]
    FinanceMgr["Finance Manager"]
    AccountingMgr["Accounting Manager"]
    OpsMgr["Operations Manager"]
    HRMgr["HR Manager"]
    ProductMgr["Product Manager"]
    DesignMgr["Design Manager"]

    Eng1["Backend Lead"]
    Eng2["Frontend Lead"]
    IT1["SysAdmin"]
    Fin1["Financial Analyst"]
    Ops1["Logistics Lead"]
    Prod1["Product Analyst"]
    Des1["UX Designer"]

    CEO --> CTO
    CEO --> CFO
    CEO --> COO
    CEO --> CPO

    CTO --> EngDir
    CTO --> ITMgr

    CFO --> FinanceMgr
    CFO --> AccountingMgr

    COO --> OpsMgr
    COO --> HRMgr

    CPO --> ProductMgr
    CPO --> DesignMgr

    EngDir --> Eng1
    EngDir --> Eng2
    ITMgr --> IT1
    FinanceMgr --> Fin1
    OpsMgr --> Ops1
    ProductMgr --> Prod1
    DesignMgr --> Des1
```


<!--

- Influencers and Connectors - people who are the center and those that connect different groups together.
- Group dyamics that are not visible in the organizational chart.
- Interpersonal Connections that are not broadcast - for example, a silent contributors that works via 1-1s vs group meetings.
-->


---
transition: fade-out
---

# What can we learn with SNA in a work context?

<br>

```mermaid
graph TD
    CEO["CEO"]
    CTO["CTO"]
    CFO["CFO"]
    COO["COO"]
    CPO["CPO"]

    EngDir["Engineering Director"]
    ITMgr["IT Manager"]
    FinanceMgr["Finance Manager"]
    AccountingMgr["Accounting Manager"]
    OpsMgr["Operations Manager"]
    HRMgr["HR Manager"]
    ProductMgr["Product Manager"]
    DesignMgr["Design Manager"]

    Eng1["Backend Lead"]
    Eng2["Frontend Lead"]
    IT1["SysAdmin"]
    Fin1["Financial Analyst"]
    Ops1["Logistics Lead"]
    Prod1["Product Analyst"]
    Des1["UX Designer"]

    %% Main hub
    CEO --- CTO
    CEO --- CFO
    CEO --- COO
    CEO --- CPO
    CEO --- EngDir
    CEO --- ProductMgr
    CEO --- OpsMgr
    CEO --- FinanceMgr
    CEO --- HRMgr

    %% Secondary hubs
    CTO --- EngDir
    CTO --- ITMgr
    CTO --- Eng1
    CTO --- Eng2
    CTO --- ProductMgr
    CTO --- DesignMgr

    COO --- OpsMgr
    COO --- HRMgr
    COO --- ITMgr
    COO --- ProductMgr

    CPO --- ProductMgr
    CPO --- DesignMgr
    CPO --- Prod1

    CFO --- FinanceMgr
    CFO --- AccountingMgr
    CFO --- Fin1

    %% Mid-tier nodes
    EngDir --- Eng1
    EngDir --- Eng2
    EngDir --- ProductMgr

    ProductMgr --- Prod1
    ProductMgr --- DesignMgr
    ProductMgr --- Des1

    ITMgr --- IT1
    OpsMgr --- Ops1
    FinanceMgr --- Fin1
    DesignMgr --- Des1

    %% A few extra sparse cross-links
    Eng1 --- Prod1
    Eng2 --- Des1
    AccountingMgr --- FinanceMgr
```






---
theme: default
drawings:
  persist: false
transition: slide-left
comark: true
class: text-center
---

# How I applied Social Network Analysis

<img src="./images/pepe-silva.png" alt="It's Always Sunny in Philadelpha Pepe Silva Meme" style="width: 30em; max-width: 100%; max-height: 400px; object-fit: contain; display: block; margin: 0 auto;" />


---
transition: fade-out
---

# Data Collection and Processing

- Started collecting data using a script similar to [gh-graph-explorer](https://github.com/geramirez/gh-graph-explorer) in January 2024. 
- Stored data in an csv edge list (usernames -- interaction -- GitHub Resource)
- Removed data points considered "fake collaboration" like weekly standup reports.
- At first clean up bots... but then decided to leave them. (hubot, dependabot, slack integrations, etc)

<br>
<img src="./images/edge-list.png" alt="example edge list" style="width: 30em; max-width: 100%; max-height: 280px; object-fit: contain; display: block; margin: 0 auto;" />

---
transition: fade-out
class: text-center
---

# Building The Network
<br>

## Bipartite Network

```mermaid
graph LR
    %% Users as circles
    U1(GitHub User A)
    U2(GitHub User B)
    U3(GitHub User C)

    %% PRs as rectangles
    PR1[Pull Request 1]
    PR2[Pull Request 2]

    %% Relationships
    U1 -- "pr created" --> PR1
    U2 -- "pr commented" --> PR1
    U3 -- "pr approved" --> PR1
    U3 -- "pr created" --> PR2
```


---
transition: fade-out
class: text-center
---

# Building The Network

<br>

## Collapsed Bipartite Network

<br>

```mermaid
graph LR
    U1(GitHub User A)
    U2(GitHub User B)
    U3(GitHub User C)

    U1 ---|PR: Pull Request 1| U2
    U1 ---|PR: Pull Request 1| U3
    U2 ---|PR: Pull Request 1| U3

```


---
transition: fade-out
---

# Visualizing The Network

- Tools like [Gephi](https://gephi.org/), [vis-network](https://github.com/visjs/vis-network), [Cosmograph](https://cosmograph.app/), and [Neo4J](https://neo4j.com/)

<br>

<img src="./images/notifications-team-graph.png" alt="1.5 years of GitHub Notifications Team" style="width: 30em; max-width: 100%; max-height: 350px; object-fit: contain; display: block; margin: 0 auto;" />



---
transition: fade-out
---
# Network Analysis
- Took bi-weekly measurements of the number of nodes, connectivity, and density of the network.

<br>

<img src="./images/network-graph-for-my-people.png" alt="Image showing my team's network stats" style="width: 32em; max-width: 100%; max-height: 350px; object-fit: contain; display: block; margin: 0 auto;" />



---
transition: fade-out
---

# Practical Team Management Insights

<br>

1. Team On and Offboardings
2. Cliques and Silos
3. The Seniority Bottleneck
4. Manager Bottlenecks


---
transition: fade-out
---

# Team On and Offboardings

<img src="./images/network-graph-for-my-people-onboarding.png" alt="Image showing my team's network stats" style="width: 35em; max-width: 100%; max-height: 400px; object-fit: contain; display: block; margin: 0 auto;" />

<!--
- it’s well documented that adding or removing people from a team affects team velocity.
- From the network perspective, something similar happens, the social network can also become more fragmented.
- I didn't start collecting on these people until 2-3 weeks after they joined
-->

---
transition: fade-out
---

# Team On and Offboardings Mitigations

- Buddy system 
- Onboarding Round Robins
- Question of the Day or Game Days

<!--

- Buddy system - help people find someone to talk to 
- Onboarding Round Robins Build onboarding guide that requires 1-1s with team mates
-  Question of the Day or Game Days  - Allow for relationship building rather than just pushing people into a silo

-->

---
transition: fade-out
---

# Cliques and Silos

<br>
<img src="./images/cliq-silos.png" alt="Image showing a two silos" style="width: 35em; max-width: 100%; max-height: 380px; object-fit: contain; display: block; margin: 0 auto;" />


<!--
- Cliques and silos are when a group of people form tight-knit subgroups. 
- the Picture shows two of these silos
--> 

---
transition: fade-out
---

# Cliques and Silos Mitigations


- Are cliques and silos bad?
- When It's Positive - Encourage it let it be. Allow deep connections and work.
- When It's Detrimental - Rotations, cross-team projects, mob sessions


<br>
<img src="./images/cliq-silos.png" alt="Image showing a two silos" style="width: 30em; max-width: 100%; max-height: 280px; object-fit: contain; display: block; margin: 0 auto;" />


<!--
- These network proprties can be good or bad
- Bad: only a few people have information 
- Good: people are working very well together and sharing the load with a group of people
- They can also demonsrate a period of Sepicalized work or lack of cliques can show a period of Generalized work
- They can also be a sign of Deep work vs Cross-team work
--> 


---
transition: fade-out
---

# The Seniority Bottleneck

<br>
<img src="./images/central-senior.png" alt="A central senior engineer" style="width: 30em; max-width: 100%; max-height: 380px; object-fit: contain; display: block; margin: 0 auto;" />


<!--
- senior engineers have richer networks: they have been at the organization, find it easier to reach out to others, or have more confidence when reviewing PR
- Their strong social ties are an asset, but they can also make it difficult for more junior engineers to meaningfully participate.
- Over the last two years, I tried a number of social experiments to nudge engineers closer together.
-->



---
transition: fade-out
---

# The Seniority Bottleneck Mitigations

- Mob Sessions
- Creating Junior-only Task Forces

<br>
<img src="./images/seniors-and-juniors.png" alt="A central senior engineer" style="width: 30em; max-width: 100%; max-height: 300px; object-fit: contain; display: block; margin: 0 auto;" />



<!--
 allows juniors to build confidence, practice leadership and communication in a safer environment and a smaller scale. 
- Setting up Mob programming sessions: an easier way to generate group conversations and communication especially when you have an engaging facilitator
-->


---
transition: fade-out
---

# Manager Bottlenecks


<img src="./images/network-graph-for-my-manager.png" alt="Image showing my team's network stats" style="width: 32em; max-width: 100%; max-height: 400px; object-fit: contain; display: block; margin: 0 auto;" />


<!--
- sometimes the bottleneck can be a manager. 
- it's complicated because managers tend tob
-->


---
transition: fade-out
---

# Manager Bottlenecks Mitigations

- Encourage Autonomous Decisions 
- Delegate Meeting Leadership 
- Encourage Continuity

<!--
- Encourage Autonomous Decisions   - can be hard to let go of
- Delegate meeting leadership - for example mob sessions or retros
- Encourage Continuity - if I'm not there, continue the meeting isn't about the manager it's about the team.
-->

---
transition: fade-out
---

# Can we automate Social Network Analysis with AI?

<div style="position: absolute; top: 50%; left: 50%; transform: translate(-50%, -50%); display: flex; justify-content: center; align-items: center; gap: 4rem;">
  <img src="./images/neo4j-logo.svg" alt="Neo4J logo" style="height: 100px; width: auto;" />
  <img src="./images/mcp-logo.png" alt="Model Context Protocol (MCP) logo" style="max-width: 220px; max-height: 100px; width: auto; height: auto;" />
  <img src="./images/claude-desktop-logo.svg" alt="Claude Desktop logo" style="height: 100px; width: auto;" />
</div>

<!--
Yes, we can use AI to automate the process of collecting, analyzing, and visualizing social network data.
- Neo4J has a graph database that can be used to store and query social network data
- Model Context Protocol (MCP) can be used to provide context to the AI 
- Claude Desktop can be used to interact with the AI and get insights from the data
-->

---
transition: fade-out
---

# Engineer Manager Claude

<img src="./images/claude-prompt.png" alt="Screenshot of a Claude prompt for engineering manager tasks" style="max-height: 75%; width: auto; display: block; margin: 0 auto;" />


---
transition: fade-out
---

# Engineer Manager Claude

<img src="./images/claude-explorer.png" alt="Screenshot of asking Claude to explore team network data" style="width: 100%; max-height: 80%; object-fit: contain; display: block; margin: 0 auto;" />


---
transition: fade-out
---

# Pause

<img src="./images/pause-symbol.svg" alt="Pause symbol" style="width: 200px; height: auto; display: block; margin: 2rem auto;" />

---
transition: fade-out
---

# Thinking before leaping

<!--
- For anyone that has done softare engineering with AI, you know that building complexity is easier than ever. 
- In the recent past when you had to manually code line by line, you had time to think about if the thing your building makes sense. 
- The same applies in this case. Using AI to automate difficult managerial tasks is becoming easier.
- But we should pause and think about the implications, ethics, and limits of what we are doing. 
-->

---
transition: fade-out
class: text-center
layout: center
---

# Limits of Social Network Analysis

---
transition: fade-out
---

# The Network Needs a Manager

Social Network Analysis clarifies a snapshot of interactions. It doesn't tell you why or what to do about it.

- 1-1s: surface topics that can't be graphed
- Retrospectives: help us decide what should happen
- Coaching and Mentorship: requires interpersonal connection and trust

<br>

<img src="./images/notifications-team-network-graph.png" alt="Notifications Team Network Graph" style="width: 30em; max-width: 100%; max-height: 280px; object-fit: contain; display: block; margin: 0 auto;" />

<!-- 
Without context, interpreations are worthless. SNA is not a shortcut to understanding your team. 
-->


---
transition: fade-out
---

# Performance Metrics? 
<br>
<img src="./images/imposter.png" alt="imposter xkcd" style="width: 34em; max-width: 100%; max-height: 340px; object-fit: contain; display: block; margin: 0 auto;" />

XKCD. (2013). "Impostor". XKCD. https://xkcd.com/149/

<!--
- With these insights, it can be tempting to turn social network analysis into a performance metric
- especially when we can easily use computational methods to calculate things like how central a person is to the network
-->


---
transition: fade-out
---

# Context Matters


### Reasons for High Connectedness
- Leader
- Glue work
- Low value work

<br>

### Low Connectedness and Isolation Reasons
- Vacation
- Deep Work and Research 
- Personal Issues 


<!-- 
Low connectedness and isolation can have so many different interpretations and the tools we need to apply are rarely performance management.

1. People are on vacation: this can be positive, having a person who is a central node go on a two-week vacation can give the space for new connections to form.
2. People are working through something personal: as managers, if we see an isolated individual we should be curious first and see if we can support them 
3. Other reasons include burnout, wrong fit, or lack of skills: noticing a person is isolated is only the first step, we still need our other tools like 1:1s, coaching to navigate difficult situations
-->
 

---
transition: fade-out
---

# Performance Metrics or Leadership Report Cards?

<img src="./images/network-graph-for-my-people.png" alt="Image showing my team's network stats" style="width: 32em; max-width: 100%; max-height: 400px; object-fit: contain; display: block; margin: 0 auto;" />

<!--
If anything it's a performance metrics for us as leaders. Are we being good team custodians?
-->

---
transition: fade-out
layout: center
class: text-center
---

# How to get Started with Social Network Analysis?

---
transition: fade-out
---

# [geramirez/gh-graph-explorer](https://github.com/geramirez/gh-graph-explorer?tab=readme-ov-file#installation)

<br>
<img src="./images/gh-graph-explorer.png" alt="QR Code for https://github.com/geramirez/gh-graph-explorer" style="width: 350px; max-width: 100%; max-height: 380px; object-fit: contain; display: block; margin: 0 auto;" />



---
transition: fade-out
---

# [Obsidian Graph View](https://obsidian.md/help/plugins/graph)

Using links you can map out connections during 1-1s, team meetings, or retrospectives.

<img src="./images/graph-view.png" alt="Obsidian graph-view plugin" style="width: 350px; max-width: 100%; max-height: 340px; object-fit: contain; display: block; margin: 0 auto;" />


<!--
This is how early anthropologist used to collect data
-->


---
transition: fade-out
---

# Observation and Listening


- Who do people go to when they're stuck?
- Who always seems to know what's happening across teams?
- Who is the last person to learn about important decisions?


<!--

- Anthropologists mapped social structures by watching and listening 
- Best leaders are aware of the social networks around them.
- If you can answer these questions your well on your way. 
- Hopefully this talk gives the framing to understand what's happening

-->

---
transition: fade-out
---

# What we covered

1. Using Social Network Analysis to understand teams and organizations
2. Practical team management insights derived from Social Network Analysis
3. Explore the limits of Social Network Analysis


---
transition: fade-out
---

# Thank you!
