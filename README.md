# VerbFlash

A tiny game to learn English verbs by speaking them out loud. Spring Boot (Kotlin) + a static web page.

## Running

    mvn spring-boot:run

then open **http://localhost:8080** in **Google Chrome** (the browser's speech recognition is required) and allow microphone access.

## Exam mode ("Verifica")

The **Verifica** button asks 10 random verbs, always from English to Italian, like a teacher would:
the first complete answer counts, and whether it was right is shown only at the end, with the score and
the list of verbs. The wrong verbs are then read aloud with their translation, and **Ripassa gli errori**
asks them again with immediate feedback until each one is right. Exam and review answers are not recorded
in the statistics.

## Configuration (`src/main/resources/application.yml`)

| property | default | meaning |
|---|---|---|
| `verbflash.answer-timeout-seconds` | 10 | seconds to answer (can also be changed from the settings on the page) |
| `verbflash.verbs-file` | `classpath:verbs.csv` | verbs file, e.g. `file:/Users/me/verbs.csv` |
| `verbflash.italian-to-english-ratio` | 0.5 | probability of an Italian prompt |
| `verbflash.stats-file` | `data/stats.json` | file where statistics are stored |

They can also be passed on the command line:

    mvn spring-boot:run -Dspring-boot.run.arguments="--verbflash.answer-timeout-seconds=15"

## Verb list

One verb per line, `key;english;italian`; multiple accepted answers separated by `|`:

    get;get;ottenere|prendere|ricevere

The key identifies the verb in the statistics: it must be unique and should never change,
so translations can be edited freely without losing the verb's history.

## Deploying on Render

The repo has a `Dockerfile` and a `render.yaml` blueprint. On Render: **New → Blueprint**,
pick this repository, and it builds the Docker image and serves it over HTTPS
(required by the browser for microphone access). The `PORT` provided by Render is picked up automatically.

On the free plan the filesystem is ephemeral, so statistics are reset on every redeploy,
restart and idle spin-down (after ~15 minutes without traffic). To keep them, switch to a paid
plan and enable the persistent disk commented out in `render.yaml`.
