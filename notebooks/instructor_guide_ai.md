# Instructor Guide: AI-Assisted Programming Integration

This guide supports instructors teaching the AI-integrated content woven into the USGS "Python for Hydrology" curriculum. It covers session-by-session teaching notes, discussion prompts, live demo scripts, guidance for mixed skill levels, and time allocation.

All AI examples in the notebooks are pre-captured — no live AI API access is required during class. Instructors may optionally use a live AI tool for demonstrations, but the curriculum works fully without one.

---

## Session-by-Session Teaching Notes

### Monday Morning — AI Foundations (within 1000–1130 "AI overlords" slot)

**Notebook**: `00b_ai_assisted_programming.ipynb`

**Learning Objectives**:
- Students understand what LLM-based coding assistants are and how they generate code
- Students can articulate at least three limitations of AI code generation for scientific work
- Students recognize common failure modes in AI-generated hydrology code

**Key Talking Points**:
- LLMs are pattern-completion engines trained on public code — they have no understanding of physics, units, or domain correctness
- AI tools are strongest at boilerplate, syntax help, and common patterns; weakest at domain-specific logic and numerical correctness
- The three worked examples (unit conversion error, Theis sign error, negative hydraulic conductivity) are real categories of mistakes, not contrived edge cases

**Common Student Questions**:
- *"Which AI tool should I use?"* — The course is tool-agnostic. Concepts apply to Kiro, GitHub Copilot, ChatGPT, Claude, and others. Focus on the workflow, not the product.
- *"Will AI replace hydrologists?"* — No. AI accelerates routine coding tasks but cannot verify physical correctness, design field campaigns, or interpret results in context.
- *"Is it cheating to use AI?"* — Frame this as a professional skill. The goal is knowing when and how to use AI, not avoiding it.

**Timing Guidance**: Spend roughly 13 minutes on AI foundations content within the 90-minute slot. The rest of the slot covers the existing Python basics review and Q&A discussion. Let the "AI overlords" discussion flow naturally — the discussion prompts below can fill 10–15 minutes.

---

### Monday Afternoon — Functions and Scripts (1230–1330)

**Notebook**: `01_functions_scripts.ipynb` (AI Sidebar)

**Learning Objectives**:
- Students see how AI generates function boilerplate and compare it to manual coding
- Students identify missing edge case handling and input validation in AI-generated functions

**Key Talking Points**:
- AI is good at generating function skeletons and docstrings — this is a legitimate time-saver
- AI often omits input validation and edge case handling because training data frequently lacks it
- The verification checklist is a habit to build: run it, check the output, review the logic

**Common Student Questions**:
- *"The AI code looks fine — why do I need to check it?"* — Walk through the "What Could Go Wrong" example. Code that looks correct can silently produce wrong results.
- *"How do I know what edge cases to check?"* — Domain knowledge drives this. AI doesn't know your data ranges or physical constraints.

**Timing Guidance**: The AI sidebar adds roughly 5–8 minutes to the session. Present it after students complete the manual exercise so they have a baseline for comparison.

---

### Tuesday Morning — NumPy (930–1130)

**Notebook**: `05_numpy.ipynb` (AI Sidebar)

**Learning Objectives**:
- Students see AI-assisted array operations and understand broadcasting pitfalls
- Students debug an AI-generated time unit conversion error

**Key Talking Points**:
- AI frequently gets array broadcasting wrong — it may assume shapes that don't match your data
- The debugging exercise (seconds vs. days in recharge calculation) is a realistic mistake that produces plausible but wrong numbers
- Dimensional analysis catches these errors: always check that units cancel correctly

**Common Student Questions**:
- *"The AI output looks numerically reasonable — how do I know it's wrong?"* — Compare against a hand calculation or known value. "Looks reasonable" is not verification.

**Timing Guidance**: ~8 minutes for the sidebar and debugging exercise. Place after the main NumPy exercises.

---

### Tuesday Afternoon — Matplotlib (1230–1430)

