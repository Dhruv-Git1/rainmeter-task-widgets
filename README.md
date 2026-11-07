# Rainmeter Task and Chore widgets

Two small panels for your Windows desktop, **Tasks** and **Chores**. They read a plain text to-do
file (written in Markdown, a simple text format where `- [ ]` means an unticked box) and show the
open items with a live countdown to each deadline. This repository holds only the two widgets
(called *skins* in Rainmeter) and the settings that go with them. It does not include Rainmeter itself.

Rainmeter is a free program that puts small panels on your desktop. Get it from
[rainmeter.net](https://www.rainmeter.net/).

These widgets are a customised version of
[thomas-quant/rainmeter-markdown-tasks](https://github.com/thomas-quant/rainmeter-markdown-tasks)
(MIT licence, see `LICENSE`). On top of the original, they add:

- ticking by clicking the `[ ]` box only (clicking the text does nothing),
- a **+** button to add a new item by typing it,
- deadlines written in everyday words, with a live countdown,
- colours that change as the deadline gets close,
- "soft" targets that never turn red,
- items sorted by deadline, nearest first,
- repeating chores such as "every sunday".

## What the colours mean

| Colour | When |
| --- | --- |
| Normal (light grey) | 3 days or more left, or no deadline |
| **Yellow** | Fewer than 3 days left |
| **Red** | 1 day (24 hours) or less left, or the deadline has passed |
| Dim grey | A soft target such as `aim sunday`. It never turns red. |

Repeating chores are a special case. They are yellow while they are due and turn red only if you
miss them. Otherwise a daily chore would be red all day, which would tell you nothing.

You can change these in the `[Variables]` section near the top of `Tasks/Tasks.ini` and
`Chores/Chores.ini`:

- `SoonDays=3` is the number of days before the deadline when yellow starts.
- `SoonColor=255,220,60,255` is the yellow, written as red, green, blue, opacity (each 0 to 255).
- `OverdueColor=255,105,97,255` is the red, in the same format.

The "1 day means red" rule is written inside the `.lua` files rather than being a setting. To change
it, search for `86400` (the number of seconds in a day) in `Tasks/Tasks.lua` and `Chores/Chores.lua`.

## Setting it up again from scratch

1. **Install Rainmeter** from [rainmeter.net](https://www.rainmeter.net/) and run it once.
2. **Copy the two folders.** Copy the `Tasks` and `Chores` folders from this repository into
   `Documents\Rainmeter\Skins\` (so you end up with `Documents\Rainmeter\Skins\Tasks\Tasks.ini`
   and `Documents\Rainmeter\Skins\Chores\Chores.ini`).
3. **Make your to-do file.** Copy `Tasks.md.example` to somewhere permanent, for example
   `D:\Desktop\Tasks.md`, and fill it with your own items. Open items go under a heading called
   `### Tasks` (shown in the Tasks widget) or `### Chores` (shown in the Chores widget).
4. **Tell both widgets where the file is.** Open `Tasks/Tasks.ini` and `Chores/Chores.ini` in
   Notepad and change the `TaskFile=` line in each to the full path of your file.
   The path must use plain English letters and digits only. The script refuses paths with accents
   or other special characters.
5. **Show the widgets.** Right-click the Rainmeter icon in the system tray (bottom right of the
   screen), choose **Manage**, then select `Tasks` and click **Load**. Do the same for `Chores`.
6. **Put them where you like** by dragging. My own layout, on a screen scaled to 125%, had Tasks at
   position X=1196, Y=92 and Chores at X=1196, Y=402. Positions depend on your screen size and scaling,
   so treat these as a rough guide only.
7. **Make Rainmeter start with Windows**, if it does not do that by itself. Press `Win + R`, type
   `shell:startup` and press Enter. In the folder that opens, create a shortcut to
   `C:\Program Files\Rainmeter\Rainmeter.exe`. (A silent install through the command line did *not*
   add this for me, and the widgets vanished after a restart until I added it.)

## Writing items in your to-do file

Dates are written **day first** (10.11.26 means 10 November 2026). A date with no time of day counts
as due at 11:59 pm that day.

- **Deadlines:** `by 10.11.26`, `by 10/11`, `by 10 nov`, `by sunday 5pm`, `tomorrow`, `tonight`,
  `in 3 days`, `in 5 hours`, `this weekend`, `end of week`, `end of month`, `next week`
- **Soft targets (dim grey, never red):** `aim sunday`, `ideally 10.11`, `~friday`
- **Repeating (comes back after you tick it):** `daily`, `every sunday`, `every mon and thu`,
  `every weekend`, `every 2 weeks`, `weekly`, `monthly`, `every sun 10am`

Click a `[ ]` box to tick an item. The widget changes that one line in your file from `- [ ]` to
`- [x]` and leaves everything else alone. Each widget shows up to 15 items.

## Things that can trip you up

- **Do not change the text format of the `.ini` and `.lua` files.** They are saved in a format called
  UTF-16 (Windows' "Unicode" format), and Rainmeter will not read them in any other format. Windows
  Notepad keeps the format when you save. If a file looks like garbage in another editor, do not save it
  from there. This is also why git is told to treat these files as untouchable (see `.gitattributes`).
- **Windows 11 Notepad and your to-do file.** With Markdown formatting switched on, Notepad may save
  `- [ ]` as `* \[ ]`. The widget understands both. But Notepad does not notice when the widget ticks a
  box, so close and reopen the file before you edit it again, or you may undo the tick.
- **Testing the colours without waiting for days to pass.** In the `.ini`, set
  `NowOverride=2026-11-08 16:00` (year-month-day hour:minute) and refresh the widget. It then behaves as if
  that is the current time. Clear the line again afterwards.
- **`widget-memory.txt`** is created by the widget next to each skin. It remembers when repeating items
  were last ticked. It is personal, so it is not in this repository and is safe to delete.

## Licence

MIT, the same as the project this was built from. See `LICENSE`.
