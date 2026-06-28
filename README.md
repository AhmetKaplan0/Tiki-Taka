# Tiki-Taka

**Tiki-Taka** is an AI-powered football tactical intelligence solution built with UiPath. It helps coaches and analysts select a matchup, analyze the opponent, generate a tactical plan, and automatically update the home team lineup based on the recommended formation.

## Project Description

Football is becoming more digital, data-driven, and automated. Before every match, coaches and analysts need to review opponent history, team profiles, player availability, tactical strengths, weaknesses, formations, and risk factors. This preparation is usually manual, time-consuming, and disconnected across multiple tools.

Tiki-Taka solves this by using UiPath Apps, Data Fabric, Maestro, and AI Agents to create an end-to-end match preparation workflow.

The user selects the home team and opponent team in the app. The system then triggers an automated workflow that:

1. Creates the match context.
2. Clears previous player position assignments.
3. Analyzes the opponent.
4. Generates a tactical plan.
5. Updates the lineup based on the recommended formation.
6. Visualizes the pitch, bench, risk score, and tactical insights in UiPath Apps.

## What the Solution Does

* Selects home and opponent teams from UiPath Apps.
* Creates a match record in Data Fabric.
* Resets Home Team player `SelectedPosition` values before a new analysis.
* Uses AI agents to analyze opponent data dynamically.
* Generates an opponent analysis with:

  * Strengths
  * Weaknesses
  * Key players
  * Main threats
  * Exploitable zones
  * Opponent risk score
* Generates a tactical plan with:

  * Recommended formation
  * Game model
  * Offensive plan
  * Defensive plan
  * Pressing plan
  * Substitution plan
  * Tactical score
* Updates Home Team player positions based on the tactical plan.
* Displays the final tactical lineup on a football pitch.

## UiPath Components Used

| Component                         | Description                         | Purpose                                                                                                      |
| --------------------------------- | ----------------------------------- | ------------------------------------------------------------------------------------------------------------ |
| UiPath Apps                       | Front-end web app                   | Allows users to select teams, trigger analysis, view pitch lineup, bench, risk score, and tactical insights. |
| UiPath Data Fabric / Data Service | Structured data layer               | Stores teams, matches, players, opponent analyses, tactical plans, and images.                               |
| UiPath Maestro BPMN               | Process orchestration               | Coordinates the end-to-end flow between app actions, RPA workflow, agents, and data updates.                 |
| UiPath Agents                     | Low-code AI agents                  | Performs opponent analysis, tactical planning, and lineup coordination.                                      |
| UiPath Studio Web                 | Workflow and automation development | Used to build and configure process steps and app-triggered automation.                                      |
| RPA Workflow                      | SetPlayerEntityValue                | Clears previous Home Team player `SelectedPosition` values before a new analysis starts.                     |
| Custom HTML / JavaScript          | App UI enhancement                  | Supports richer app visualization and dynamic display elements.                                              |
| Data Fabric Entities              | Relational football data model      | Provides structured data for AI reasoning and workflow execution.                                            |

## Agent Type

This solution uses **Low-code Agents** built with UiPath Agent Builder.

No coded agents are used in the current version.

## AI Agents

| Agent                   | Description                                                            | Purpose                                                                                                                  |
| ----------------------- | ---------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------ |
| OpponentScout Agent     | Reads match, team, player, and historical match data from Data Fabric. | Creates a dynamic opponent analysis with strengths, weaknesses, key players, threats, exploitable zones, and risk score. |
| TacticalPlanner Agent   | Reads only the generated OpponentAnalysis record.                      | Creates a TacticalPlan with recommended formation, game model, tactical instructions, and tactical score.                |
| LineupCoordinator Agent | Reads the TacticalPlan and Home Team player records.                   | Updates each player’s `SelectedPosition` according to the recommended formation and squad role.                          |

## Data Fabric Entities

| Entity           | Description                                                                                                                                               | Purpose                                           |
| ---------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------- |
| TeamEntity       | Stores team profile data such as team name, league, playing style, default formation, team type, and team image.                                          | Used to define home and opponent teams.           |
| Match            | Stores planned and historical match data.                                                                                                                 | Used as the central match context for analysis.   |
| Player           | Stores squad data including player name, team, default position, selected position, squad role, fitness, attack, defense, availability, and player image. | Used for analysis and lineup generation.          |
| OpponentAnalysis | Stores AI-generated opponent scouting results.                                                                                                            | Used as the input for tactical planning.          |
| TacticalPlan     | Stores AI-generated tactical strategy.                                                                                                                    | Used as the input for lineup position assignment. |
| PlayerImage      | Stores player image references.                                                                                                                           | Used by the app UI to display player cards.       |
| AppConfig        | Stores app-related file configuration.                                                                                                                    | Used for app configuration and file references.   |

## Main Workflow