**Notebook**: `06_matplotlib.ipynb` (AI Sidebar)

**Learning Objectives**:
- Students use AI to generate plot code and evaluate axis labels, units, and style
- Students understand that AI-generated plots may look professional but contain incorrect labels or units

**Key Talking Points**:
- Plot generation is one of AI's stronger areas — it handles matplotlib syntax well
- The risk is in labels, units, and axis orientation: AI may label an axis "Discharge (cfs)" when your data is in cms
- Always verify axis labels and units against your data source

**Common Student Questions**:
- *"Can I just use AI for all my plots?"* — For routine plots, yes, with verification. For publication figures, you need full control over every element.

**Timing Guidance**: ~5 minutes for the sidebar. Matplotlib sessions are hands-on, so keep the sidebar brief.

---

### Tuesday Afternoon — Theis Exercise (1445–1615)

**Notebook**: `07a_Theis-exercise.ipynb` (AI Sidebar)

**Learning Objectives**:
- Students see AI implement the Theis equation and verify against the known analytical solution
- Students debug AI code that confuses transmissivity (T) with hydraulic conductivity (K)

**Key Talking Points**:
- This is the most important sidebar for demonstrating verification. The known solution (s = 1.40636669 at r=1000m, t=10 days) is the ground truth.
- The T vs. K confusion is a common real-world error — AI training data contains both conventions and the model may mix them
- Walk through the debugging exercise step by step: identify the error, trace it to the parameter definition, fix it, re-verify

**Common Student Questions**:
- *"How did the AI get T and K confused?"* — LLMs don't understand physics. They pattern-match from training data where both symbols appear in similar contexts.
- *"What if I don't know the analytical solution?"* — That's exactly the challenge. For well-known equations, use published solutions. For novel problems, use dimensional analysis and physical reasoning.

**Timing Guidance**: ~8 minutes for the sidebar and debugging exercise. This is a critical teaching moment — don't rush it.

---

### Wednesday Morning — Pandas (900–1000, continued from Tuesday)

**Notebook**: `08_pandas.ipynb` (AI Sidebar)

**Learning Objectives**:
- Students see AI-assisted data manipulation and understand index alignment pitfalls
- Students practice using AI to process NWIS streamflow data

**Key Talking Points**:
- AI handles Pandas syntax well but often gets index alignment wrong, especially with time series
- The groupby logic errors are subtle — AI may group by the wrong column or apply the wrong aggregation
- Always inspect `.head()`, `.shape`, and `.dtypes` after AI-generated transformations

**Common Student Questions**:
- *"The DataFrame looks right — how do I know the groupby is wrong?"* — Check a few values by hand. Pick a group you can verify manually.

**Timing Guidance**: ~5 minutes for the sidebar.

---

### Wednesday Morning — GeoPandas (1000–1300)

**Notebook**: `09_a_Geopandas.ipynb` (AI Sidebar)

**Learning Objectives**:
- Students see AI-assisted spatial operations and understand CRS and spatial join pitfalls
- Students practice using AI to join wells with aquifer boundaries

**Key Talking Points**:
- CRS errors are the most common AI mistake in geospatial code — AI may assume EPSG:4326 when your data is in a projected CRS
- Spatial join type matters: "intersects" vs. "within" vs. "contains" produce different results
- Always plot the result to visually verify spatial operations

**Common Student Questions**:
- *"Why did the spatial join return no results?"* — Almost always a CRS mismatch. Check `.crs` on both GeoDataFrames.

**Timing Guidance**: ~5 minutes for the sidebar.

---

### Wednesday Afternoon — Verification and Testing (1630–1730)

**Notebook**: `12_verifying_ai_code.ipynb`

**Learning Objectives**:
- Students learn three systematic verification strategies: analytical solutions, unit testing, and dimensional analysis
- Students write pytest test functions for Theis equation and Darcy's Law
- Students practice using assertions as a rapid feedback loop

