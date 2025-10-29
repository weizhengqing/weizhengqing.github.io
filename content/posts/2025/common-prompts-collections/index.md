+++
title = "Prompts"
date = 2025-08-18
description = "Common Prompts Collection"
slug = "common-prompts-collections"

[extra]
toc = true

[taxonomies]
tags = ["prompt"]
categories = ["AI"]
+++

This article is a collection of commonly used prompts.

<!-- more --> 

## Writing Articles
```
Please translate the provided text, then rephrase it in a formal academic/scientific writing style. Finally, convert the rephrased version into properly formatted LaTeX code. Only output the final LaTeX result – don't include any additional explanations or the intermediate steps.
```
```
Convert the above given text into LaTeX code only. Output only the LaTeX representation of my text, without adding any extra content (no document preamble, packages, or begin/end statements). Keep the meaning unchanged, just format it properly in LaTeX.
```

### BibTeX Entry Formatting
```
You are a professional academic literature assistant with expertise in LaTeX syntax and BibTeX specifications. Please correct the provided BibTeX entry according to the following rules:
1. Title Formatting  
   - Capitalization: Preserve the original case formatting of titles (e.g., capitalize proper nouns), without forcing all uppercase or lowercase.  
   - Chemical formulas: Write chemical formulas correctly using LaTeX syntax.  
   - Superscripts and subscripts:  
     - Superscripts: Wrap with `\textsuperscript{}` or `^{}`.
     - Subscripts: Wrap with `\textsubscript{}` or `_{}`.
   - Units: Follow `siunitx` conventions.  
2. Author Names  
   - Format as `Last, First` or `Last, F. M.`, retaining the `and` connector.  
   - Protect special characters (e.g., `Özgen` → `{\"O}zgen`).  
3. Journal/Conference Names  
   - Abbreviate according to standard conventions (e.g., `Journal of Chemical Physics` → `J. Chem. Phys.`); keep full names for unknown titles.  
   - Mark journal names with `\textit{}` (e.g., `\textit{Physical Review Letters}`).  
4. Special Characters and Escaping  
   - Latin characters (e.g., `é` → `{\'e}`), mathematical symbols (e.g., `α` → `$\alpha$`).  
   - Escape LaTeX special characters such as `&`, `%`, `$` (e.g., `%` → `\%`).  
5. Dates and Page Numbers  
   - Date format: `YYYY-MM-DD` (e.g., `2024-01-15`).  
   - Page ranges: Connect with `--` (e.g., `pages = {123--130}`).  
6. Protecting Uppercase Letters  
   - Wrap words that must retain capitalization with `{}` (e.g., `{NASA}`).  
Output Requirements:  
- Return the complete corrected BibTeX entry without additional explanations.  
- If fields are missing or formatting is unrecognizable, retain the original content and add a comment `% NOTE: Check this field`.
```

## Image to Text Conversion
```
Please perform the following tasks:
1. Conduct OCR text recognition on the provided image to accurately extract all textual content.
2. Retain all body text and mathematical formulas, ignoring only the formatting of non-body content such as document titles, section headings, etc. (treat bold, italic, enlarged font sizes, etc., as plain text).
3. Identify all mathematical formulas and convert them into standard LaTeX format, strictly preserving their original formatting:
   - Mathematical variables in italics (e.g., $x$, $F = ma$)
   - Units in upright font (e.g., $\mathrm{m/s}$, $\Omega$)
   - Standard function names in upright font (e.g., $\sin$, $\log$, $\exp$)
   - Constants and special symbols in upright font (e.g., $\pi$, $\mathrm{e}$, $\mathrm{i}$)
4. Output non-mathematical text in the body (paragraphs, sentences, etc.) as plain text without any LaTeX formatting commands.
5. Preserve the original numbering after each formula (e.g., (1), (2.3), etc.), ensuring exact correspondence with their positions in the image.
6. Output only clean LaTeX code content without any additional document class declarations, package loading, begin/end{document}, or other extraneous code.
7. Formatting requirements:
   - Body text should be output directly.
   - Formulas must be wrapped exclusively as follows:
     - If a formula takes up a single line (displayed), wrap it with `$$...$$` (example: $$Formula$$).
     - If a formula is in the body text (inline), wrap it with `$...$` (example: $Formula$).
   - The use of \[...\] for wrapping formulas is strictly prohibited and must never appear, regardless of how complex or lengthy the task is.
   - The output must only contain the converted LaTeX code, with no explanatory text.

Output only the converted LaTeX code directly, without any explanatory text. The output of the answer must ne wrapped in a code box.
```

## Literature Search
### Topic Research
```
You are an expert deep researcher with unlimited computational resources. Your goal is to conduct the most thorough, in-depth research on the above provided topic, statement or questions. Maximize your use of computing power by performing extensive searches, analyzing multiple layers of information, and citing the maximum number of highly relevant sources.

Step-by-step process:
1. Break down the topic into 5-10 key sub-questions or angles (e.g., historical context, current trends, future implications, counterarguments).
2. Use all available tools: Perform web searches with advanced operators (e.g., site:edu for academic sources, filetype:pdf for reports). Crucially, you must prioritize searching for and summarizing peer-reviewed academic papers from SCI/SSCI indexed journals or top-tier publishers (including but not limited to APS, IEEE, Elsevier, Wiley, Springer, Nature, Science, and PNAS).
3. For each sub-question, retrieve and analyze at least 5-10 sources. Strictly avoid blogs, non-academic sources, or low-quality articles. Cross-verify facts, identify biases, and include diverse perspectives (e.g., from experts, critics, and data-driven studies) found within the academic literature.
4. Dive deep: If a source mentions related concepts, follow up with additional searches to explore them exhaustively within academic databases and publisher sites.
5. Maximize citations: Include direct quotes, summaries, and links from the most relevant sources. Prioritize high-quality, peer-reviewed, and authoritative academic papers above all other source types.

Output format:
- Executive summary: A concise overview of key findings based primarily on the academic literature.
- Detailed analysis: Sectioned by sub-questions, with in-depth explanations grounded in peer-reviewed research.
- Sources table: List all cited sources with title, author, link, relevance score (1-10), and a brief excerpt. The vast majority of sources should be from SCI/SSCI indexed journals or the specified top publishers.
- Potential gaps: Note any unanswered areas within the existing academic literature and suggest further research.

Ensure the research is comprehensive, unbiased, and backed by evidence from high-quality academic sources. If needed, perform multiple iterations to deepen the results.
```
### Statement Verification
```
I have provided you above with a statement. Your task is to verify whether this statement is correct by performing a literature search. If the statement is correct, provide supporting evidence from peer-reviewed sources, including direct citations (with author, year, and title/journal). If the statement is not correct, explain what the scientific literature actually says, again with proper citations. Always include at least 2–3 references, and ensure the citations are specific and verifiable.
```

### Audio Transcription
```
Extract the full transcript from the provided audio file and output only the raw text content inside a markdown code block. Do not include any summaries, translations, interpretations, or additional commentary. Preserve the exact wording, including fillers, pauses, or errors if present in the audio. Output nothing but the transcribed text in a code block.
```


## Translation
### English
```
Please translate the above provided text into English while adhering to the following requirements:
- Maintain professional accuracy while employing natural native English expressions
- Adopt the stylistic conventions of academic writing
- Pay special attention to the following challenges:
  - Idioms/proverbs → Substitute with equivalent English idioms
  - Culture-specific references → Provide appropriately localized explanations
  - Restructure sentence patterns according to English conventions to avoid Chinglish
  - Ensure terminological consistency.
```

## Learning German

### Grammar Explanation
```
You are an experienced German language teacher. Please help me explain all grammar points in the sentence (or utterance) provided above in detail. Please categorize by grammatical structure, tense, syntactic components, key vocabulary explanation, and clause types, and illustrate their usage with examples. Please answer in clear and easy-to-understand language.
```

### Word Formation and Etymology Analysis
```
Please analyze the word formation of the word provided above, explain the etymology, and provide its synonyms and antonyms.
```

## Communication
### Nonviolent Communication
```
You are a "Nonviolent Communication (NVC) Coach."  
Please use the four core steps of NVC: **Observation, Feeling, Need, Request** to help me better understand and express my feelings and needs.  
Task Description:  
1. Based on the specific situation, dialogue, or problem encountered in daily life provided above.  
2. You need to unfold according to the four NVC steps in sequence:  
   - Observation: Objectively describe what happened without evaluation or judgment.  
   - Feeling: Identify the speaker's current emotions or emotional state.  
   - Need: Point out the internal needs or values that cause these feelings.  
   - Request: Propose a clear, positive, and actionable request to help improve communication or meet needs.  
3. After the analysis, please provide suggestions or example sentences on how to express your feelings and needs more gently and clearly.  
Output Format Example:  
Observation: ……  
Feeling: ……  
Need: ……  
Request: ……  
Suggested Expression: …… 
```

### High-Conflict Communication
```
You are an expert in High-Conflict Communication and Emotional Intelligence.  
Your task is to: analyze the conflict structure, underlying emotions, and core needs within the communication scenario I provide,  
and guide me on how to express myself more clearly while reducing defensiveness and misunderstanding.

Please analyze and respond following these steps:
1. Situational Understanding  
   - Briefly restate the situation I have provided  
   - Clarify the roles and relationships of all parties involved (e.g., partners, friends, colleagues, etc.)
2. Conflict Signal Identification
   - Point out signals in the dialogue that may lead to high conflict (such as defensiveness, blame, competitive reasoning, or stonewalling)  
   - Analyze the psychological dynamics or emotional motivations behind these signals
3. Emotion and Need Analysis  
   - Help me identify the primary emotions I am genuinely experiencing (e.g., feeling ignored, anxious, angry, or longing to be understood)  
   - Infer the core needs underlying these emotions (e.g., being valued, being understood, sense of security)
4. Expression Optimization Suggestions  
   - Provide phrasing templates I can use to express my inner state more calmly and clearly (avoiding blame)  
   - Example phrases should align with high-conflict communication principles: less defensiveness, less projection, more self-awareness and need-based statements
5. Interaction Guidance  
   - Suggest how I might respond or what questions I could ask next to foster understanding and cooperation  
   - If the other party remains defensive, provide de-escalation strategies

Output Format:
- Present in logically structured sections
- Use a calm, neutral, and empathetic tone
- Base all conclusions on principles from psychology and communication studies, not subjective judgment
```

## Blog Editing
````
---
以上为输入内容
- 选项要求："省略目录"、"翻译成地道规范的英式英文"
- 期望的标签和分类建议（如无提供，请根据内容合理推断）

你是一位专业的博客作家，擅长将内容转化为结构清晰、通俗易懂的博客文章。
你的任务是：根据上面或文档提供的内容，将其重新组织并撰写一篇符合以下格式的博客文章。

输出格式要求：
1. 必须严格使用以下 Front Matter 模板（包含博客元数据）：

```markdown
+++
title = "填写博客标题"
date = "2025-09-30"    # 修改为此刻日期
description = "填写博客的简短描述"
slug = "填写用于URL的简短slug"
[extra]
toc = true  # 根据选项要求进行修改
[taxonomies]
tags = ["标签1", "标签2", "标签3"]
categories = ["分类1", "分类2"]
+++

博客开头简介（2-3句话，会显示在博客列表页预览中）。这段内容应吸引读者，概括全文主旨。

<!-- more -->

正文内容（使用规范的Markdown语法）
```

2. 正文要求：
   - 结构清晰，使用恰当的标题（H2/H3/H4）
   - 技术内容准确，表达通俗易懂；如果是文献调研，禁止省略或改动原文论述。
   - 段落分明，逻辑连贯；如有步骤，使用有序列表。
   - 代码块使用正确语法高亮

可选功能：
请在处理时确认选项要求（我会在输入中明确指定）：
1. 目录控制：如需要省略目录，请在 Front Matter 中移除或注释 `[extra]` 部分
2. 语言翻译：如需要全文翻译为地道英文，请将整篇博客（包括Front Matter和正文）转换为英文。如果未说明，则保持原文语言。

检查文件和目录结构，并命名文档：
1. 根据内容生成合适的目录名称，建议格式：`YYYY-MM-DD-slug.md`
2. 确认博客文档名称为`index.md`，存储于该博客目录中。
````
