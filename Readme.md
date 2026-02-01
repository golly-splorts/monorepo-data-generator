# ai-monorepo-data-generator

This repository is a monorepo. It combines multiple repos for 
generating data for a competitive cellular automata sport.


# Overview of the Sport

## Unions and Teams

The sport consists of two unions:

* Golly Union
* Peninsula Union

The unions are composed of a set of 20 teams,
divided into two leagues (10 teams each) and four divisions
(5 teams each).

## Cups

The teams compete for 24 seasons in each cup.
Initially cups had one-off names but have settled into a
consistent naming convention with roman numerals.

Golly Union:
* Hellmouth Cup
* Toroidal Cup
* Rainbow Cup
* Klein Cup
* Hellmouth II Cup
* Hellmouth III Cup
* Hellmouth IV Cup
* etc...

Peninsula Union:
* Pseudo Cup
* Dragon Cup
* Star Cup
* Star II Cup
* Star III Cup
* Star IV Cup
* etc...

## Rules

The Golly Union implements a competitive version of the classic Game of Life
cellular automata, B3/S23. The Peninsula Union's earlier Cups implemented various
cellular automata rules, but the Star Cups all implement the Star Wars cellular
automata, B2/S345/C4.


# Overview of the Components

The data generation process involves several libraries:

* gollyx-python - core cellular automata simulator. Implements B3/S23 and B2/S345/C4 
  cellular automata on periodic grids.
* gollyx-maps - generates the initial conditions for various map patterns, used by
  compute workers simulating games to generate initial conditions. Generates unique 
  maps, even for the same pattern, so even though each game is deterministic, it is 
  also unique.
* gollyx-backend-generator - takes teams as input, handles the generation regular season
  and postseason schedules, and manages the process of simulating each game.
* gollyx-automation-data - provides a meta-layer for orchestrating
  the different steps required to use the above three libraries to generate
  and simulate 24 seasons for a new Golly cup.


# Overview of the Data Generation Process

The gollyx-backend-generator library performs the following steps:

* Takes a list of teams and a season number as an input
* Generates a schedule based on the league/division structure
* Simulates each game in the schedule
* Determines final season ranking, assembles postseason bracket
* Simulates each game in the postseason bracket
* Final output for a given season is the regular season schedule,
  regular season game outcomes, postseason schedule, and postseason
  game outcomes.

The compute steps can be carried out in one of two ways, using the
orchestrator:

1. Local thread pool mode - the orchestrator code runs all backend
   generator tasks on the local machine. The backend generator has
   a set of tasks and a pool of threads, runs each game, and assembles
   the results that are output to disk (gameid.json).

2. Lambda mode - the user deploys an orchestrator EC2 instance, along with
   compute worker lambda function (container), and an SQS task queue.
   The orchestrator code generates the season and postseason schedules, 
   and populate the SQS queue with tasks.
   The SQS queue runs a lambda for each task, which output game outcomes to S3.
   The orchestrator EC2 assembles everything into the final output.

















