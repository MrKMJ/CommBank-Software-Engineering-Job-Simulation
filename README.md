# Commonwealth Bank Software Engineering: Job Simulation (Forage)

My work for the [Commonwealth Bank Software Engineering job simulation](https://www.theforage.com/simulations/commonwealth-bank/software-engineering-c4hw): **adding emoji icons to savings goals**, end to end across a C#/.NET + MongoDB API (`server/`) and a React/Redux + TypeScript web app (`web/`).

The starter code comes from the program's public repositories ([commbank-server](https://github.com/fencer-so/commbank-server) and [commbank-web](https://github.com/fencer-so/commbank-web)). It is imported unchanged in the first commit. My changes are on the **`icons`** branch in a single **"Support icons"** commit.

| Task | Change |
|---|---|
| 1. Extend the .NET backend | `server/CommBank-Server/Models/Goal.cs`: optional `Icon` (`string?`) field |
| 2. Extend the React/Redux frontend | `web/src/api/types.ts` (`icon: string \| null`), `GoalCard.tsx` (shows the icon), `GoalManager.tsx` (Add-icon button, emoji picker, Redux update) |
| 3. Persist icons | `GoalManager.tsx` calls the `updateGoal` PUT request (`PUT /api/Goal/{id}`) when an emoji is picked |
| 4. Test coverage | `server/CommBank.Tests/GoalControllerTests.cs`: `GetForUser` test (not-null, count, type, owning `UserId`, order) |
| 5. Git | `icons` branch, "Support icons" commit |

### Two things I fixed along the way
- **Icon overwritten by later edits:** the name, amount and date handlers spread the modal's original `props.goal`. After picking an emoji, editing the name would PUT the *old* icon back. Each handler now carries the current icon forward.
- **Test that couldn't fail:** the fake goals have no `UserId`, so the suggested assertion compared `null` with `null`. The test now gives the goals a real owner before asserting.

## Verification
- Web: `npx tsc --noEmit` and a production build pass. In a browser, picking 👍 on "House Down Payment" shows it in the editor and on the goal card, and sends `PUT /api/Goal/62a3f587102e921da1253d32` with `"icon": "👍"` (API responses were mocked with the program's seed data).
- Server: GitHub Actions runs `dotnet build` and `dotnet test` on every push (see the **ci** workflow).

## Run locally
```bash
# API (needs a MongoDB Atlas connection string in server/CommBank-Server/Secrets.json)
cd server && dotnet run --project CommBank-Server
# Web
cd web && npm install && npm start
```
