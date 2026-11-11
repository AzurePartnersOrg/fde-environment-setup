# FDE Course: Environment Setup

This page is the hands-on guide: install the tools, get the project, check your machine, and run a course yourself. To see what a finished run looks like first, with screenshots, the real output files, and where the time and tokens went, read [page 03](https://github.com/AzurePartnersOrg/fde-learning-kit/blob/main/ai-course-factory/03-system-demo/README.md). This page does not repeat that walkthrough.

## Course Factory Lite: how to use it

You type one request. Claude builds a reviewed course package (syllabus, slides, quiz, lab, QA report) as markdown and PDF files. You approve it at three gates by replying in plain words.

## Part 1. Set up (once)

**Step 1. Install four tools.**

| Tool | Get it |
| --- | --- |
| Git | https://git-scm.com/downloads |
| uv | Commands below |
| Google Chrome | https://www.google.com/chrome (it prints the PDFs) |
| Claude Code | https://docs.claude.com/en/docs/claude-code/overview. Run `claude` once and sign in. |

Install uv on Windows:

```
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
```

Install uv on Mac or Linux:

```
curl -LsSf https://astral.sh/uv/install.sh | sh
```

Python 3.9 or newer must also be on your path.

**Step 2. Get the project.** The project lives in the `AI-course-factory-reference` repo, the same
one the Page 05 challenge task starts from. Ask the project owner to add your GitHub account, then
run:

```
git clone https://github.com/AzurePartnersOrg/AI-course-factory-reference.git
cd AI-course-factory-reference
```

<div style="page-break-before: always"></div>

**Step 3. Check your machine.**

```
powershell -File scripts/readiness-check.ps1     # Windows
bash scripts/readiness-check.sh                  # Mac or Linux
```

You should see five `PASS` lines. Fix any `FAIL` before going on.

## Part 2. Make a course

This part is a tour of the full pipeline. The Page 05 tasks do not need the course you make here:
both start from the sample course that ships in the repo, `intro-python-lists-hs`. Short on time?
Do Part 1, then go to Page 05. The tasks need the same tools.

**Step 4. Start Claude in the project folder.** Open a terminal in `AI-course-factory-reference` and type:

```
claude
```

If it asks whether you trust the folder, choose yes. Always start Claude from this folder, because the agents and commands load from it.

**Step 5. Start a course.** Type the command with your topic, level and number of modules:

```
/new-course Intro to SQL, level: high school, modules: 1
```

- Modules can be 1 to 4. The default is 2.
- You can add `capstone: ...` if you already know what learners should build.
- Claude researches the topic. This takes several minutes and uses a lot of tokens, so a one-module course is the best first test.

**Step 6. Gate 0, viability.** Claude stops and asks whether the course is worth building. Open the PDF it names:

`courses/<slug>/dist/00-viability/viability-brief.pdf`

Reply in plain words:

- To approve, type `yes` or `good to go`.
- To change something, type what you want changed. Claude revises and asks again.

A reply with "but" or a question in it is not counted as approval. Answer with a plain `yes`, or give the changes.

**Step 7. Gate 1, syllabus.** Claude writes the syllabus and stops again. Open:

`courses/<slug>/dist/01-syllabus/syllabus.pdf`

Reply `yes` to approve, or type your changes.

**Step 8. Wait while the package is built.** Claude writes the slides, quiz and lab at the same time, then runs the checks and a QA review. Fixes happen automatically. You do not need to do anything.

**Step 9. Gate 2, the finished package.** Claude lists any open items, such as "the lab was written, not run". Read the PDFs in:

`courses/<slug>/dist/`

Reply `yes` to publish, or type your changes.

**Step 10. Collect your files.** The course is now published. Everything is under `courses/<slug>/`:

| What | Where |
| --- | --- |
| Viability brief and sources | `dist/00-viability/` |
| Syllabus | `dist/01-syllabus/` |
| Slides: student and instructor versions | `dist/02-deck/` |
| Quiz (student) and answer key (instructor) | `dist/03-assessments/` |
| Lab guide | `dist/04-labs/` |
| QA report | `dist/qa/` |
| Your approvals, word for word | `dist/APPROVALS.pdf` |

The markdown files (`.md`) in the folders next to `dist/` are the originals. To change a file, edit the markdown, then type `/render <slug>`. Do not edit the PDF.

**Step 11. Run the lab yourself before you teach it.** Lab steps are written, not run, so nothing in them has been tested.

## Part 3. After publishing

| To do this | Type |
| --- | --- |
| Ask the teaching assistant a student question | `/ask-ta <slug> <question>` |
| Get a summary of student questions, without names | `/ta-digest <slug>` |
| See where every course stands | `/status` |
| Carry on after closing Claude | start `claude` again, then `/continue` |
| Rebuild the PDFs | `/render <slug>` |
| Rerun the checks and QA review | `/qa <slug>` |

The slug is the course folder name, for example `intro-sql-hs`.

## If something goes wrong

| You see | Do this |
| --- | --- |
| `Agent type '...' not found`, or the `/` commands are missing | Quit Claude. Start it again from the project folder. |
| `Chrome not found` | Set the `CHROME_PATH` variable to your Chrome program. |
| The first PDF takes minutes | Wait. This happens once while uv downloads tools. |
| `PDF-STALE` in the checks | Type `/render <slug>`. |
| A gate reply is refused as hedged | Reply with a plain `yes`, or give the changes you want. |
| Claude says it is stuck after three fix rounds | Read what it lists as blocked and tell it how to proceed. |

---

**Previous:** [03 System demo and walkthrough](https://github.com/AzurePartnersOrg/fde-learning-kit/blob/main/ai-course-factory/03-system-demo/README.md) · **Next:** [05 Hands-on practice](https://github.com/AzurePartnersOrg/fde-learning-kit/blob/main/ai-course-factory/05-hands-on-practice/README.md)
