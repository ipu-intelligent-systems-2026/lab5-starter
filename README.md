# Lab 5: Evaluation, Guardrails & LLM Reliability
# 課題5：評価、ガードレール、LLM信頼性設計

## [EN] Overview

You will build the reliability layer around an LLM application:

| Part | File | What you implement |
|---|---|---|
| A. Schema enforcement | `src/guardrails.py` | Strict Pydantic schema: no hallucinated fields, no empty strings, summary ≤ 150 words |
| B. Input guardrail | `src/guardrails.py` | Prompt-injection detector that survives simple evasion (spacing, full-width text, Japanese) |
| C. Parsing real output | `src/guardrails.py` | Strip the ```` ```json ```` fences that LLMs often add |
| D. LLM-as-a-Judge | `src/evaluator.py` | Strict judge schema, injection-resistant judge prompt, real Gemini call |

Every task is marked `TODO (A1)`, `TODO (B2)`, … in the code. **Do not change function names, signatures, or `JUDGE_SAFETY_NOTE`**: the autograder imports them.

### Setup

```bash
pip install -r requirements.txt
cp .env.example .env        # then paste your Gemini API key into .env
pytest -v                   # all tests fail at first; make them pass
```

The tests **do not** need an API key (Part D uses a fake client). The API key is only for the notebooks.

### Notebooks

Open Jupyter **from the repository root** (`jupyter lab` in this folder).

- `notebooks/ex1_structured_guardrails.ipynb`: try your guardrail, then **red-team it**: find at least 2 injections that still get through and write them in the notebook.
- `notebooks/ex2_llm_as_judge.ipynb`: run your judge on real Gemini output and test whether a malicious agent answer can manipulate the score.

### Grading

- Autograded tests (`tests/test_lab5.py`), run on every push.
- Notebook: red-team findings and a short reflection (see the last cell of each notebook).

### Limitation to keep in mind

A regex blocklist can never catch every attack. Part B teaches you **why** deterministic guardrails are useful (fast, cheap, predictable) and **where they break**. Real systems combine them with other layers.

---

## [JA] 課題概要

LLMアプリケーションを取り巻く「信頼性レイヤー」を実装します。

| パート | ファイル | 実装内容 |
|---|---|---|
| A. スキーマ強制 | `src/guardrails.py` | 厳密なPydanticスキーマ（幻覚フィールド禁止・空文字禁止・要約150語以内） |
| B. 入力ガードレール | `src/guardrails.py` | 単純な回避（空白挿入・全角文字・日本語）にも耐えるプロンプトインジェクション検知 |
| C. 実出力の解析 | `src/guardrails.py` | LLMが付けがちな ```` ```json ```` フェンスの除去 |
| D. LLM-as-a-Judge | `src/evaluator.py` | 厳密な評価スキーマ、インジェクション耐性のある評価プロンプト、Gemini呼び出し |

コード中の `TODO (A1)`、`TODO (B2)` などがすべての課題です。**関数名・引数・`JUDGE_SAFETY_NOTE` は変更しないでください**（自動採点で使用します）。

### 準備

```bash
pip install -r requirements.txt
cp .env.example .env        # .env に Gemini APIキーを記入
pytest -v                   # 最初は全テスト失敗 → すべて通るように実装
```

テストに **APIキーは不要** です（パートDは疑似クライアントを使用）。APIキーはノートブックでのみ使います。

### ノートブック

Jupyter は **リポジトリのルートから** 起動してください（このフォルダで `jupyter lab`）。

- `ex1_structured_guardrails.ipynb`：ガードレールを試し、**レッドチーミング**を行う。すり抜ける攻撃を2つ以上見つけて記録する。
- `ex2_llm_as_judge.ipynb`：実際のGeminiで評価器を動かし、悪意ある回答が採点を操作できるか検証する。

### 評価方法

- 自動採点テスト（`tests/test_lab5.py`）：push のたびに実行。
- ノートブック：レッドチーミングの結果と考察（各ノートブック最後のセル）。

### 注意点

正規表現のブロックリストですべての攻撃を防ぐことはできません。パートBでは、決定的ガードレールが **なぜ有用か**（高速・低コスト・予測可能）と **どこで破綻するか** を学びます。実運用では他の防御層と組み合わせます。
