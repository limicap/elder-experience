# 어르신 체험관 (Elder Experience)

A Korean-language elderly-empathy education web app. Static multi-page
HTML/CSS/JS, no build system, no backend. `index.html` is the hub linking
to five self-contained experience pages, each simulating a different aspect
of aging for the visitor:

- **vision.html** — 카메라로 보는 안질환 13종 · 초기·중기·말기 단계별 시야
  변화. Live camera feed run through per-condition visual filters
  (CSS `filter` for most conditions; a dedicated `<canvas>` redrawn every
  `requestAnimationFrame` for effects CSS can't express, e.g. color-blindness
  matrices and the presbyopia "loupe" partial-sharp-patch interaction).
  Includes stage buttons (초기/중기/말기) for every condition, and a
  hemianopia left/right toggle.
- **games.html** — 물건 찾기 · 키오스크 주문 · 지하철 노선도 찾기 · 카메라
  체험, 4가지 게임.
- **hearing.html** — 퀴즈로 맞혀보기 · 지하철·병원 안내음성 정상·경증·중증
  비교.
- **tremor.html** — 미로 통과 · 따라쓰기 채점, 2가지 방식 선택. The maze mode
  overlays a room-grid derived from the real hospital floor plan photo
  (`assets/img/hospital_map.jpg`) with a walkability grid + BFS pathfinding
  (`ROOMS`/`BLOCK_ONLY_RECTS`, line-of-sight-verified path simplification)
  and a guide line whose visible jitter simulates hand tremor at increasing
  severity across 3 mission sets. Per-set miss counts are shown on the
  victory screen (not summed across sets).
- **memory.html** — 암기 → 방해과제 → 회상, 3단계로 알아보는 기억력.

Shared design tokens live in `assets/css/theme.css`. Pages are IIFE-scoped;
any function referenced from an inline `onclick` must be explicitly exported
via `window.fnName = fnName`.

## Workflow conventions

- All development happens on branch `claude/elderly-app-file-linking-h00643`
  against this repo (`limicap/elder-experience`).
- The user's merge-trigger is the literal word **"반영"** — do not merge a PR
  until they say it, even after describing/testing the change.
- Lifecycle per change: implement → verify with Playwright (headless
  Chromium at `/opt/pw-browsers/chromium`, camera mocked via
  `canvas.captureStream()` injected in `page.addInitScript`) → commit → push
  → open a **draft** PR → wait for "반영" → mark ready for review →
  squash-merge → unsubscribe from PR activity.
- Squash-merge means each merged PR becomes one commit on `main`. Since the
  designated branch is reused across PRs, **after every merge**, reset it
  from the fresh default branch before starting new work:
  `git fetch origin main && git checkout -B claude/elderly-app-file-linking-h00643 origin/main`,
  then push with `-u` (force-with-lease if the branch still has the old,
  now-merged history).
- Past PRs (and `git log`) are the authoritative history of what's been
  built and why — this file intentionally does not duplicate a changelog.
