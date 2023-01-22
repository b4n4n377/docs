---
title: Arcade Monitor Calibration
parent: Arcade-Projects
has_children: false
nav_order: 1
---

# Arcade Monitor Calibration

Following a thread from the Arcade Projects forum, here I document the the calibration of an arcade monitor using a colorimeter.

## Preconditions and Definitions

- **Source**: [Thread Forum Arcade Projects](https://www.arcade-projects.com/threads/arcade-monitor-calibration-guides.17183)
- **Colorimeter used**: [Calibrite ColorChecker Display Pro](https://calibrite.com/)
- **Arcade Monitor used**: [Hantarex Polo 25](http://files.arcadeinfo.de/Monitore/Hantarex%20Polo.pdf)
- **Test Pattern Software used**:[240P Test Suite Rom Sega Dreamcast](https://artemiourbina.itch.io/240p-test-suite) on [GroovyArcade with Flycast Emulator / Retroarch](https://gitlab.com/groovyarcade/support/-/wikis/1-About-GroovyArcade/1.1-Welcome)
- **Monitor Calibration Software used**: [hfcr](https://sourceforge.net/projects/hcfr/)

## Steps

1. ### Heat up monitor tube 
- **White screen in menu:** Test Patterns &rarr; Color & Black Levels &rarr; White & RGB screens.
- **Time**: 10-60 minutes 

2. ### Prepare Screen Setting
- **Gain Red (RV1) and Blue (RV6):** Middle Position
- **Cutoff Red (RV3), Green (RV4) and Blue (RV5):** Low Position
- **Contrast (RV405)**: Middle Position
- **Brightness (RV406)**: Low Position
- **Change white to black in menu:**: R,G,B off


2. ### Prepare HFCR
- File &rarr; New &rarr; Generator: DVD manual &rarr; Sensor: X-Rite DisplayPro &rarr; No correction file &rarr; Finish
- Display Type: CRT, Reading Type: Display, Rest: default &rarr; Okay




