# Vehicles API — Scripted REST APIs in ServiceNow

This repository contains the **Vehicles API** scoped application (`x_snc_vehicles_api`), the teaching app that accompanies the **Scripted REST APIs in ServiceNow** YouTube series. It's a deliberately simple application — a single `Vehicle` table with some demo data — that gives us something concrete to build an API around.

You import it into your own ServiceNow **Personal Developer Instance (PDI)** and follow along with the videos as we build, secure, and harden a Scripted REST API from scratch.

▶️ **YouTube playlist:** https://www.youtube.com/playlist?list=PLrhqGp3sUzhvp1sfgP8OZjXRxKOP_cyBU

> ⚠️ **Work in progress.** This application is not yet complete. It is being developed alongside the video series, and the repository will continue to grow as new videos are released. Existing branches may also be updated as the series is refined.

---

## What the application contains

- **One table** — `Vehicle` (`x_snc_vehicles_api_vehicle`) with the fields `make`, `model`, `year`, `vin`, `country`, and `city`.
- **Demo data** — a small set of vehicle records so your API has something to query.
- **Roles** — a few user and integration roles used later in the series when we apply access controls.

Everything the API itself needs — the web service, its resources, access controls, OAuth configuration, and so on — is added across the series, video by video.

---

## Branches as checkpoints

The repository has multiple branches that act as **checkpoints**, each one reflecting the state of the application at a particular point in the series. Branches are numbered to match the corresponding video.

For example, `stage-05-get-resources` is the state of the application *after* you complete the steps in the GET video (video 05).

This means you can:

- **Start from the beginning** and build everything yourself, or
- **Jump in partway** by importing the branch for the video you want to start from, or
- **Catch up** if you fall behind — import the branch for the video you're on and carry on from there.

`main` holds the latest available state of the application.

> 💡 You can switch between branches in ServiceNow Studio after importing, so you're not locked into the branch you started with. See *Switching branches* below.

---

## Before you start

You'll need:

- A **ServiceNow Personal Developer Instance (PDI)**. If you don't have one, you can request one for free from the [ServiceNow Developer Program](https://developer.servicenow.com).
- **Read access to this repository** (it's public, so anonymous read access is fine).

---

## Installing the application using ServiceNow Studio

The application is imported from source control directly into ServiceNow Studio. The full, official, step-by-step procedure is documented by ServiceNow here:

📖 **[Import an app from source control](https://www.servicenow.com/docs/r/application-development/servicenow-studio-classic/sns-sc-import-app-source-control.html?content-lang=en-US)**

In short, the process is:

1. Open **ServiceNow Studio** in your PDI.
2. Choose to **import an application from source control**.
3. Provide this repository's URL:
   ```
   https://github.com/jnichols-servicenow/SN2026-Vehicles-API
   ```
4. Select the **branch** you want to import — for example, `main` to get the latest state, or a numbered stage branch (such as `stage-05-get-resources`) to start from a specific video.
5. Complete the import. The Vehicles API application will then be available in your instance.

Refer to the ServiceNow documentation linked above for the exact menu locations, credential requirements, and screenshots.

---

## Switching branches

Because the application is linked to source control after import, you can move between checkpoints without re-importing from scratch. Within ServiceNow Studio's source control options, **apply (check out) a different branch** to bring your instance to that stage of the build.

This is handy if you want to skip ahead to a later topic, or roll back to an earlier known-good state.

> Switching branches changes the application files in your instance to match the selected branch. If you've made your own changes, commit or stash them first so they aren't lost.

---

## Following along with the series

Each video builds on the previous one, so if you follow along in order you'll end up with a complete, secured, and hardened API by the end. The videos are also short and focused, so you can drop in on a specific topic and use the matching branch as your starting point.

The series is organised into modules covering the API foundations (CRUD operations, HTTP methods, versioning), security fundamentals (access controls, OAuth 2.0, authentication scopes, API access policies, rate limits), script hardening, and testing and tooling.

▶️ **Watch the full playlist:** https://www.youtube.com/playlist?list=PLrhqGp3sUzhvp1sfgP8OZjXRxKOP_cyBU

---

## A note on demo data

The application is intended to ship with demo vehicle records so your API has something to return. Depending on how the application is installed, demo data behaviour can vary. If you import a branch and find the Vehicle table is empty, check the repository for any supplementary data import instructions, or load the records manually before testing your API calls.

---

## Feedback

Found an issue with the application or a branch? Open an issue on this repository. For questions about the content itself, the comments on the corresponding YouTube video are a good place to ask.

Have fun developing!
