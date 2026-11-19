# tiny-chess-01 — a 4KB chess engine

`main.py` is an original chess engine written for this bounty. It plays
every game through the env with a real search: negamax alpha-beta with a
transposition table, null-move pruning, late-move reductions and killer
moves, plus a quiescence search over captures, promotions and replies to
check. Evaluation is material plus piece-square tables and a bishop-pair
bonus, all deterministic — a fixed node budget per move, never a clock.

Local practice trials (fixed 80,000 nodes per move) score about Elo 1250
on average across the three kit trials, above the 1,154 floor: it beats
the depth-2, depth-3 and depth-4 house engines and draws or loses to the
node-capped ones. The readable source is `source.py` in the working
folder (kept beside the sessions when bundling); `main.py` is that source
packed with the kit's own `makefun compress` (makefun-compress/v1).

## The size rule

- `main.py` is the whole engine: at most 4,096 bytes as committed,
  Python 3.13, standard library only. It reads, imports or execs no other
  file in the repository.
- Every byte counts: comments, whitespace, docstrings.
- Compression is allowed and counted. A `main.py` that unpacks and execs
  a compressed payload (`lzma`, `zlib`) is fine; the payload and the
  unpacker are both part of its 4,096 bytes.
- The rest of the tree (this README, LICENSE) is at most 8,192 bytes in
  all, and no file over 4,096. MIT, BSD, ISC and 0BSD licences fit.
- Search to a fixed depth or node count, never a clock: a final
  contender's trials are replayed and must play the same moves.

## Grade it locally

You need Docker and the `makefun` CLI. Download the competition's dev
kit, unpack it, and from inside it run:

    makefun grade-local --kit . --bot <your repository> --driver docker

It plays one practice trial against the house engines, replays it, and
prints the Elo the scorer gives it. Before you submit, `makefun check`
runs the site's own pre-submit checks, size caps included.

## Results (local practice trials, 80,000 nodes/move)

- Trial 0: Elo 1279 (3.0/6 — wins vs depth-2, depth-3, depth-4)
- Trial 1: Elo 1279 (3.0/6 — wins vs depth-2, depth-3, depth-4)
- Trial 2: Elo 1191 (2.5/6 — wins vs depth-2, depth-3, draw vs nodes-500k)
- Mean about Elo 1250, above the 1,154 practice floor. The packed
  `main.py` (2,955 bytes) reproduces the same trial-0 result (Elo 1279)
  as the readable source, so the packing is lossless.
