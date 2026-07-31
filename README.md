<div align="center">

# Seedance 2.5 Prompt.skill
A video prompt Skill built specifically for **Seedance 2.5**.<br>
**Turn an idea / a fragment of a theme / a novel excerpt into a polished 30-second film.**

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)[![Skill](https://img.shields.io/badge/Format-Skill.md-purple)](./SKILL.md)[![Seedance](https://img.shields.io/badge/Target-Seedance%202.5-success)](https://www.seedance.ai)[![Bilingual](https://img.shields.io/badge/Lang-ZH%20%2F%20EN-orange)](./references/templates.md)

<p align="center">
  <a href="./README.md" style="display:inline-block;padding:6px 18px;background:#4f46e5;color:#fff;border-radius:6px 0 0 6px;text-decoration:none;font-weight:600;">English</a>  <a href="./README.zh.md" style="display:inline-block;padding:6px 18px;background:#f3f4f6;color:#4b5563;border-radius:0 6px 6px 0;text-decoration:none;font-weight:600;">简体中文</a>
</p>

</div>

<p align="center">
  <img src="public/海报.jpg" width="100%" alt="Poster" />
</p>

---

## 🌟 Why This Skill?

Writing AI video prompts is a path paved with traps.

It took a long time of stubborn refining before these things became clear:

**1.** Seedance isn't picky about prompts, but it still helps to spell out the **subject, scene, music, and shots** clearly.
- **Subject**: specify exactly which reference image; the name must stay consistent throughout.
- **Scene**: uploading one scene image is enough.
- **Music**: strictly prompt "do not generate any BGM," only sound effects — otherwise post-production editing becomes a nightmare.
- **Shots**: timestamps depend on the content, but the shot numbers must be clearly written, e.g. Shot 01, 02.

**2.** Don't blindly chase resolution. **720P follows prompts best**, 1080P is next, 4K is mediocre.

**3. The core is still picture control.** You need concrete storyboards, framing, transitions, action, camera moves, and plot in your head — good pictures make good stories.

**4.** If you write a prompt you wouldn't even read yourself, don't expect Seedance to produce a high-quality video from it.

These insights hardened into rules, templates, and checklists, and so this Skill was born. **Give it any text — a novel excerpt, a one-line plot, a few theme words — and it returns a multi-shot prompt you can paste straight into Seedance 2.5, along with all the text-to-image prompts for the asset images you need.**

> If you've been tormented by AI video pacing, consistency, and camera work — this Skill is for you.

---

## ✨ What It Does

### 🎬 Direct-to-Seedance 2.5 Prompts

| Input Type | How It's Handled |
|------------|------------------|
| Novel / script / story excerpt | Distills the most visual story beats into consecutive shots |
| A single line of plot | Fills in cause, effect, and turning points, then breaks into shots |
| A few theme words | Creatively expands into a short film with a full arc, then breaks into shots |

Whatever the input, the output is always the same structure: **【Script Overview】+【Asset Image T2I Prompts】+【Video Prompt】**, ready to copy as a single block.

### 🎯 Default Deliverables

- **Total duration 20–30s** (30s hard cap, auto-collapsed if exceeded)
- **Shot count decided by story rhythm** — no fixed tiers (30s typically lands at 8–14 shots)
- **Every shot carries a time range**, and the sum of all shot durations strictly equals the total
- **All required asset image T2I prompts included**: subjects on clean backgrounds, scenes with people excluded — to lock consistency *before* you generate the video

### 📐 Skill Workflow

1. **Parse & expand**: break down story rhythm, inventory subjects & scenes, set the tone
2. **Set duration & shot count**: lay out shots by story beats, assign duration per shot type, sum and calibrate
3. **Build subject & scene profiles**: write anchor descriptions, name assets, lock consistency for the whole film
4. **Write asset image T2I prompts**: one for each subject and scene — generate images first, then video
5. **Write the multi-shot video prompt**: strictly follow the unified template, describe only moving pictures
6. **Control word count**: hard constraint ≤ 4000 characters
7. **Pre-delivery QA**: an 11-point checklist — nothing ships until it passes

---

## 🛠️ Design Philosophy

This is the most valuable part of the Skill. Every rule below was earned the hard way.

### 1. Asset Anchoring: The Real Fix for Consistency

The instinctive way to hold consistency is to "re-describe the protagonist's appearance in every shot" — that's exactly wrong: the more you restate, the more attention the model spends on restating, and the weaker the motion and camera work become.

**The fix**: a subject's and scene's fixed traits are written exactly once in the profile, as the basis for generating asset images (T2I); the video prompt uses only the **short name + this shot's delta** (e.g. "Wu Song": straw hat strapped to his back, hair blown loose by the wind), never re-stating "~30yo brawny man, thick brows…". Consistency is locked by the asset images; the prompt is responsible only for "motion."

### 2. Configure Shots by Story Type

Many people first decide "I want 12 shots," then divide total duration by 12 to get 2.5s per shot — the result is a flat wall of fast cuts with no rhythm at all. That's chaos cutting.

**Hard rule**: shot count is the *result* of laying out story beats and assigning shot types — never a target you set first. Duration is set by shot type: **close-up / fast cut 2-3s, medium / narrative 3-6s, wide / establishing 6-7s**. Every cut needs a narrative reason (change of space / time / viewpoint / information layer / emotional breakpoint). If one shot can tell it, don't use two. Alternate long and short. After a fast-cut group, settle the rhythm with a longer shot.

Typical 30s skeleton: `Wide establishing (6-7s) → Medium narrative ×2-3 → Close-up fast-cut group (2-3s) ×3-4 → Medium transition → Wide closing`

### 3. Write Only the Picture, Don't Distract the Model

```
❌ Still-frame: Wu Song stands on the mountain path, sunset glow behind him, sharp gaze
✅ Video: Wu Song strides out from deep in the frame, his long robe's hem blown back by
   the wind, the camera slowly tilts up from the ground to a wide shot, the sun slowly
   sinks behind the ridge
```

The shot body describes only "what the camera captures and how it moves" — no sound effects (handled by the fixed Music line in the header), no `Plot: …`, no emotional explanation — without distracting the model.
**The three video-essentials** (self-check after every shot; rewrite if any is missing): ① strong verb ② camera-movement word ③ environment/light dynamic.

### 4. Unified Prompt Template

```
Subject: subject1, subject2
Scene: scene name
Music: Do not generate any background music, only generate corresponding sound effects
Shot 1 ｜ 0.0–6.0s ｜ …
Shot 2 ｜ 6.0–9.5s ｜ …
```

- The Music line is fixed (the single audio instruction for the whole film; no shot describes sound); style keywords are unified film-wide and declared once in Shot 1.
- **Hard constraints**: the sum of all shot durations = total duration ≤ 30s; total output ≤ 4000 characters. When over budget, compress redundant environment/style descriptions and merge minor shots first — **never save words by cutting camera movement**.

---

## 📁 Project Structure

```
.
├── SKILL.md              # Main Skill file (core logic, workflow, hard constraints)
├── references/
│   └── templates.md      # Output template + full example + shot-count decision
│                         #   cheat sheet + style/camera/light lexicon
├── public/
│   ├── 关于作者.md
│   ├── 公众号二维码.png
│   └── 海报.jpg
├── README.md             # English (this file)
└── README.zh.md          # 简体中文
```

---

## 🚀 Quick Start

### Who Is This Skill For

Any AI Agent that supports the `Agent Skills` spec (e.g. Codex, Claude Code, WorkBuddy, TRAE Work, Qoder Work, Kimi Work, etc.). As long as your Agent can read and obey a Skill, it can use this to design Seedance 2.5 video prompts.

In principle, models with stronger reasoning are recommended, e.g. Claude Opus 5, K3, DeepSeek-v4-pro, Qwen3.8, GPT-5.6, etc.

### Installation

```bash
# Clone the repo
git clone https://github.com/woyin2024/lengyi-seedance2.5-prompt.git
```

Drop the entire `lengyi-seedance2.5-prompt` folder into your Agent's Skills directory:

- **Claude Code project-level**: `<project>/.claude/skills/lengyi-seedance2.5-prompt/`
- **WorkBuddy user-level**: `~/.workbuddy/skills/lengyi-seedance2.5-prompt/`
- **Other Agents**: follow your tool's Skill installation guide

### Usage

Once installed, trigger it in conversation any of these ways:

- "Turn this novel excerpt into a Seedance video storyboard"
- "Use the lengyi-seedance2.5-prompt skill to design a 30-second multi-shot prompt, the theme is…"
- "Make a shotlist, image-to-video"

Trigger keywords (Chinese): `seedance 视频提示词`, `多镜头提示词`, `视频分镜脚本`, `镜头脚本`, `shotlist`, `文生视频 / 图生视频`, `AI 视频镜头脚本`.

### Recommended Workflow

```
1. Use the Skill to generate the "Asset Image T2I Prompts" first
2. Feed the T2I prompts into a text-to-image model (e.g. Image2) to generate
   subject / scene asset images
3. Feed the asset images + the Skill's "Video Prompt" output into Seedance 2.5
```

---

## 🖼️ Output Example

Input — a single line of plot: **"Wu Song, drunk on Jingyang Ridge, encounters a fierce tiger and fights it bare-handed."**

Output (excerpt; full version in [`references/templates.md`](./references/templates.md)):

```
【Script Overview】
· Total: 30s　· Shots: 9　· Avg: 3.3s
· Shot-count rationale: beats are "entrance — awareness — tiger appears —
  dodge — staff breaks — pin down — exhaustion — ending." The opening needs
  one 6s wide shot to establish the barren mountain and the drunk at once;
  the midsection uses two medium narrative shots; the fight is the emotional
  peak, handled with a group of three 2.5s close-up fast cuts; then it
  returns to medium-close and wide to settle the rhythm.
· Rhythm skeleton: Wide 6.0 → Medium 3 → Close-up 2 → Medium follow 4.0
  → Close-up×3 → Medium-close 3.0 → Wide 3.5
· Style: cinematic wuxia, film grain, realistic texture, warm sunset to cold moon

【Asset Image T2I Prompts】
- Subject "Wu Song": ~30yo brawny man, thick brows, square face, black beard,
  hair tied high, earth-yellow coarse-cloth short martial robe, dark-brown
  cloth sash at waist, straw hat on back, holding a wooden staff, full-body
  front view, cinematic wuxia realism, film grain, clean white background
  showing only the subject, 8K, fine detail, studio soft light
- Subject "Fierce Tiger": adult South China tiger, orange-yellow with black
  stripes, sturdy build, eyes glowing green, side-body coiled-strike pose…
- Scene "Jingyang Ridge mountain path": barren mountain dirt road, scattered
  rocks and dry grass, dead trees leaning in on both sides, blood-red sunset…

【Video Prompt】
Subject: Wu Song, Fierce Tiger
Scene: Jingyang Ridge mountain path
Music: Do not generate any background music, only generate corresponding sound effects
Shot 1 ｜ 0.0–6.0s ｜ Jingyang Ridge mountain path, cinematic wuxia, film grain,
  realistic texture. Extreme wide shot, low-angle slow push-in, tilting up over
  the scattered rocks at the midpoint, settling on a medium shot of the figure
  by the end. Subject "Wu Song" stumbles in his steps, flushed with drink,
  striding out from deep in the frame, staff dragging on the ground carving a
  dirt trail, slowing as he nears and looking around. Long robe hem blown back
  by the mountain wind, sun keeps sinking behind the ridge, shadow stretching
  ever longer.
Shot 2 ｜ 6.0–9.5s ｜ Medium shot, camera steadily tracking Wu Song's right side.
  Wu Song lifts a gourd, tilts his head back to drink, wipes his mouth…
  (remaining shots in the full example)
```
---

## 🤝 Contributing

This Skill was born from repeatedly refining something that didn't feel right when I used it — but it can certainly be better.

If you find, while using it, that:

- A certain input type (ads, anime, product showcases…) has room for better shot rhythm
- The lexicon needs additions (new camera, light, or environment-dynamic terms)
- The template could fit Seedance 2.5's latest features more tightly in specific scenarios
- You'd like more output modes (English-only / bilingual)

contributions are very welcome:

1. Fork this repo
2. Create a feature branch: `git checkout -b feature/your-feature`
3. Commit your changes: `git commit -m 'Add some feature'`
4. Push the branch: `git push origin feature/your-feature`
5. Open a Pull Request

Extra credit for PRs that include **a real, run-through case** (input → output → model-generated result). That's worth more than any abstract discussion.

---

## 📜 License

This project is open-sourced under the [MIT License](LICENSE).

For personal learning use, you're free to use, modify, and distribute it; for commercial use, please obtain authorization in advance — contact via WeChat: lengcp2013.

---

## 👤 About the Author

**Leng Yi (冷逸)**, blogger at **沃垠AI (WoyinAI)**, one of China's top AI tech media outlets. A Vibe Coding developer who doesn't write code, obsessed with Prompt, Skills, and Agent engineering.

- **Unified handle across platforms**: 沃垠AI (WoyinAI)
- **Tags**: Product & operations background, in-depth AI reviews, super OPC
- **Content matrix**: WeChat Official Account, Xiaohongshu, Zhihu, GitHub, Bilibili, X, and more

Follow the **沃垠AI** WeChat Official Account for more AI insights, hands-on Prompt engineering, and Skill design methodology:

<p align="center">
  <img src="public/公众号二维码.png" width="90%" alt="沃垠AI WeChat Official Account QR Code" />
</p>

---

<div align="center">

### If it helped you, leave a ⭐

**Thank you 🙏**

</div>