**Key Talking Points**:
- Verification is the skill that makes AI tools safe to use in professional work
- The three strategies complement each other: analytical solutions for known problems, unit tests for regression, dimensional analysis for novel problems
- pytest is not just for software engineers — scientists benefit from automated checks on their calculations

**Common Student Questions**:
- *"Do I need to write tests for everything?"* — No. Focus on critical calculations, unit conversions, and any code that feeds into decisions or publications.
- *"What if I don't have an analytical solution?"* — Use dimensional analysis and physical bounds. If heads are negative or conductivity is 10^12, something is wrong.

**Timing Guidance**: ~20 minutes of AI-specific verification content within the 60-minute code style/testing/version control slot. The remaining 40 minutes cover existing content on code style and version control.

---

### Thursday — FloPy Sidebars (Part 1)

**Notebooks**: `01-Flopy-intro.ipynb`, `02-Building-Post-Processing-MODFLOW6.ipynb`, `03_Loading_and_visualizing_models.ipynb` (AI Sidebars)

**Learning Objectives**:
- Students see AI generate FloPy boilerplate and understand common API confusion
- Students verify AI-generated model output against expected head values
- Students identify deprecated or incorrect FloPy API calls

**Key Talking Points**:
- The `flopy.modflow` vs. `flopy.mf6` confusion is the single most common AI error with FloPy — emphasize this
- AI training data includes old FloPy versions, so generated code may use deprecated patterns
- Model construction exercise: students describe a scenario in plain language, use AI to generate code, then verify the model runs and heads are reasonable

**Common Student Questions**:
- *"The AI used flopy.modflow — is that wrong?"* — It depends. `flopy.modflow` is for MODFLOW-2005. If you're building a MODFLOW 6 model, you need `flopy.mf6`. AI often confuses the two.
- *"How do I know if the API call is deprecated?"* — Check the FloPy documentation. If the AI uses a class name you can't find in the docs, it's likely outdated.

**Timing Guidance**: ~8 minutes per sidebar across three sessions. Keep sidebars focused — Part 1 students are learning FloPy for the first time and need most of the session for core content.

---

### Friday — Project Day

No new AI content is introduced on Friday. Students may use AI tools during the project if they choose. Encourage them to apply the verification checklist and document which parts of their project code were AI-assisted.

---

## Discussion Prompts for "AI Overlords" Segment

Use these prompts during the Monday morning "AI overlords: Q&A and discussion" slot (1000–1130). Select 2–3 prompts based on class interest and time. Each prompt is designed for 3–5 minutes of group discussion.

### Prompt 1: AI Limitations in Scientific Work

> "What tasks in your daily work would benefit most from AI coding assistance? What tasks absolutely require your domain expertise and should never be delegated to AI without careful review?"

**Instructor notes**: Guide students toward recognizing that data formatting, boilerplate code, and plot generation are good AI tasks, while equation selection, parameter estimation, and result interpretation require human expertise. Push back gently if students say "everything" or "nothing."

### Prompt 2: Citation and Attribution

> "If you use an AI tool to generate code that ends up in a USGS publication or data release, how should you cite it? What are the implications for reproducibility if the AI tool gives different code each time you ask?"

**Instructor notes**: There is no settled standard yet. Discuss emerging practices: noting AI tool and version in methods sections, saving prompts alongside code, and the challenge that AI output is non-deterministic. Reference USGS Fundamental Science Practices as the framework for scientific integrity.

### Prompt 3: Data Sensitivity

> "Think about the data you work with — streamflow records, well logs, model inputs, draft publications. Which of these would you be comfortable sending to a cloud-based AI service? Which would you keep local only?"

**Instructor notes**: Distinguish between public data (published NWIS records) and sensitive data (pre-decisional model results, embargoed publications, location data for endangered species habitat). Emphasize that cloud AI services transmit data to external servers. Local tools like Kiro process data on the user's machine.

### Prompt 4: Reproducibility

> "You use an AI tool to write a data processing script today. Six months from now, a colleague needs to reproduce your analysis. The AI tool has been updated and gives different code for the same prompt. How do you handle this?"

