# IPL-data-analysis-using-SQL

#Objective:
The objective of IPL data analysis using SQL is to extract meaningful insights and statistics from the IPL dataset. 
This involves writing SQL queries to answer various questions related to team performance, player performance, match outcomes, and other relevant metrics. 


-- 1. WHAT ARE THE TOP 5 PLAYERS WITH THE MOST PLAYER OF THE MATCH AWARDS?
select player_of_match,count(*) as awards_count
from matches group by player_of_match
order by awards_count desc 
limit 5;

-- 2. HOW MANY MATCHES WERE WON BY EACH TEAM IN EACH SEASON?
select season,winner as team,count(*) as matches_won
from matches group by season,winner;

-- 3. WHAT IS THE AVERAGE STRIKE RATE OF BATSMEN IN THE IPL DATASET?
select avg(strike_rate) as average_strike_rate
from( select batsman,(sum(total_runs)/count(ball))*100 as strike_rate
from deliveries group by batsman)as batsman_stats;

-- 4. WHAT IS THE NUMBER OF MATCHES WON BY EACH TEAM BATTING FIRST VERSUS BATTING SECOND?
select batting_first,count(*) as matches_won
from(select case when win_by_runs>0 then team1
else team2 end as batting_first
from matches
where winner!="Tie") as batting_first_teams
group by batting_first;


-- 5. WHICH BATSMAN HAS THE HIGHEST STRIKE RATE (MINIMUM 200 RUNS SCORED)?
select batsman,(sum(batsman_runs)*100/count(*))
as strike_rate
from deliveries group by batsman
having sum(batsman_runs)>=200
order by strike_rate desc
limit 1;


-- 6. HOW MANY TIMES HAS EACH BATSMAN BEEN DISMISSED BY THE BOWLER 'MALINGA'?
select batsman,count(*) as total_dismissals
from deliveries 
where player_dismissed is not null 
and bowler='SL Malinga'
group by batsman;



