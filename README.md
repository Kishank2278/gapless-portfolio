<p align="center"><img src="logo.svg" width="140" alt="Gapless app logo"></p>

# Gapless

**An iOS app that helps you move from the career you have to the career you want — free.**

Pick your current role and your target role. Gapless finds the skills in between, matches each one with a free learning resource, and shows real jobs for the target role.

- **Skill gap:** current role → target role → the 5 missing skills, ranked High / Medium / Low, with completed skills sinking to the bottom and progress saved per user.
- **Resume-aware:** paste a resume and the plan respects what you already know — skills it clearly shows drop out and the next gaps take their place. (Verified live: a Python/Git-heavy resume made both drop out of an IT Support → Software Engineer plan.)
- **Learning resources, in order:** MIT OpenCourseWare (when genuinely relevant), Wikipedia, YouTube, Open Library, Coursera once at the bottom. No invented courses or URLs — links are resolved server-side from provider APIs or clearly labelled as search links.
- **Jobs:** real listings via Adzuna, through a server-side proxy.
- **Learn sheet:** per-skill deep dive; the AI returns topics only, never URLs, and the server resolves real links.

## Stack

- **iOS:** SwiftUI, Xcode, iOS 17+
- **Backend:** Supabase — Postgres, Auth, RPC, Edge Functions (Deno / TypeScript)
- **Data:** curated career directory + O*NET 31.0
- **AI:** OpenRouter (free models) via Edge Functions
- **Jobs:** Adzuna API via server-side proxy
- **Resources:** MIT OCW, Wikipedia, YouTube, Open Library, Coursera

## How it works

`SkillGapView → skill-path` (directory plan via `get_skill_path`, or an AI plan for roles outside the directory) → resource enrichment with provider rotation and relevance gates → plan cached per user in `skill_gap_cache`. `LearnSkillView → Gemini-OpenRouter-Proxy` for the per-skill Learn sheet. Degree requirements come from the College Scorecard API.

## Security, on purpose

- AI functions require a verified signed-in user token (401 without one); signed-out reads of user tables return empty (RLS verified by live probe).
- Per-user rate limit, 60 requests/hour, on both AI functions.
- No keys in client code or the repo — OpenRouter, YouTube, Adzuna and the service-role key live only in Edge Function secrets. The AI never sees a key and never generates a URL.

## Status

v1.0 (build 1) — code complete on `Gapless_V3`, in TestFlight soft launch. Next: a designer-style UI polish pass (V4) based on real tester feedback — type hierarchy, one spacing system, fewer card shadows. No new features in V4, no logic changes.
