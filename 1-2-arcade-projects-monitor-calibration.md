---
title: Arcade Monitor Calibration
parent: Arcade-Projects
has_children: false
nav_order: 1
---

# Arcade Monitor Calibration

Following a thread from the Arcade Projects forum, here I document the calibration of an arcade monitor using a colorimeter. 

<div align="center">
  <img alt="Display of emojis" src="https://user-images.githubusercontent.com/17674324/213933538-7f98b563-4287-4502-9013-3bf2e76fd8a1.jpg" width="50%" height="50%"><br>
  <sup>Sample display of image in HTML format <sup>
</div>

After the first try I was still way off from the perfect grey scale, but hey, I bet I can do this drunk or tired now and the picture looks good.

<img src="https://user-images.githubusercontent.com/17674324/213933211-2ddfb2a3-67ff-4e9b-b4c6-d2692ae58721.PNG" width="100" height="100">

## Preconditions and Definitions

- **Source**: [Thread Forum Arcade Projects](https://www.arcade-projects.com/threads/arcade-monitor-calibration-guides.17183)
- **Colorimeter used**: [Calibrite ColorChecker Display Pro](https://calibrite.com/)
- **Arcade Monitor used**: [Hantarex Polo 25](http://files.arcadeinfo.de/Monitore/Hantarex%20Polo.pdf)
- **Test Pattern Software used**:[240P Test Suite Rom Sega Dreamcast](https://artemiourbina.itch.io/240p-test-suite) on [GroovyArcade with Flycast Emulator / Retroarch](https://gitlab.com/groovyarcade/support/-/wikis/1-About-GroovyArcade/1.1-Welcome)
- **Monitor Calibration Software used**: [hfcr](https://sourceforge.net/projects/hcfr/)

## Steps

1. ### Prepare HFCR
- File &rarr; New &rarr; Generator: DVD manual &rarr; Sensor: X-Rite DisplayPro &rarr; No correction file &rarr; Finish
- Display Type: CRT, Reading Type: Display, Rest: default &rarr; Okay

2. ### Heat up monitor tube 
- **White Screen Pattern:** Test Patterns &rarr; Color & Black Levels &rarr; White & RGB screens.
- **Time**: 10-60 minutes 

3. ### Set Flyback Screen Dial
- **Gain Red (RV1) and Blue (RV6):** middle
- **Cutoff Red (RV3), Green (RV4) and Blue (RV5):** low
- **Contrast (RV405):** middle
- **Brightness (RV406)** low
- **Set Black Pictures:** R:0x00, G:0x00, B:0x00
- **Screen Pot:** low &rarr; picture slightly visible

4. ### Set Contrast and Brightness 
- **Gain Red (RV1) and Blue (RV6):** middle
- **Cutoff Red (RV3), Green (RV4) and Blue (RV5):** middle
- **Contrast (RV405):** middle
- **Brightness (RV406)** middle
- **100 IRE Pattern:** Test Patterns → Color & Black Levels → 100 IRE
- **Value Y ftl:** dial Contrast → 35 Y ftl.
- **Note Value Y:** 120 (in my case)
- **Set IRE Level:** 10 IRE (with shoulder pads L and R)
- **Value Y ftl:** dial Brightness → 0,0065 x Y value before → Y 0,78 (in my case, adjust Screen a hair if not low enough)

5. ### Set Gain and Cutoff 
- **Set IRE Levels:** 80 IRE for Gain, 30 IRE for Cutoff
- **Dial Pots until:** R 100%, G 100%, B 100%, Delta E 0,0 

6. ### Repeat Steps 4 and 5 once  

7. ### Verify Calibration
- Measures &rarr; Grey Scale &rarr; Follow instructions and take a measurement IRE values 10-100
 

