# Kashkey - Kashmiri Keyboard Pro

<div align="center">

![Kashkey](https://img.shields.io/badge/Kashkey-Kashmiri%20Keyboard-667eea?style=for-the-badge)
![Version](https://img.shields.io/badge/version-1.0.0-00f2fe?style=for-the-badge)
![License](https://img.shields.io/badge/license-MIT-2ecc71?style=for-the-badge)

**کٲشُر کیٖ بورڈ**

*A modern, feature-rich virtual keyboard for typing Kashmiri script with a premium glassmorphism design*

[Features](#-features) • [Demo](#-demo) • [Installation](#-installation) • [Usage](#-usage) • [Architecture](./ARCHITECTURE.md) • [Build Guide](./BUILD_GUIDE.md)

</div>

---

## 🎯 Overview

**Kashkey** is a professional-grade virtual keyboard application specifically designed for the Kashmiri language (کٲشُر). It provides a seamless typing experience with support for both on-screen clicking and physical keyboard input, featuring over 50+ Kashmiri characters with modifier variations.

### Why Kashkey?

Kashmiri (کٲشُر) is a Dardic language spoken primarily in the Kashmir Valley. Despite having millions of speakers, digital typing support for Kashmiri has been limited. Kashkey bridges this gap by providing:

- **Native Script Support**: Full support for Kashmiri Perso-Arabic script with all diacritics
- **Modern UI**: Premium glassmorphism design that doesn't compromise usability
- **Learning Tools**: Built-in practice mode for language learners
- **Cross-Platform**: Works in any modern web browser

---

## ✨ Features

### 🎹 Keyboard Features

| Feature | Description |
|---------|-------------|
| **50+ Characters** | Complete Kashmiri character set with vowel markers |
| **Modifier Keys** | Shift/Alt support for extended characters |
| **Physical Keyboard** | Use your hardware keyboard for faster typing |
| **Visual Feedback** | Animated key presses with glow effects |
| **Audio Feedback** | Optional keystroke sounds using Web Audio API |

### 📊 Typing Analytics

- **Real-time Statistics**: Characters, words, WPM tracking
- **Accuracy Measurement**: Track your typing precision
- **Timer**: Automatic session timing from first keystroke

### 📚 Practice Mode

- **Curated Sentences**: Pre-loaded Kashmiri phrases with translations
- **Difficulty Levels**: Easy, Medium, and Hard practice sentences
- **Instant Feedback**: Check your typed text against the target
- **Navigation**: Browse through practice sentences easily

### 🎨 Design & UX

- **Glassmorphism UI**: Modern frosted-glass aesthetic
- **Animated Gradients**: Dynamic background animations
- **Dark/Light Themes**: Toggle between color schemes
- **Responsive Layout**: Works on desktop and mobile devices
- **Smooth Animations**: Polished transitions throughout

### 🔧 Utilities

- **Copy to Clipboard**: One-click copy functionality
- **Text-to-Speech**: Hear pronunciation (uses Urdu TTS as fallback)
- **Clear Text**: Quick reset option
- **Toast Notifications**: User feedback for all actions

---

## 🖥️ Demo

<div align="center">

### Keyboard Layout

```
┌───┬───┬───┬───┬───┬───┬───┬───┬───┬───┬───┐
│ ` │ ۱ │ ۲ │ ۳ │ ۴ │ ۵ │ ۶ │ ۷ │ ۸ │ ۹ │ ۰ │
├───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┤
│ ق │ ؤ │ ع │ ر │ ت │ ے │ ٖ │ ی │ ہ │ پ │ ط │ ظ │
├───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┤
│ ا │ س │ د │ ف │ گ │ ح │ ج │ ک │ ل │
├───┼───┼───┼───┼───┼───┼───┼───┼───┼───┤
│ ز │ ش │ چ │ و │ ب │ ن │ م │ ، │ ۔ │ ٔ │
├───┼─────────────────────────────────┼───┤
│ ۲ │           SPACE                 │   │
└───┴─────────────────────────────────┴───┘
```

</div>

---

## 🚀 Installation

### Quick Start (No Installation)

1. Download or clone this repository
2. Open `index.html` in any modern web browser
3. Start typing!

```bash
# Clone the repository
git clone https://github.com/yourusername/kashkey.git

# Navigate to the directory
cd kashkey

# Open in browser (Windows)
start index.html

# Open in browser (macOS)
open index.html

# Open in browser (Linux)
xdg-open index.html
```

### Requirements

- Modern web browser (Chrome, Firefox, Safari, Edge)
- JavaScript enabled
- Internet connection (for Google Fonts)

---

## 📖 Usage

### Basic Typing

1. **Click** on any key to type that character
2. **Physical Keyboard**: Type on your hardware keyboard - keys are mapped to the corresponding Kashmiri characters
3. **Backspace**: Remove the last character

### Modifier Characters

| Modifier | Usage |
|----------|-------|
| **Shift** | Hold Shift and click/type to get secondary characters |
| **Alt** | Hold Alt for alternative characters |

### Practice Mode

1. Click the **🎯 Practice** button in the header
2. A Kashmiri sentence will appear with its English translation
3. Type the sentence using the keyboard
4. Click **✅ Check** to verify your input
5. Use **⬅️** and **➡️** to navigate between sentences

### Additional Features

| Button | Action |
|--------|--------|
| 🗑️ Clear | Clears all typed text |
| 📋 Copy | Copies text to clipboard |
| 🔊 Speak | Reads the text aloud |
| 🌓 Theme | Toggles dark/light mode |
| 🔊 Sound | Toggles keystroke sounds |

---

## 📁 Project Structure

```
kashkey/
├── index.html              # Main application (HTML + CSS + JS)
└── practice_data/
    └── sentences.js        # Practice sentences data
```

### Character Mapping

The keyboard maps English keys to Kashmiri characters:

| Key | Normal | Modifier |
|-----|--------|----------|
| A | ا (Alif) | آ (Alif Madda) |
| B | ب (Ba) | ً (Fathatan) |
| C | چ (Che) | ث (Se) |
| Q | ق (Qaf) | ٍ (Kasratan) |
| ... | ... | ... |

See the [Architecture Document](./ARCHITECTURE.md) for the complete character mapping.

---

## 🛠️ Development

### Adding New Practice Sentences

Edit `practice_data/sentences.js`:

```javascript
const practiceSentences = [
    {
        id: 1,
        text: "کٲشُر زَبان",      // Kashmiri text
        translation: "Kashmiri language",  // English translation
        difficulty: "Easy"                  // Easy | Medium | Hard
    },
    // Add more sentences...
];
```

### Customizing the Theme

Edit the CSS variables in `index.html`:

```css
:root {
    --bg-gradient: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    --accent-color: #00f2fe;
    --text-primary: #ffffff;
    /* ... */
}
```

---

## 🤝 Contributing

Contributions are welcome! Here are some ways you can help:

1. **Add More Practice Sentences**: Help expand the learning content
2. **Bug Reports**: Open an issue if you find any bugs
3. **Feature Requests**: Suggest new features
4. **Documentation**: Improve the docs

---

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## 🙏 Acknowledgments

- **Noto Nastaliq Urdu** font by Google Fonts
- **Outfit** font by Google Fonts
- Kashmiri language community for script references

---

## 📞 Contact

For questions, suggestions, or feedback:

- Create an [Issue](https://github.com/yourusername/kashkey/issues)
- Pull Requests welcome!

---

<div align="center">

**Made with ❤️ for the Kashmiri language community**

*کٲشُر چھُ اَکھ خوبصورت زَبان*

*Kashmiri is a beautiful language*

</div>
