---
name: poem_word_assistant
description: Generate a Chinese poetry Word document from poem titles, including LLM poem lookup, cross-checking, validation, and Word ruby pinyin generation.
metadata:
  openclaw:
    os: ["linux"]
    requires:
      bins: ["python3"]
---

# Poem Word Assistant

Use this skill when the user asks to generate a Word document from Chinese poem titles.

## Strong trigger phrases

Use this skill when the user message contains any of:

- 生成诗词word
- 生成古诗word
- 诗词word
- 古诗word
- 唐诗词word
- 把某几首诗生成word
- 根据诗名生成word

Examples:

- 生成诗词word：静夜思，登鹳雀楼
- 请生成word，诗名：春晓、咏鹅
- 帮我把静夜思和登鹳雀楼生成word

## DO NOT use other document skills

For the trigger phrases above, DO NOT use Tencent Docs or general document-editing skills.

This workflow is local Word generation, not Tencent Docs editing.

## Workflow

1. Extract poem titles from the user message.
2. Use the configured LLM to generate poem JSON.
3. Use the configured LLM again to cross-check and correct the poem JSON.
4. Run the Python script to:
   - validate JSON structure
   - validate poem fields
   - generate pinyin automatically
   - generate a `.docx` file using Word ruby / phonetic guide
5. Return the generated `.docx` path.

## Input to Python

After LLM generation and cross-checking, call:

```bash
/root/.openclaw/venvs/poem_skill/bin/python \
/root/.openclaw/workspace/skills/poem_word_assistant/poem_word_assistant.py \
--poems '<JSON>' \
--output outputs/poems/<safe_filename>.docx

## Required JSON schema

The JSON passed to Python MUST be:
[
  {
    "title": "静夜思",
    "dynasty": "唐",
    "author": "李白",
    "lines": [
      "床前明月光，疑是地上霜。",
      "举头望明月，低头思故乡。"
    ]
  },
  {
    "title": "登鹳雀楼",
    "dynasty": "唐",
    "author": "王之涣",
    "lines": [
      "白日依山尽，黄河入海流。",
      "欲穷千里目，更上一层楼。"
    ]
  }
]

## LLM first-pass prompt
When poem titles are extracted, ask the LLM to produce strict JSON.

Rules:

 - Return JSON only
 - No markdown
 - No explanation
 - No pinyin
 - At least 2 poems
 - Use standard textbook-style Chinese punctuation
 - Include complete poem content
 - Schema must be exactly:
 - title
 - dynasty
 - author
 - lines

## LLM cross-check prompt
After first-pass JSON is generated, ask the LLM again to verify and correct it.

The second-pass task is verification, not free generation.

Checklist:
 - Title matches the requested title
 - Dynasty is correct
 - Author is correct
 - Lines are complete
 - Line order is correct
 - No duplicated lines
 - No explanation text mixed into poem lines
 - Chinese punctuation is normal

Output rules:
 - Return corrected JSON only
 - No markdown
 - No explanation
 - No pinyin
 - If first-pass JSON is already correct, return it unchanged

## Word format
First line:
 - Text: 跑团少儿晨读会
 - Font: 华文琥珀 / STHupo
 - Size: 28 pt
 - Center aligned
 - No pinyin

Each poem:
Title:

 - 楷体 / KaiTi
 - 20 pt
 - Center aligned
 - Word ruby phonetic guide

Dynasty + Author:
 - Format: 【朝代】作者
 - 楷体 / KaiTi
 - 20 pt
 - Center aligned
 - Word ruby phonetic guide

Body:
 - One paragraph per poem line
 - 楷体 / KaiTi
 - 20 pt
 - Center aligned
 - Word ruby phonetic guide
 - Pinyin must appear above corresponding Chinese characters
 - Do NOT use inline pinyin like 汉 (han)

Between poems:
 - Exactly 2 blank lines

## Python validation rules

The Python script must reject:
 - invalid JSON
 - fewer than 2 poems
 - missing title/dynasty/author/lines
 - duplicated poem titles
 - empty poem lines
 - suspicious explanatory text
 - unsupported characters
 - duplicated poem lines

## Output
Return only the generated .docx file path after success.

 

