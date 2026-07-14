# Instructor Notes

## De{c0}dE REGIONAL CAFÉS — Break & Secure: Cyber X AI

- **Model choice drives whether attacks land.** Gemini/Claude frequently refuses - that's the vendor-safety
  lesson, so *use it*. When you want an exploit to land every time, switch `PROVIDER` to
  `"litellm"` and pick an open model (`qwen3-coder-30b`, `glm-4.7-flash`). Test your exact key + model
  the night before; behavior drifts.
- **Your LiteLLM credentials are temporary.** Both the key and the base URL are revoked after the
  event. Share both at the start of the session; students enter them in the Setup cell when prompted
  (read with `getpass` - neither value is hard-coded in the notebooks).
- **Rate limits:** free-tier quota is per-project. 30 students on one key *will* throttle. Everyone on
  their **own** AI Studio key avoids this - bake it into the 15-min setup window.
- **Lab 5's best line:** the MCPTox benchmark found *more capable* models obey poisoned tool
  descriptions *more often* (attack success >60%, top ~72%), because they follow instructions better.
  Set it up as a bet before the reveal.
- **Gamify:** one flag per lab. First-to-flag on a shared leaderboard; after everyone secures Lab 1,
  pair students to attack each other's *fixed* versions - someone always finds a bypass.
