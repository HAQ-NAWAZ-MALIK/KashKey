# Kashkey Architecture Documentation

> **Technical Deep-Dive into the Kashmiri Keyboard Framework**

This document provides a comprehensive architecture overview of the Kashkey project, including data flow, component structure, and system design with Mermaid diagrams.

---

## Table of Contents

1. [High-Level Architecture](#1-high-level-architecture)
2. [Component Architecture](#2-component-architecture)
3. [Data Flow](#3-data-flow)
4. [Character Mapping System](#4-character-mapping-system)
5. [Keyboard Generation Pipeline](#5-keyboard-generation-pipeline)
6. [State Management](#6-state-management)
7. [Event Handling System](#7-event-handling-system)
8. [Audio System](#8-audio-system)
9. [Practice Mode System](#9-practice-mode-system)
10. [UI Component Hierarchy](#10-ui-component-hierarchy)
11. [File Structure](#11-file-structure)
12. [Complete Character Map](#12-complete-character-map)

---

## 1. High-Level Architecture

The Kashkey application follows a simple but effective single-page architecture with no external dependencies beyond fonts.

```mermaid
graph TB
    subgraph "User Interface Layer"
        UI[HTML/CSS UI]
        KB[Virtual Keyboard]
        DP[Display Panel]
        SP[Side Panel]
    end
    
    subgraph "Logic Layer"
        KG[Keyboard Generator]
        EH[Event Handlers]
        SM[State Manager]
        AU[Audio System]
    end
    
    subgraph "Data Layer"
        CM[Character Map]
        KL[Key Layout]
        PM[Physical Map]
        PS[Practice Sentences]
    end
    
    subgraph "External Resources"
        GF[Google Fonts]
        WA[Web Audio API]
        SS[Speech Synthesis]
        CB[Clipboard API]
    end
    
    UI --> KG
    KB --> EH
    EH --> SM
    SM --> DP
    SM --> AU
    
    KG --> CM
    KG --> KL
    EH --> PM
    SP --> PS
    
    UI -.-> GF
    AU -.-> WA
    DP -.-> SS
    DP -.-> CB
```

---

## 2. Component Architecture

```mermaid
graph LR
    subgraph "App Container"
        direction TB
        H[Header Component]
        MC[Main Content]
        NA[Notification Area]
    end
    
    subgraph "Header"
        BR[Brand/Logo]
        HC[Header Controls]
    end
    
    subgraph "Main Content Grid"
        direction TB
        TS[Text Section]
        SPA[Side Panel]
    end
    
    subgraph "Text Section"
        DA[Display Area]
        KC[Keyboard Container]
    end
    
    subgraph "Side Panel"
        PC[Practice Card]
        ST[Stats Panel]
        QG[Quick Guide]
    end
    
    H --> BR
    H --> HC
    MC --> TS
    MC --> SPA
    TS --> DA
    TS --> KC
    SPA --> PC
    SPA --> ST
    SPA --> QG
```

---

## 3. Data Flow

### 3.1 Input Processing Flow

```mermaid
flowchart TD
    A[User Input] --> B{Input Type}
    B -->|Virtual Click| C[handleKeyClick]
    B -->|Physical Key| D[keydown Event]
    B -->|Mouse/Touch| E[mousedown/touchstart]
    
    D --> F[Map Physical Key]
    F --> G{Valid Key?}
    G -->|Yes| C
    G -->|No| H[Ignore]
    
    E --> C
    
    C --> I[Play Sound]
    C --> J[Update Start Time]
    C --> K[Increment Keystroke Count]
    C --> L[Visual Feedback]
    
    L --> M{Key Type}
    M -->|SPACE| N[Add Space]
    M -->|Character| O{Modifier Active?}
    
    O -->|Yes| P[Add Modifier Char]
    O -->|No| Q[Add Normal Char]
    
    N --> R[Update Text Content]
    P --> R
    Q --> R
    
    R --> S[Update Display]
    R --> T[Update Stats]
```

### 3.2 State Update Flow

```mermaid
sequenceDiagram
    participant U as User
    participant E as Event Handler
    participant S as State Manager
    participant D as Display
    participant ST as Statistics
    
    U->>E: Press Key
    E->>S: Update textContent
    E->>S: Update keystrokeCount
    S->>D: updateDisplay()
    D->>D: Set textOutput.textContent
    D->>D: Toggle Placeholder
    D->>D: Auto-scroll
    S->>ST: updateStats()
    ST->>ST: Calculate charCount
    ST->>ST: Calculate wordCount
    ST->>ST: Calculate WPM
    ST->>D: Update UI Elements
```

---

## 4. Character Mapping System

### 4.1 Mapping Structure

```mermaid
classDiagram
    class CharacterMap {
        +Object kashCharMap
        +getCharacter(key, modifier)
        +hasKey(key)
    }
    
    class KeyEntry {
        +String normal
        +String modifier
    }
    
    class PhysicalMap {
        +Object physicalMap
        +mapKey(physicalKey)
    }
    
    CharacterMap "1" --o "*" KeyEntry : contains
    PhysicalMap --> CharacterMap : references
```

### 4.2 Key Mapping Example

```mermaid
flowchart LR
    subgraph "English Key"
        A["'A'"]
    end
    
    subgraph "kashCharMap['A']"
        N["normal: 'ا'<br/>(U+0627)"]
        M["modifier: 'آ'<br/>(U+0622)"]
    end
    
    subgraph "Output"
        O1["Without Shift/Alt: ا"]
        O2["With Shift/Alt: آ"]
    end
    
    A --> N
    A --> M
    N --> O1
    M --> O2
```

---

## 5. Keyboard Generation Pipeline

```mermaid
flowchart TD
    A[DOMContentLoaded] --> B[generateKeyboard]
    B --> C[Get keyboardGrid Container]
    C --> D[Clear Container]
    D --> E[Iterate keyLayout Array]
    
    E --> F[Create Row Div]
    F --> G[Iterate Row Keys]
    
    G --> H{Key Type}
    H -->|SPACE| I[Create Spacebar]
    H -->|Regular| J[Create Regular Key]
    
    I --> K[Add Event Listeners]
    J --> L[Add English Label]
    J --> M[Add Main Character]
    J --> N[Add Modifier Label]
    J --> K
    
    K --> O[Append to Row]
    O --> P{More Keys?}
    P -->|Yes| G
    P -->|No| Q[Append Row to Container]
    Q --> R{More Rows?}
    R -->|Yes| E
    R -->|No| S[Keyboard Complete]
```

### 5.1 Key Layout Structure

```mermaid
graph TD
    subgraph "Row 0: Numbers"
        R0["`1234567890"]
    end
    
    subgraph "Row 1: Top Row"
        R1["QWERTYUIOP[]"]
    end
    
    subgraph "Row 2: Home Row"
        R2["ASDFGHJKL"]
    end
    
    subgraph "Row 3: Bottom Row"
        R3["ZXCVBNM,./"]
    end
    
    subgraph "Row 4: Space Row"
        R4["-SPACE"]
    end
    
    R0 --> R1 --> R2 --> R3 --> R4
```

---

## 6. State Management

### 6.1 Application State

```mermaid
classDiagram
    class AppState {
        +String textContent
        +Boolean isShiftPressed
        +Boolean isAltPressed
        +Date startTime
        +Number keystrokeCount
        +Boolean soundEnabled
        +String theme
        +Boolean practiceMode
        +Number currentSentenceIndex
    }
    
    class Actions {
        +clearText()
        +copyText()
        +speakText()
        +toggleTheme()
        +toggleSound()
        +togglePracticeMode()
    }
    
    class Updates {
        +updateDisplay()
        +updateStats()
        +loadSentence()
    }
    
    AppState <-- Actions : modifies
    AppState <-- Updates : reads/updates
```

### 6.2 State Transitions

```mermaid
stateDiagram-v2
    [*] --> Idle: Page Load
    
    Idle --> Typing: First Keystroke
    Typing --> Typing: More Keys
    Typing --> PracticeMode: Toggle Practice
    PracticeMode --> PracticeMode: Type/Navigate
    PracticeMode --> Typing: Toggle Practice Off
    Typing --> Idle: Clear Text
    
    state Typing {
        [*] --> Normal
        Normal --> Shift: Shift Down
        Normal --> Alt: Alt Down
        Shift --> Normal: Shift Up
        Alt --> Normal: Alt Up
    }
    
    state PracticeMode {
        [*] --> ViewingSentence
        ViewingSentence --> TypedAnswer: Type
        TypedAnswer --> Checked: Check
        Checked --> ViewingSentence: Next/Prev
    }
```

---

## 7. Event Handling System

```mermaid
flowchart TD
    subgraph "DOM Events"
        KD[keydown]
        KU[keyup]
        MD[mousedown]
        TS[touchstart]
        CL[click]
    end
    
    subgraph "Event Handlers"
        KDH[Keydown Handler]
        KUH[Keyup Handler]
        KCH[Key Click Handler]
    end
    
    subgraph "State Updates"
        SM[Shift/Alt Modifier]
        BS[Backspace Handler]
        CH[Character Handler]
    end
    
    subgraph "Actions"
        PS[Play Sound]
        VF[Visual Feedback]
        UD[Update Display]
        US[Update Stats]
    end
    
    KD --> KDH
    KU --> KUH
    MD --> KCH
    TS --> KCH
    
    KDH --> SM
    KDH --> BS
    KDH --> CH
    KUH --> SM
    
    CH --> PS
    CH --> VF
    CH --> UD
    CH --> US
    
    CL --> AU[Resume Audio Context]
```

---

## 8. Audio System

```mermaid
flowchart TD
    A[initAudio] --> B[Create AudioContext]
    B --> C[Define keyTone Function]
    
    D[playKeySound] --> E{Sound Enabled?}
    E -->|No| F[Return]
    E -->|Yes| G{keyTone Exists?}
    G -->|No| F
    G -->|Yes| H[Create Oscillator]
    
    H --> I[Create Gain Node]
    I --> J[Connect Oscillator to Gain]
    J --> K[Connect Gain to Destination]
    K --> L[Set Frequency: 800Hz]
    L --> M[Set Gain Envelope]
    M --> N[Start Oscillator]
    N --> O[Stop After 100ms]
    
    subgraph "Gain Envelope"
        G1[Start: 0.05]
        G2[Decay: 100ms]
        G3[End: 0.001]
        G1 --> G2 --> G3
    end
```

---

## 9. Practice Mode System

```mermaid
flowchart TD
    A[togglePracticeMode] --> B{Practice On?}
    B -->|Yes| C[Show Practice Card]
    B -->|No| D[Hide Practice Card]
    
    C --> E[Load First Sentence]
    
    E --> F[loadSentence]
    F --> G{Index Valid?}
    G -->|No| H[Wrap Around]
    G -->|Yes| I[Get Sentence Object]
    
    H --> I
    I --> J[Display Kashmiri Text]
    I --> K[Display Translation]
    I --> L[Display Difficulty]
    I --> M[Clear Previous Feedback]
    I --> N[Clear Text Area]
    
    O[checkPractice] --> P{Practice Mode?}
    P -->|No| Q[Return]
    P -->|Yes| R[Compare Input to Target]
    
    R --> S{Match?}
    S -->|Yes| T[Show Success]
    S -->|No| U[Show Error]
```

### 9.1 Practice Sentence Data Structure

```mermaid
classDiagram
    class PracticeSentence {
        +Number id
        +String text
        +String translation
        +String difficulty
    }
    
    class DifficultyLevel {
        <<enumeration>>
        Easy
        Medium
        Hard
    }
    
    PracticeSentence --> DifficultyLevel
```

---

## 10. UI Component Hierarchy

```mermaid
graph TD
    HTML[HTML Document]
    HTML --> Body[body data-theme]
    Body --> AC[app-container]
    
    AC --> Header[header.header.glass-panel]
    AC --> MC[div.main-content]
    AC --> NA[div#notificationArea]
    
    Header --> Brand[div.brand]
    Header --> HC[div.header-controls]
    
    Brand --> MT[h1.main-title]
    Brand --> TU[div.title-urdu]
    
    HC --> PB[button#practiceBtn]
    HC --> TB[button#themeBtn]
    HC --> SB[button#soundBtn]
    
    MC --> TS[div.text-section]
    MC --> SP[div.side-panel]
    
    TS --> DA[div.display-area.glass-panel]
    TS --> KC[div.keyboard-container.glass-panel]
    
    DA --> TO[div.text-output]
    DA --> PH[div.placeholder]
    DA --> AB[div.action-bar]
    
    KC --> KR[div.keyboard-row *5]
    KR --> Key[div.key *n]
    
    SP --> PC[div.practice-card.glass-panel]
    SP --> Stats[div.glass-panel Stats]
    SP --> QG[div.glass-panel Quick Guide]
```

---

## 11. File Structure

```mermaid
graph LR
    Root[kashkey/]
    Root --> Index[index.html]
    Root --> PD[practice_data/]
    Root --> README[README.md]
    Root --> ARCH[ARCHITECTURE.md]
    Root --> BUILD[BUILD_GUIDE.md]
    
    PD --> Sent[sentences.js]
    
    subgraph "index.html Contents"
        CSS[Embedded CSS]
        HTML_S[HTML Structure]
        JS[Embedded JavaScript]
    end
    
    Index --> CSS
    Index --> HTML_S
    Index --> JS
```

---

## 12. Complete Character Map

### 12.1 Number Row

| Key | Normal | Unicode | Modifier | Unicode |
|-----|--------|---------|----------|---------|
| ` | ` | U+0060 | ﷺ | U+FDFA |
| 1 | ۱ | U+06F1 | ؐ | U+0610 |
| 2 | ۲ | U+06F2 | ؓ | U+0613 |
| 3 | ۳ | U+06F3 | ؔ | U+0614 |
| 4 | ۴ | U+06F4 | ؃ | U+0603 |
| 5 | ۵ | U+06F5 | ؒ | U+0612 |
| 6 | ۶ | U+06F6 | ؑ | U+0611 |
| 7 | ۷ | U+06F7 | ْ | U+0652 |
| 8 | ۸ | U+06F8 | ' | U+2018 |
| 9 | ۹ | U+06F9 | ( | U+0028 |
| 0 | ٠ | U+0660 | ) | U+0029 |

### 12.2 QWERTY Row

| Key | Normal | Unicode | Modifier | Unicode |
|-----|--------|---------|----------|---------|
| Q | ق | U+0642 | ٍ | U+064D |
| W | ؤ | U+066E+U+06EA | ء | U+0621 |
| E | ع | U+0639 | ٰ | U+0670 |
| R | ر | U+0631 | ڑ | U+0691 |
| T | ت | U+062A | ٹ | U+0679 |
| Y | ے | U+06D2 | ّ | U+0651 |
| U | ٖ | U+0656 | ٗ | U+0657 |
| I | ی | U+06CC | ۍ | U+06CD |
| O | ہ | U+06C1 | ھ | U+06BE |
| P | پ | U+067E | ُ | U+064F |
| [ | ط | U+0637 | ' | U+2019 |
| ] | ظ | U+0638 | ' | U+2018 |

### 12.3 Home Row

| Key | Normal | Unicode | Modifier | Unicode |
|-----|--------|---------|----------|---------|
| A | ا | U+0627 | آ | U+0622 |
| S | س | U+0633 | ص | U+0635 |
| D | د | U+062F | ڈ | U+0688 |
| F | ف | U+0641 | ٚ | U+065A |
| G | گ | U+06AF | غ | U+063A |
| H | ح | U+062D | ٛ | U+065B |
| J | ج | U+062C | ض | U+0636 |
| K | ک | U+06A9 | خ | U+062E |
| L | ل | U+0644 | رحمتہ اللہ علیہ | Phrase |

### 12.4 Bottom Row

| Key | Normal | Unicode | Modifier | Unicode |
|-----|--------|---------|----------|---------|
| Z | ز | U+0632 | ذ | U+0630 |
| X | ش | U+0634 | ژ | U+0698 |
| C | چ | U+0686 | ث | U+062B |
| V | و | U+0648 | ۄ | U+06C4 |
| B | ب | U+0628 | ً | U+064B |
| N | ن | U+0646 | ں | U+06BA |
| M | م | U+0645 | ٕ | U+0655 |
| , | ، | U+060C | ِ | U+0650 |
| . | ۔ | U+06D4 | َ | U+064E |
| / | ٔ | U+0654 | ؟ | U+061F |
| - | ٲ | U+0672 | ۭ | U+06ED |

---

## Technology Stack Summary

```mermaid
mindmap
  root((Kashkey))
    Core Technologies
      HTML5
      CSS3
      Vanilla JavaScript
    Fonts
      Outfit
      Noto Nastaliq Urdu
    Browser APIs
      Web Audio API
      Clipboard API
      Speech Synthesis
    Design
      Glassmorphism
      CSS Variables
      Flexbox/Grid
      Animations
```

---

## Conclusion

The Kashkey architecture demonstrates that a sophisticated, feature-rich virtual keyboard can be built with:

- **Zero JavaScript dependencies** - Pure vanilla JS
- **Single HTML file** - Easy deployment
- **Modern CSS** - Glassmorphism with CSS variables
- **Modular data** - Separated character mapping and practice data
- **Browser APIs** - Leveraging native capabilities

This architecture is highly portable and can be adapted for any language by modifying the character mapping and keyboard layout arrays. See the [BUILD_GUIDE.md](./BUILD_GUIDE.md) for step-by-step instructions.
