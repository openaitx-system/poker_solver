
<div align="right">
  <details>
    <summary >🌐 Language</summary>
    <div>
      <div align="center">
        <a href="https://openaitx.github.io/view.html?user=noambrown&project=poker_solver&lang=en">English</a>
        | <a href="https://openaitx.github.io/view.html?user=noambrown&project=poker_solver&lang=zh-CN">简体中文</a>
        | <a href="https://openaitx.github.io/view.html?user=noambrown&project=poker_solver&lang=zh-TW">繁體中文</a>
        | <a href="https://openaitx.github.io/view.html?user=noambrown&project=poker_solver&lang=ja">日本語</a>
        | <a href="https://openaitx.github.io/view.html?user=noambrown&project=poker_solver&lang=ko">한국어</a>
        | <a href="https://openaitx.github.io/view.html?user=noambrown&project=poker_solver&lang=hi">हिन्दी</a>
        | <a href="https://openaitx.github.io/view.html?user=noambrown&project=poker_solver&lang=th">ไทย</a>
        | <a href="https://openaitx.github.io/view.html?user=noambrown&project=poker_solver&lang=fr">Français</a>
        | <a href="https://openaitx.github.io/view.html?user=noambrown&project=poker_solver&lang=de">Deutsch</a>
        | <a href="https://openaitx.github.io/view.html?user=noambrown&project=poker_solver&lang=es">Español</a>
        | <a href="https://openaitx.github.io/view.html?user=noambrown&project=poker_solver&lang=it">Italiano</a>
        | <a href="https://openaitx.github.io/view.html?user=noambrown&project=poker_solver&lang=ru">Русский</a>
        | <a href="https://openaitx.github.io/view.html?user=noambrown&project=poker_solver&lang=pt">Português</a>
        | <a href="https://openaitx.github.io/view.html?user=noambrown&project=poker_solver&lang=nl">Nederlands</a>
        | <a href="https://openaitx.github.io/view.html?user=noambrown&project=poker_solver&lang=pl">Polski</a>
        | <a href="https://openaitx.github.io/view.html?user=noambrown&project=poker_solver&lang=ar">العربية</a>
        | <a href="https://openaitx.github.io/view.html?user=noambrown&project=poker_solver&lang=fa">فارسی</a>
        | <a href="https://openaitx.github.io/view.html?user=noambrown&project=poker_solver&lang=tr">Türkçe</a>
        | <a href="https://openaitx.github.io/view.html?user=noambrown&project=poker_solver&lang=vi">Tiếng Việt</a>
        | <a href="https://openaitx.github.io/view.html?user=noambrown&project=poker_solver&lang=id">Bahasa Indonesia</a>
        | <a href="https://openaitx.github.io/view.html?user=noambrown&project=poker_solver&lang=as">অসমীয়া</
      </div>
    </div>
  </details>
</div>

# River Poker Solver

This repository builds a no-limit Texas hold'em river subgame solver, starting with Kuhn and Leduc poker for validation. Core algorithms include CFR, CFR+, external-sampling MCCFR, Fictitious Play, and DCFR. Python is the reference implementation; C++ targets performance.

## Repository Layout

- `python/` reference implementation (algorithms, games, CLI tooling).
- `cpp/` optimized C++ river solver.

Contributor guide: see `AGENTS.md`.

## Quick Start

Python (Kuhn/Leduc):

```sh
PYTHONPATH=python/src python -m cli.run_exploitability --game kuhn --algo cfr
```

Python (river defaults):

```sh
PYTHONPATH=python/src python -m cli.run_river_exploitability --algo cfr+
```

Optimized C++ (river defaults):

```sh
cmake -S cpp -B cpp/build
cmake --build cpp/build -j
./cpp/build/river_solver_optimized --algo cfr+ --iters 2000
```

## Subgame JSON Format

GUI exports a JSON file that both C++ solvers can load with `--config`. Key fields:

```json
{
  "board": ["Ks", "Th", "7s", "4d", "2s"],
  "pot": 1000,
  "stack": 9500,
  "bet_sizes": [1.0],
  "include_all_in": true,
  "max_raises": 1000,
  "players": [
    {"hands": ["AsKd", "..."], "weights": [1.0, "..."]},
    {"hands": ["AsKd", "..."], "weights": [1.0, "..."]}
  ]
}
```

Advanced sizing arrays (`oop_first_bets`, `ip_first_bets`, `oop_first_raises`, `ip_first_raises`, `oop_next_raises`,
`ip_next_raises`) are optional and default to `bet_sizes` when omitted.

## Notes

- Defaults use a uniform range, board `Ks Th 7s 4d 2s`, pot 1000, stacks 9500, and bet sizes `0.5, 1.0` with all-in enabled.
- For subgames saved from the GUI, pass `--config path/to/subgame.json`.