1. User opens Tiki-Taka in UiPath Apps.
2. User selects the Home Team and Opponent Team.
3. User clicks **Analyze Matchup**.
4. Maestro starts the end-to-end process.
5. The workflow creates a Match record.
6. `SetPlayerEntityValue` clears previous Home Team `SelectedPosition` values.
7. OpponentScout Agent analyzes the opponent and creates an OpponentAnalysis record.
8. TacticalPlanner Agent creates a TacticalPlan record.
9. LineupCoordinator Agent updates Home Team player positions.
10. UiPath Apps refreshes the data and displays the final lineup and insights.

## Formation and Position Logic

The solution supports multiple tactical formations, including:

* 4-4-2
* 4-3-3
* 3-5-2
* 3-4-3
* 4-2-3-1
* 4-4-1-1
* 4-3-2-1
* 4-1-2-1-2
* 4-1-4-1
* 4-5-1
* 4-3-1-2

Player positions are handled through Data Fabric ChoiceSet values, including:

* GK
* CB
* LB
* RB
* DM
* CM
* AM
* LM
* RM
* LW
* RW
* ST

The LineupCoordinator Agent applies strict formation rules to avoid invalid lineup combinations and to ensure that each Starting XI player is mapped to the correct tactical slot.

## Setup Instructions

### 1. Import the Data Fabric Schema

Create or import the required Data Fabric entities:

* TeamEntity
* Match
* Player
* OpponentAnalysis
* TacticalPlan
* PlayerImage
* AppConfig

Make sure all relationship fields and ChoiceSets are configured correctly.

### 2. Prepare Sample Data

Add sample records for:

* Teams
* Players
* Historical matches
* Planned matches
* Team images
* Player images

For the demo, the app can use teams such as Türkiye, Brasil, France, Paraguay, Spain, Italy, and Morocco.

### 3. Configure UiPath Apps

Import or recreate the Tiki-Taka app.

The app should include:

* Home Team selector
* Opponent Team selector
* Analyze Matchup button
* Opponent Team Analysis button
* Create Tactic button
* Pitch visualization
* Bench list
* Risk score display
* Insight buttons for Main Threats, Strengths, Key Players, and Weaknesses

### 4. Configure Maestro BPMN

Create the BPMN process that orchestrates:

1. Match creation
2. SetPlayerEntityValue RPA workflow
3. OpponentScout Agent
4. TacticalPlanner Agent
5. LineupCoordinator Agent
6. Data refresh back to UiPath Apps

### 5. Configure the RPA Workflow

Create the `SetPlayerEntityValue` workflow.

Purpose:

* Triggered when **Analyze Matchup** is clicked.
* Finds Home Team players.
* Clears existing `SelectedPosition` values before the new tactical analysis starts.

This ensures each new matchup starts with a clean lineup.

### 6. Configure the Agents

Create the following low-code agents:

#### OpponentScout Agent

Input:

* MatchId

Output:

* OpponentAnalysisId
* Risk score
* Analysis summary

Purpose:

* Reads Match, Team, Player, and historical Match data.
* Generates the opponent analysis.
* Creates an OpponentAnalysis record.

#### TacticalPlanner Agent

Input:

* OpponentAnalysisId

Output:

* TacticalPlanId
* Recommended formation
* Game model
* Tactical score
* Tactical summary

Purpose:

* Reads the OpponentAnalysis record.
* Generates a tactical plan.
* Creates a TacticalPlan record.

#### LineupCoordinator Agent

Input:

* TacticalPlanId

Output:

* Updated Starting XI count
* Updated Bench count
* Lineup summary
* Warnings

Purpose:

* Reads the TacticalPlan record.
* Resolves the linked Match and Home Team.
* Retrieves Home Team players.
* Updates each player’s `SelectedPosition` based on the recommended formation.

### 7. Connect UiPath Apps to the Process

In UiPath Apps:

1. Configure the **Analyze Matchup** button.
2. Use **Start Process** to trigger the Maestro process.
3. Pass selected team or match values through Input Override.
4. Store process output values in app variables.
5. Refresh Data Fabric queries after the process completes.
6. Display the updated pitch, bench, risk score, and insights.

### 8. Run the Demo

Recommended demo flow:

1. Open the Tiki-Taka app.
2. Select Türkiye as the Home Team.
3. Select an opponent team.
4. Click **Analyze Matchup**.
5. Show the opponent risk score and scouting insights.
6. Click **Create Tactic**.
7. Show the generated tactical plan.
8. Show the pitch with updated player positions.
9. Show the bench and final formation.

## Demo Value

Tiki-Taka demonstrates how UiPath AI Agents can act as active workflow participants, not just chat assistants. The agents read structured data, reason over football context, generate tactical decisions, update records, and make the result visible in a user-friendly app.

## Tagline

**Analyze. Plan. Win.**
