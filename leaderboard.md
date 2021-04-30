---
layout: default
title: Public Leaderboard
---
# P-Rank Leaderboard {#p-rank-leaderboard}
This is the offical Power-Ranking (P-Rank) of all enlisted members! Guild members who are not enlisted will not have their P-Rank published, but can ask for their P-Rank from the guildmaster. Rankings are tracked and listed by Family name. Changing characters may drastically affect P-Rank. Retired and new member's P-Rank may be delayed in their publishing.

|   Family   |  Elo  |
|:-----------|:-----:|
{% for item in site.data.rankings -%}
|<p id="{{item.family}}">{{ item.family }}</p>|{{ item.elo }}|
{% endfor %}

Note: In some circumstances the rankings may be removed, changed, or otherwise falsified. This may be due to node war activity. Speak with the Commander if there seems to be a problem with your published P-Rank.
{: .alert}
