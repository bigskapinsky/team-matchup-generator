# Team Matchup Generator

The scenario is the following:

You are hosting Olympiads (for example at a work retreat or at a wedding, birthday party, whatever...) and you have a certain number $n$ of teams, let's say 10. We'll name them A through J.

- There are $\frac{n}{2}$ different activities. For our example, we will call them activities 1 through 5
- There are also $\frac{n}{2}$ different time slots for each matchup
- Matchups are always between two teams
- Teams must compete in all events once and only once
- Teams must never compete against the same team twice
    - This does not mean that they must compete against every other team!

The question is, how do you generate a schedule for this? Try it on paper, for more than 8 teams, it gets very complex very fast!

This is why I created this bit of code, which is arguably not the most elegant solution, but I was crunched for time. I'm sure there could be some recursive tree-pruning way to solve this faster.

# How the code works

This notebook generates however many teams are needed and generates the matchups randomly. 

The matchups are calculated timeslot by timeslot, and then event by event:
 - We look at which teams haven't been assigned for this timeslot yet
 - We check which teams haven't participated in this event yet
     - If no teams are available, we scrap everything and start again
 - We randomly assign a team to this event and this timeslot
 - We check which of the remaining teams haven't competed against this team yet
     - If no teams are available, we scrap everything and start again
 - Keep going until we find a solution!

# Results

I did a few benchmark tests to see how the compute time rises as more teams are added:
| n_teams | compute time (s) | attempts |
|---------|------------------|----------|
|       6 |           0.0075 |        1 |
|       6 |           0.0055 |        1 |
|       6 |           0.0073 |        1 |
|       6 |           0.0071 |        1 |
|       6 |           0.0059 |        1 |
|       8 |           0.0618 |       30 |
|       8 |           0.0282 |       13 |
|       8 |           0.0229 |       11 |
|       8 |            0.027 |       12 |
|       8 |           0.0201 |        8 |
|       8 |           0.0124 |        4 |
|      10 |           0.3623 |      181 |
|      10 |           0.0212 |        7 |
|      10 |           1.1347 |      579 |
|      10 |          0.04826 |       22 |
|      10 |           0.4478 |      226 |
|      12 |           49.515 |    19916 |
|      12 |          53.0612 |    21192 |
|      12 |           27.613 |    11018 |
|      12 |           2.7094 |     1073 |
|      12 |            23.33 |     9256 |

The time increase seems to follow a power series:

![image](https://github.com/bigskapinsky/team-matchup-generator/assets/11352293/6841f501-f82a-47de-b284-a829d7d9c6b6)

If you can think of a more efficient way to compute this to get it down to polynomial or even linear time... Fork away!
