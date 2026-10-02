# Writing posts

Rules for drafting, editing, translating and checking posts in `content/`. Site setup, series mechanics and commands are in [README.md](README.md).

## Voice

These rules apply to post content only. Code, comments in site files and repository docs follow the usual conventions.

- Friendly, storytelling tone. A post may open with a personal experience, a problem the author ran into, or an everyday analogy, while staying technically precise.
- Write like a blog, not a textbook: conversational, talk to the reader, and lead with an example or a story before the explanation. Introduce a term after the reader has seen the problem it solves. Avoid tables and long lists where a few paragraphs would read more naturally.
- Vietnamese posts use "mình" for the author and "bạn" for the reader.
- English posts use "I" and "you".
- Keep technical terms in English (function, deploy, commit, container, ...). Use Vietnamese only where the Vietnamese word is already standard (biến, vòng lặp, hàm). Do not add English glosses in parentheses for terms that are left in English.

## Avoid

- Exaggeration and marketing words: "cực kỳ mạnh mẽ", "tuyệt vời", "thay đổi cuộc chơi", "powerful", "game-changing", "seamless".
- The word "vỡ" for something that was attacked, failed or found vulnerable; say what actually happened instead (bị khai thác, có lỗi, lộ ra).
- Marks of AI-generated prose:
  - generic openings ("Trong thế giới công nghệ ngày nay...", "In today's fast-paced world...");
  - lists padded to three items for rhythm;
  - closing lines such as "Hy vọng bài viết hữu ích với bạn" or "Happy coding!";
  - summarizing a paragraph right after writing it;
  - bold used for emphasis on every other sentence.

## Audience

The audience varies per post. Every post states near the top who it is for and what the reader needs to know or have installed. Pitch the depth of explanation to that stated audience: explain basics for beginners, skip them for experienced readers.

## Structure

Each post contains, in this order:

1. **Opening**: the context or problem that motivates the post.
2. **Audience and prerequisites**: who the post is for, required knowledge, tools and versions.
3. **Body**: sections with `##` headings (the table of contents starts at level 2).
4. **Summary**: the main points to remember.
5. **References**: links to the sources used.

There is no length limit. Long posts rely on the table of contents; a topic that splits into independent parts becomes a series (see README).

## Code

- Every code block must run as shown. State language and library versions in the prerequisites.
- Run Python code on Python 3.14 (the version the Python series uses) and state that version in the prerequisites. Without Docker, `uv python install 3.14` provides an interpreter.
- Put the output of a command or program right after the code, in its own block (use `text` as the language).
- Comments inside code blocks follow the post language: Vietnamese in `index.vi.md`, English in `index.en.md`.
- Always set the language on fenced code blocks.

## Diagrams

- Use Mermaid for diagrams (flows, sequences, architecture, state, relationships) when a picture explains faster than text. Write it as a fenced block with the language `mermaid`; the theme renders it and follows light and dark mode. Do not use ASCII art or screenshots for diagrams that Mermaid can draw.
- Do not add a diagram when a sentence or a short list says the same thing.
- Keep each diagram small enough to read at phone width (roughly 8 nodes, `flowchart LR` or `TD` as fits). Write node labels in the post language and keep technical terms in English.
- Introduce the diagram in the sentence before it. The "must run as shown" and "output in its own block" rules for code do not apply to Mermaid blocks, but the syntax must render without errors.

## Bilingual posts

- `index.vi.md` is written first. `index.en.md` has the same content, sections, code and images, but is rewritten the way a native English writer would put it, not translated sentence by sentence.
- Tags follow the post language: Vietnamese tags in `index.vi.md` (e.g. `"cơ sở dữ liệu"`), English tags in `index.en.md` (e.g. `"database"`). The same applies to `series`.
- `series_weight`, `date` and `draft` are the same in both files.
- Images and other files live in the post directory (page bundle) and are shared by both languages.

## Claude's tasks

- **Draft from an outline**: write a complete `index.vi.md` from the outline given, following the rules above, with `draft = true`. Do not invent personal experiences, opinions or results. Where the voice calls for a personal story that the outline does not provide, leave `<!-- TODO: ... -->` describing what is needed.
- **Edit a post**: fix errors and apply the rules above while keeping the author's voice and wording where it already complies. Do not rewrite passages that need no change. List substantive changes in the reply.
- **Translate vi → en**: create or update `index.en.md` from `index.vi.md` as described under "Bilingual posts". When `index.vi.md` changes, update `index.en.md` to match.
- **Technical check**: render every Mermaid diagram and run every code block (in Docker when a runtime is needed) and compare with the stated output; check that facts, versions and reference links are correct and reachable. Report problems instead of silently changing the content.
- Never set `draft = false`; publishing is the author's decision.