**Instructor notes**: Key strategies: version-control the generated code (not just the prompt), include the AI tool version in your documentation, write tests that verify the output regardless of how the code was generated. The code itself is the reproducible artifact, not the prompt.

---

## Live Demo Scripts

These demos are designed for the Monday morning session but can be adapted for other slots. Each demo takes 5–7 minutes. Pre-captured outputs are available in the notebooks if live AI access is unavailable.

### Demo 1: Well Capture Zone Calculation

**Setup**: Open a code editor or Jupyter notebook with a live AI assistant (or use the pre-captured example from `00b_ai_assisted_programming.ipynb`).

**Script**:

1. **Describe the task to the class** (30 seconds):
   "Let's ask an AI to write a function that calculates the capture zone of a pumping well using the Grubb (1993) analytical solution. This is a standard calculation many of you have done by hand."

2. **Write the prompt** (1 minute):
   ```
   Write a Python function that calculates the capture zone width of a pumping
   well using the Grubb (1993) analytical solution. The function should take
   pumping rate Q (m³/day), aquifer thickness b (m), and hydraulic gradient i
   (dimensionless) as inputs and return the maximum capture zone half-width
   in meters.
   ```

3. **Show the AI output** (1 minute):
   Display the generated code. Point out what looks correct (function structure, docstring, return type) and what needs checking (the formula itself, parameter handling).

4. **Reveal the error** (2 minutes):
   The AI will likely produce a formula that is close but not exactly right. Common errors:
   - Using `Q / (2 * pi * b * K * i)` instead of the correct `Q / (2 * b * i * v)` where v is the Darcy velocity
   - Confusing hydraulic conductivity K with Darcy velocity v
   - Missing the pi factor or placing it incorrectly

   Walk through the correction:
   - "The AI confused Darcy velocity with hydraulic conductivity — a classic domain error"
   - "The formula structure is right, but the physics is wrong"
   - "This is why we verify against known solutions"

5. **Show the fix and verify** (1 minute):
   Correct the formula, run it with known parameters, and compare against a hand-calculated value.

**Key takeaway**: "The AI got the code structure right and saved us time on boilerplate, but it got the physics wrong. This is the pattern you'll see throughout the week — AI is a fast first draft, not a final answer."

---

### Demo 2: FloPy Model from Description

**Setup**: Open a Jupyter notebook. This demo works best in the Thursday FloPy session but can be previewed on Monday.

**Script**:

1. **Describe the scenario to the class** (30 seconds):
   "Let's ask an AI to build a simple MODFLOW 6 model from a plain-language description. This is something you might do when starting a new project."

2. **Write the prompt** (1 minute):
   ```
   Using FloPy, create a simple confined aquifer MODFLOW 6 model with:
   - 10 rows, 10 columns, 1 layer
   - Cell size 100m x 100m
   - Constant head of 20m on the left boundary, 10m on the right boundary
   - Hydraulic conductivity of 10 m/day
   - Single steady-state stress period
   Write the complete code to create, run, and plot the head results.
   ```

3. **Show the AI output** (1 minute):
   Display the generated code. It will likely be 30–50 lines covering simulation setup, model creation, packages, and plotting.

4. **Identify the errors** (2 minutes):
   Common AI errors with FloPy:
   - Using `flopy.modflow.Modflow()` (MODFLOW-2005) instead of `flopy.mf6` classes
   - Wrong class names: `ModflowGwfdis` instead of `ModflowGwfDis` (capitalization matters)
   - Incorrect stress period data format (list of tuples vs. dictionary)
   - Missing required packages (e.g., forgetting the OC package for output control)

   Walk through each error:
   - "See this line? The AI used the MODFLOW-2005 API. We need MODFLOW 6."
   - "This class name is wrong — FloPy is case-sensitive and the AI guessed wrong"
   - "The stress period data format changed between FloPy versions — the AI used an old pattern"

5. **Debug and fix** (2 minutes):
   Show the debugging workflow:
   - Run the code, read the error message
   - Check the FloPy documentation for the correct class name
   - Fix the import and class references
   - Re-run and verify heads are physically reasonable (between 10m and 20m)

**Key takeaway**: "AI can scaffold a FloPy model quickly, but it frequently confuses API versions and class names. The debugging workflow — run, read the error, check the docs, fix — is the skill that makes AI useful for FloPy work."

---

## Managing Mixed Skill Levels

The class typically includes students ranging from Python beginners to experienced programmers. Here is how to handle AI content across skill levels.

### Beginners (Limited Python Experience)

- **Priority**: Focus on the manual exercises first. AI sidebars are a "preview" of what's possible, not a replacement for learning fundamentals.
- **Guidance**: Tell beginners: "Work through the main exercise first. When you're done, read through the AI sidebar to see how the same task could be approached with AI assistance. Don't worry about the AI exercises until you're comfortable with the manual approach."
- **Risk to watch for**: Beginners may want to skip manual coding and jump straight to AI. Redirect them: "Understanding the fundamentals is what lets you evaluate whether AI output is correct."

### Advanced Students (Comfortable with Python)

- **Priority**: Encourage them to attempt the AI exercise variants after completing the manual exercises.
- **Guidance**: Tell advanced students: "Try the AI-assisted variant and compare your experience. How does the prompt you'd write differ from the manual approach? Can you improve on the AI's output?"
- **Extra challenge**: Point them to the prompt engineering notebook (`00c_prompt_engineering.ipynb`) for self-study during breaks.
- **Role in class**: Advanced students can help beginners debug AI-generated code — this reinforces their own verification skills.

### All Students

- **Verification exercises are for everyone**. Regardless of skill level, every student should participate in the verification and testing content (Wednesday afternoon). This is the most transferable skill in the AI integration.
- **Discussion participation**: The "AI overlords" discussion benefits from diverse perspectives. Beginners bring fresh questions; advanced students bring practical experience. Encourage both.
- **Capstone flexibility**: In the capstone exercise, students self-select their balance of manual vs. AI-assisted work. There is no wrong ratio — the goal is thoughtful annotation of their choices.

---

## Time Allocation Table

The table below shows AI content time per session, confirming that AI material stays within 15% of each session's total time. This preserves the core Python and FloPy curriculum while integrating AI content meaningfully.

| Day | Session | Time Slot | Total Minutes | AI Content (min) | AI % | AI Content Description |
|-----|---------|-----------|---------------|-------------------|------|------------------------|
| Mon | AM | 1000–1130 | 90 | 13 | 14.4% | AI Foundations notebook within "AI overlords" slot |
| Mon | PM | 1230–1730 | 300 | 8 | 2.7% | AI Sidebar in 01 Functions/Scripts |
| Tue | AM | 900–1130 | 150 | 8 | 5.3% | AI Sidebar in 05 NumPy |
| Tue | PM | 1230–1730 | 300 | 13 | 4.3% | AI Sidebars in 06 Matplotlib, 07a Theis |
| Wed | AM | 900–1300 | 240 | 10 | 4.2% | AI Sidebars in 08 Pandas, 09a GeoPandas |
| Wed | PM | 1300–1730 | 270 | 20 | 7.4% | AI Sidebar in 10 Rasterio slot area + Verification notebook in testing slot |
| Thu | AM+PM | 900–1730 | 510 | 24 | 4.7% | AI Sidebars in FloPy 01, 02, 03 |
| Fri | AM+PM | 900–1700 | 480 | 0 | 0.0% | Project day — no new AI content |

**Notes**:
- The prompt engineering notebook (`00c_prompt_engineering.ipynb`) is self-study and not included in session time
- The capstone notebook (`13_ai_capstone.ipynb`) is optional/self-study and not counted against session time
- All percentages are well within the 15% maximum per session
- Sidebar times (5–8 min each) are estimates; instructors should adjust based on class pace and engagement
- If a session runs long on core content, AI sidebars can be shortened or assigned as self-review
