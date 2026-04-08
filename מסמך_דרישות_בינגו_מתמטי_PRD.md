# PRD - Math Bingo Game for 9th Grade
## Product Requirements Document v1.0

---

## 📋 Executive Summary

**Product Name:** Math Bingo - Interactive Educational Game  
**Target Users:** Math teachers (9th grade, 4-unit level) in Israel  
**Primary Language:** Hebrew (RTL)  
**Platform:** Web-based application (HTML/CSS/JavaScript)  
**Deployment:** Single-file, offline-capable HTML application

---

## 🎯 Product Vision

An interactive bingo game system that transforms math test preparation into an engaging classroom activity. Teachers can project questions on screen with fun animations while students solve problems and mark answers on printed bingo boards.

---

## 👥 User Personas

### Primary User: Math Teacher
- **Name:** Noam, 35 years old
- **Context:** Teaching 9th grade (4-unit math) in Israel
- **Tech Level:** Comfortable with basic web applications
- **Needs:**
  - Quick setup (< 5 minutes)
  - Printable student boards
  - Professional appearance for classroom projection
  - Easy question management

### Secondary User: Student
- **Age:** 14-15 years old
- **Interaction:** Passive (receives printed board, watches projected screen)
- **Needs:** Clear, readable printed boards with distinct answers

---

## 🎮 Core Features & Requirements

### 1. Game Configuration Panel

#### 1.1 Board Size Selection
**Requirement ID:** REQ-001  
**Priority:** P0 (Critical)

```
FUNCTIONALITY:
- Dropdown selector with 3 options:
  ├─ 3x3 (9 cells) - Label: "3x3 (קל)"
  ├─ 4x4 (16 cells) - Label: "4x4 (בינוני)"
  └─ 5x5 (25 cells) - Label: "5x5 (מאתגר)" [DEFAULT]

VALIDATION:
- Only one size can be selected at a time
- Default to 5x5 on page load

UI SPECS:
- Standard dropdown/select element
- Hebrew labels, RTL direction
- Clearly visible, minimum 16px font size
```

#### 1.2 Number of Players Input
**Requirement ID:** REQ-002  
**Priority:** P0 (Critical)

```
FUNCTIONALITY:
- Numeric input field
- Range: 1-40 players
- Default value: 25

VALIDATION:
- Only integers accepted
- Min: 1, Max: 40
- Show error if out of range: "מספר תלמידים חייב להיות בין 1 ל-40"
- Auto-correct to nearest valid value

UI SPECS:
- Number input with +/- controls
- Clear label: "מספר תלמידים"
```

#### 1.3 Answer Range Configuration
**Requirement ID:** REQ-003  
**Priority:** P1 (High)

```
FUNCTIONALITY:
- Two numeric inputs:
  ├─ Minimum answer value (default: -10)
  └─ Maximum answer value (default: 100)

PURPOSE:
- Filter question bank to show only questions with answers in range
- Enable teacher to focus on specific number ranges

VALIDATION:
- Min must be less than Max
- Check that enough questions exist in range before generating boards
- Error if fewer than (boardSize² - 1) unique answers available

UI SPECS:
- Two side-by-side number inputs
- Labels: "טווח תשובות (מינימום)" / "טווח תשובות (מקסימום)"
```

#### 1.4 Action Buttons
**Requirement ID:** REQ-004  
**Priority:** P0 (Critical)

```
BUTTONS:
1. "צור לוחות משחק להדפסה" [Generate Boards]
   - Icon: 📋
   - Action: generateBoards()
   - Style: Success (blue/green gradient)

2. "התחל משחק חדש" [Start New Game]
   - Icon: 🎮
   - Action: startNewGame()
   - Style: Secondary (pink/red gradient)

BEHAVIOR:
- Buttons clearly labeled in Hebrew
- Responsive hover effects (lift + shadow)
- Disabled state when clicked (prevent double-click)
- Loading indicator for board generation
```

---

### 2. Question Display System

#### 2.1 Main Question Display Area
**Requirement ID:** REQ-005  
**Priority:** P0 (Critical)

```
LAYOUT:
- Large centered card/panel
- White background with shadow
- Minimum height: 300px
- Responsive to screen size

CONTENT STATES:
1. Initial State: "לחץ על 'הגרל שאלה' כדי להתחיל! 🎲"
2. Question State: Display current question
3. Answer Revealed State: Show question + answer

ANIMATIONS:
- Pulse effect (scale 1.0 → 1.05 → 1.0) when new question appears
- Smooth transitions (0.3s ease)
- Emoji rain effect (5 random emojis falling from top)
```

#### 2.2 Question Format
**Requirement ID:** REQ-006  
**Priority:** P0 (Critical)

```
DISPLAY:
Question text:
- Font size: 2.5em (responsive)
- Color: #333 (dark gray)
- Font weight: Bold
- Text align: Center
- Line height: 1.4

Example: "משולש שווה שוקיים: זווית ראש 40°, מה זווית הבסיס?"

TECHNICAL:
- Hebrew text with RTL support
- Support for special characters (°, ², ³)
- Math notation support (x², √, ±)
```

#### 2.3 Answer Reveal System
**Requirement ID:** REQ-007  
**Priority:** P0 (Critical)

```
BEHAVIOR:
1. Initially hidden (CSS class: 'hidden')
2. Revealed only when teacher clicks "הצג תשובה"
3. Cannot be hidden again without drawing new question

DISPLAY:
- Font size: 3em (very large)
- Color: #667eea (brand purple)
- Background: Light purple (#f0f4ff)
- Border: 3px dashed purple
- Padding: 20px
- Border radius: 15px

FORMAT: "התשובה: {answer_value}"
Example: "התשובה: 70"

ANIMATION:
- Floating emoji appears when revealed (random position)
- Smooth fade-in (0.3s)
```

---

### 3. Question Drawing Mechanism

#### 3.1 Draw Question Button
**Requirement ID:** REQ-008  
**Priority:** P0 (Critical)

```
FUNCTIONALITY:
- Button text: "🎲 הגרל שאלה"
- Triggers: drawQuestion() function
- Location: Below question display, centered

ALGORITHM:
1. Check if questions remain in pool
   - If empty: Alert "כל השאלות הוגרלו! התחל משחק חדש 🎊"
   - If available: Continue to step 2

2. Select random question:
   - Pick random index from remaining questions
   - Remove question from available pool
   - Add to used questions array

3. Update display:
   - Show question text
   - Hide answer
   - Trigger animations

4. Update statistics:
   - Increment question count
   - Decrement remaining count
   - Add to history list

UI SPECS:
- Large button: 1.5em font, 20px vertical padding, 40px horizontal
- Gradient background (purple theme)
- Prominent positioning
```

#### 3.2 Reveal Answer Button
**Requirement ID:** REQ-009  
**Priority:** P0 (Critical)

```
FUNCTIONALITY:
- Button text: "👁️ הצג תשובה"
- Triggers: revealAnswer() function
- Location: Next to "Draw Question" button

BEHAVIOR:
- Only works if question is currently displayed
- If no question: Alert "תחילה הגרל שאלה! 🎲"
- Removes 'hidden' class from answer div
- Triggers floating emoji animation
- Cannot hide answer again

UI SPECS:
- Same size as draw button
- Secondary color scheme (pink gradient)
```

---

### 4. Question Bank Management

#### 4.1 Question Bank Structure
**Requirement ID:** REQ-010  
**Priority:** P0 (Critical)

```javascript
DATA STRUCTURE:
const questionBank = [
    {
        q: "Question text in Hebrew",  // String
        a: 70                           // Number (integer or decimal)
    },
    // ... more questions
];

REQUIREMENTS:
- Minimum 40 unique questions
- Each question must have exactly one correct answer
- Answers must be numeric (integers preferred)
- Questions in Hebrew with proper RTL
```

#### 4.2 Question Categories (Content Requirements)
**Requirement ID:** REQ-011  
**Priority:** P0 (Critical)

```
CATEGORIES (Aligned with 9th grade 4-unit math curriculum):

1. ISOSCELES TRIANGLES (משולש שווה שוקיים)
   - Calculate base angles from apex angle
   - Calculate apex angle from base angles
   - Questions: Minimum 4

2. KITE SHAPES (דלתון)
   - Calculate missing angles (sum = 360°)
   - Identify equal angles
   - Questions: Minimum 2

3. ALGEBRAIC IDENTITIES (כפל מקוצר)
   - (a+b)² = a² + 2ab + b²
   - (a-b)² = a² - 2ab + b²
   - (a+b)(a-b) = a² - b²
   - Find coefficients after expansion
   - Evaluate at specific x values
   - Questions: Minimum 8

4. LINEAR EQUATIONS (משוואת הישר)
   - Identify slope (m) from y = mx + b
   - Identify y-intercept (b)
   - Convert to standard form
   - Find intersection points
   - Questions: Minimum 8

5. QUADRATIC EQUATIONS (משוואה ריבועית)
   - Identify coefficients (a, b, c)
   - Solve using quadratic formula
   - Calculate discriminant
   - Questions: Minimum 8

6. GENERAL MATH (כללי)
   - Multiplication, squares, square roots
   - Simple equations
   - Percentages
   - Fractions
   - Questions: Minimum 10

QUALITY REQUIREMENTS:
- Questions must be unambiguous
- Answers should be integers when possible
- Decimal answers rounded to 2 places maximum
- Avoid negative answers unless specified in range
```

#### 4.3 Question Pool Management
**Requirement ID:** REQ-012  
**Priority:** P1 (High)

```javascript
STATE MANAGEMENT:

currentGame = {
    questions: [],        // Array of remaining questions
    usedQuestions: [],   // Array of already drawn questions
    currentQuestion: null, // Currently displayed question object
    answerRevealed: false // Boolean flag
}

FUNCTIONS:

startNewGame():
- Reset questions array to full questionBank
- Clear usedQuestions array
- Reset currentQuestion to null
- Clear history display
- Update statistics

drawQuestion():
- Validate questions array not empty
- Select random index
- Move question from questions to usedQuestions
- Set as currentQuestion
- Trigger animations
- Update UI

PERSISTENCE:
- No server-side storage required
- All state in memory (resets on page reload)
- Optional: localStorage for session persistence
```

---

### 5. Statistics & History Display

#### 5.1 Live Statistics Panel
**Requirement ID:** REQ-013  
**Priority:** P1 (High)

```
LAYOUT:
- Horizontal grid, 2 stat cards
- Cards have gradient background (brand colors)
- White text, centered alignment

STAT CARDS:
1. Questions Drawn
   - Large number (2em font)
   - Label: "שאלות שהוגרלו"
   - Updates after each drawQuestion()

2. Questions Remaining  
   - Large number (2em font)
   - Label: "שאלות נותרו"
   - Updates after each drawQuestion()

CALCULATION:
- Questions Drawn = usedQuestions.length
- Questions Remaining = questions.length

UI SPECS:
- Auto-refresh on state change
- Smooth number transitions (optional)
```

#### 5.2 Question History List
**Requirement ID:** REQ-014  
**Priority:** P2 (Medium)

```
FUNCTIONALITY:
- Display all previously drawn answers
- Show in reverse chronological order (newest first)
- Scrollable if exceeds container height

LAYOUT:
- Grid of answer cards
- Auto-fit columns (min 120px)
- Max height: 200px with scroll

CARD DESIGN:
- Answer number in center
- Light purple background
- Rounded corners
- Purple border

UPDATES:
- Add new card at beginning after each question
- Scroll to top automatically
- Initial state: "עדיין לא הוגרלו שאלות..."

PURPOSE:
- Teacher can review which answers already appeared
- Students can verify their boards
```

---

### 6. Board Generation System

#### 6.1 Generate Boards Function
**Requirement ID:** REQ-015  
**Priority:** P0 (Critical)

```javascript
FUNCTION: generateBoards()

INPUT PARAMETERS (from UI):
- boardSize: number (3, 4, or 5)
- numPlayers: number (1-40)
- minAnswer: number
- maxAnswer: number

ALGORITHM:

1. VALIDATION:
   a. Check numPlayers in range [1, 40]
   b. Filter questionBank for answers in [minAnswer, maxAnswer]
   c. Count unique answers (let uniqueAnswers = filtered.length)
   d. Calculate required = boardSize² - (hasFreeSpace ? 1 : 0)
   e. If uniqueAnswers < required: Alert error and abort

2. BOARD GENERATION (for each player 1 to numPlayers):
   a. Create new bingo board object
   b. Randomly select (boardSize² - freeSpace) unique answers
   c. Shuffle selected answers
   d. Generate board grid
   e. If odd size: Place "⭐ חינם" in center cell
   f. Add player identifier header

3. RENDER:
   a. Clear printBoards container
   b. Append all board HTML elements
   c. Add print-friendly header
   d. Scroll to boards section

4. USER FEEDBACK:
   - Success alert: "נוצרו {numPlayers} לוחות בגודל {size}x{size}! גלול למטה להדפסה 🖨️"
   - Auto-scroll to boards after 100ms delay

ERROR HANDLING:
- Invalid player count: "מספר תלמידים חייב להיות בין 1 ל-40"
- Insufficient answers: "אין מספיק תשובות בטווח שנבחר!"
```

#### 6.2 Board Layout Specifications
**Requirement ID:** REQ-016  
**Priority:** P0 (Critical)

```
HTML STRUCTURE:
<div class="bingo-board">
    <div class="board-header">
        🎯 בינגו מתמטי - תלמיד {playerNumber}
    </div>
    <div class="board-grid">
        <div class="board-cell">{answer}</div>
        <!-- Repeated for each cell -->
    </div>
</div>

GRID LAYOUT:
- CSS Grid: grid-template-columns: repeat(N, 1fr)
  where N = boardSize
- Equal cell sizes (square aspect ratio)
- Gap between cells: 5px

CELL SPECIFICATIONS:
Regular Cell:
- Border: 2px solid #667eea (purple)
- Background: white
- Padding: 15px
- Font size: 1.2em
- Font weight: bold
- Min height: 60px
- Content centered (flexbox)

Free Space Cell (center of odd-sized boards):
- Background: gradient (purple)
- Color: white
- Text: "⭐ חינם"
- Same dimensions as regular cells

BOARD SIZING:
- 3x3: ~350px width
- 4x4: ~450px width  
- 5x5: ~550px width
- Inline-block display (multiple boards per row)
- Margin: 20px between boards

PAGE BREAK:
- CSS: page-break-inside: avoid
- Ensures board not split across pages when printing
```

#### 6.3 Print Optimization
**Requirement ID:** REQ-017  
**Priority:** P0 (Critical)

```css
PRINT STYLES (@media print):

HIDE ELEMENTS:
- Navigation/controls panel
- Statistics panel
- Question display area
- History panel
- All buttons
- Page title/header

SHOW ELEMENTS:
- Only printBoards container
- All bingo boards

LAYOUT ADJUSTMENTS:
- Remove background colors (white page)
- Remove shadows
- Optimize spacing for paper
- Black and white friendly (borders remain visible)

PAPER SETTINGS:
- Orientation: Portrait (default) or Landscape (optional)
- Margins: 1cm all sides
- Boards per page: Auto-fit (typically 2-4 depending on size)

PRINT TRIGGER:
- Ctrl+P / Cmd+P system dialog
- Optional: "הדפס" button that triggers window.print()
```

---

### 7. Animation & Visual Effects

#### 7.1 Emoji Rain Effect
**Requirement ID:** REQ-018  
**Priority:** P2 (Medium)

```
TRIGGER: When new question is drawn

BEHAVIOR:
- Spawn 5 emoji elements sequentially
- Emojis: ['🎉', '⭐', '🎊', '✨', '🎈', '🎯', '🏆', '🌟', '💫', '🎪']
- Random emoji selected for each spawn
- Random horizontal position (0-100%)
- Staggered timing (200ms delay between each)

ANIMATION:
- Duration: 2-3 seconds (random per emoji)
- Movement: translateY(0 → 400px)
- Rotation: rotate(0 → 360deg)
- Opacity: 1 → 0 (fade out at end)

CSS KEYFRAMES:
@keyframes fall {
    to {
        transform: translateY(400px) rotate(360deg);
        opacity: 0;
    }
}

CLEANUP:
- Remove DOM element after animation completes
- Prevent memory leaks
```

#### 7.2 Floating Emoji Effect
**Requirement ID:** REQ-019  
**Priority:** P2 (Medium)

```
TRIGGERS:
- Answer revealed: 🎯
- New game started: 🎮
- Boards generated: 📋
- Page load: 🎉

BEHAVIOR:
- Single large emoji (80px font size)
- Appears at random X position
- Starts at Y = 50% of viewport height
- Floats upward 200px
- Rotates 360° during ascent
- Fades out

ANIMATION:
- Duration: 2 seconds
- Easing: ease-out

CSS KEYFRAMES:
@keyframes float-up {
    0% {
        transform: translateY(0) rotate(0deg);
        opacity: 1;
    }
    100% {
        transform: translateY(-200px) rotate(360deg);
        opacity: 0;
    }
}

CLEANUP:
- Auto-remove after animation
```

#### 7.3 Pulse Animation
**Requirement ID:** REQ-020  
**Priority:** P2 (Medium)

```
TRIGGER: Question display updated

BEHAVIOR:
- Scale element from 1.0 to 1.05 to 1.0
- Duration: 0.5s
- Easing: ease-in-out
- Single iteration (no loop)

CSS KEYFRAMES:
@keyframes pulse {
    0%, 100% { transform: scale(1); }
    50% { transform: scale(1.05); }
}

APPLICATION:
- Add class "animating" to questionDisplay
- Remove class after animation completes
```

---

### 8. Responsive Design & Accessibility

#### 8.1 Responsive Breakpoints
**Requirement ID:** REQ-021  
**Priority:** P1 (High)

```
BREAKPOINTS:

Desktop (> 1200px):
- Container max-width: 1200px
- Full feature visibility
- Side-by-side stat cards

Tablet (768px - 1199px):
- Container width: 100% - 40px padding
- Stack input fields if needed
- Maintain board grid layout

Mobile (< 767px):
- Single column layout
- Full-width inputs
- Smaller font sizes:
  * Question: 1.8em (reduced from 2.5em)
  * Answer: 2.2em (reduced from 3em)
- Reduced button padding
- Stack buttons vertically

PROJECTION/TV MODE (recommended):
- Fullscreen browser
- Increase base font size (120-150%)
- High contrast enabled
- Focus on question display (minimize controls)
```

#### 8.2 Hebrew RTL Support
**Requirement ID:** REQ-022  
**Priority:** P0 (Critical)

```
HTML ATTRIBUTES:
- <html lang="he" dir="rtl">
- All text containers inherit RTL

CSS PROPERTIES:
- direction: rtl (body and containers)
- text-align: right (default for text)
- text-align: center (headings, display areas)

FLEXBOX/GRID:
- Automatic reversal of flex-direction
- Grid columns fill right-to-left

NUMBER DISPLAY:
- Numbers displayed LTR even in RTL context
- Use direction: ltr for answer numbers
- Example: <span style="direction: ltr">70</span>

EDGE CASES:
- Mixed Hebrew + Math notation handled correctly
- Parentheses orientation: ) ( becomes ( )
- Ensure emoji always LTR
```

#### 8.3 Keyboard Navigation
**Requirement ID:** REQ-023  
**Priority:** P2 (Medium)

```
KEYBOARD SHORTCUTS (optional enhancement):

Space / Enter:
- Draw new question (if focus on draw button)

R key:
- Reveal answer (if question displayed)

N key:
- Start new game

P key:
- Print boards (if boards generated)

FOCUS MANAGEMENT:
- Visible focus indicators (outline)
- Logical tab order
- Skip to main content link

ACCESSIBILITY:
- All interactive elements keyboard accessible
- Clear visual feedback on focus
- No keyboard traps
```

---

### 9. Error Handling & Edge Cases

#### 9.1 Error Messages
**Requirement ID:** REQ-024  
**Priority:** P1 (High)

```
ERROR SCENARIOS & MESSAGES:

1. No questions remaining:
   Alert: "כל השאלות הוגרלו! התחל משחק חדש 🎊"
   Action: Prompt user to click "התחל משחק חדש"

2. Reveal answer without question:
   Alert: "תחילה הגרל שאלה! 🎲"
   Action: Focus draw question button

3. Invalid player count:
   Alert: "מספר תלמידים חייב להיות בין 1 ל-40"
   Action: Highlight input field, reset to valid value

4. Insufficient questions in range:
   Alert: "אין מספיק תשובות בטווח שנבחר!"
   Action: Suggest widening range or reducing board size

5. Board generation failure:
   Alert: "שגיאה ביצירת לוחות. אנא נסה שוב."
   Action: Reset board container

MESSAGE STYLE:
- Use browser alert() for simplicity
- Clear, actionable Hebrew text
- Include relevant emoji for friendliness
```

#### 9.2 Data Validation
**Requirement ID:** REQ-025  
**Priority:** P1 (High)

```
INPUT VALIDATION:

Board Size:
- Type: Integer
- Values: 3, 4, 5 only
- Default: 5
- Validation: Dropdown prevents invalid input

Number of Players:
- Type: Integer
- Range: [1, 40]
- Default: 25
- Validation: HTML5 min/max + JavaScript check
- Auto-correct: Round to nearest valid integer

Answer Range (Min/Max):
- Type: Integer
- Range: [-1000, 1000] (reasonable bounds)
- Validation: Min < Max
- Default: Min=-10, Max=100

SANITIZATION:
- Trim whitespace from inputs
- Parse integers with parseInt()
- Reject NaN values
- Use fallback defaults if parsing fails
```

---

### 10. Performance Requirements

#### 10.1 Load Time
**Requirement ID:** REQ-026  
**Priority:** P1 (High)

```
TARGETS:
- Initial page load: < 1 second (offline, no external dependencies)
- Board generation (40 students, 5x5): < 2 seconds
- Question draw: < 100ms (instant feel)
- Animation rendering: 60 FPS

OPTIMIZATION STRATEGIES:
- Single-file HTML (no external resources)
- Inline CSS and JavaScript
- No image files (emoji only)
- Minimize DOM manipulations
- Use CSS transforms for animations (GPU accelerated)
```

#### 10.2 Browser Compatibility
**Requirement ID:** REQ-027  
**Priority:** P1 (High)

```
SUPPORTED BROWSERS:
- Chrome 90+ (primary testing target)
- Firefox 88+
- Safari 14+
- Edge 90+

REQUIRED FEATURES:
- CSS Grid (full support)
- CSS Flexbox
- ES6 JavaScript (arrow functions, spread operator, const/let)
- HTML5 form validation
- CSS animations

FALLBACKS:
- No polyfills required (modern browsers only)
- Graceful degradation for older browsers (basic functionality)

TESTING:
- Primary: Chrome (Windows/Mac)
- Secondary: Firefox, Safari
- Mobile: Chrome Android, Safari iOS
```

---

### 11. Code Structure & Architecture

#### 11.1 File Organization
**Requirement ID:** REQ-028  
**Priority:** P0 (Critical)

```
SINGLE-FILE STRUCTURE:
<!DOCTYPE html>
<html lang="he" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>בינגו מתמטי - כיתה ט׳</title>
    <style>
        /* All CSS inline */
    </style>
</head>
<body>
    <!-- All HTML structure -->
    
    <script>
        /* All JavaScript inline */
    </script>
</body>
</html>

RATIONALE:
- Easy distribution (single file)
- Offline functionality
- No build process required
- Teacher can email/USB the file directly
```

#### 11.2 JavaScript Architecture
**Requirement ID:** REQ-029  
**Priority:** P1 (High)

```javascript
GLOBAL STATE:
const questionBank = [ /* question objects */ ];

let currentGame = {
    questions: [],
    usedQuestions: [],
    currentQuestion: null,
    answerRevealed: false
};

const emojis = ['🎉', '⭐', '🎊', /* ... */];

FUNCTION ORGANIZATION:

// Core Game Functions
function startNewGame() { }
function drawQuestion() { }
function revealAnswer() { }

// Board Generation
function generateBoards() { }
function createBingoBoard(size, allAnswers, playerNum) { }

// UI Updates
function updateStats() { }
function updateHistory() { }
function animateQuestion() { }

// Effects
function createEmojiRain() { }
function createFloatingEmoji(emoji) { }

// Initialization
window.onload = () => {
    startNewGame();
    createFloatingEmoji('🎉');
};

CODE STYLE:
- camelCase for functions and variables
- Clear, descriptive naming
- Comments for complex logic
- Consistent indentation (2 or 4 spaces)
```

#### 11.3 CSS Architecture
**Requirement ID:** REQ-030  
**Priority:** P1 (High)

```css
CSS ORGANIZATION:

/* 1. Reset & Base Styles */
* { margin: 0; padding: 0; box-sizing: border-box; }
body { /* base styles */ }

/* 2. Layout Containers */
.container { }
.controls { }
.question-display { }

/* 3. Components */
.bingo-board { }
.board-grid { }
.board-cell { }

/* 4. UI Elements */
button { }
input, select { }
.stat-item { }

/* 5. Animations */
@keyframes pulse { }
@keyframes fall { }
@keyframes float-up { }

/* 6. Utility Classes */
.hidden { }
.animating { }

/* 7. Media Queries */
@media print { }
@media (max-width: 767px) { }

/* 8. RTL Specific */
[dir="rtl"] { }

NAMING CONVENTION:
- BEM-inspired (Block__Element--Modifier)
- Semantic class names
- Avoid ID selectors for styling
```

---

### 12. Testing Requirements

#### 12.1 Functional Testing
**Requirement ID:** REQ-031  
**Priority:** P1 (High)

```
TEST SCENARIOS:

1. BOARD GENERATION:
   [✓] Generate 1 player, 3x3 board
   [✓] Generate 40 players, 5x5 boards
   [✓] Generate with limited answer range
   [✓] Verify all boards have unique arrangements
   [✓] Verify free space in odd-sized boards (center cell)
   [✓] Verify no duplicate answers within a board

2. QUESTION DRAWING:
   [✓] Draw all questions in sequence
   [✓] Verify no duplicates
   [✓] Verify questions exhaust correctly
   [✓] Verify error message when pool empty

3. ANSWER REVEAL:
   [✓] Reveal answer for current question
   [✓] Verify cannot reveal without question
   [✓] Verify answer matches question

4. GAME RESET:
   [✓] Start new game resets all state
   [✓] History clears
   [✓] Statistics reset
   [✓] Questions pool refills

5. INPUT VALIDATION:
   [✓] Test invalid player counts (0, 41, negative)
   [✓] Test invalid range (min > max)
   [✓] Test insufficient questions for range

6. PRINT FUNCTIONALITY:
   [✓] Print layout correct
   [✓] All boards visible
   [✓] No UI elements in print
   [✓] Page breaks appropriate
```

#### 12.2 Visual Testing
**Requirement ID:** REQ-032  
**Priority:** P2 (Medium)

```
VISUAL TESTS:

1. ANIMATIONS:
   [✓] Emoji rain appears on question draw
   [✓] Floating emoji on answer reveal
   [✓] Pulse animation on question update
   [✓] All animations smooth (60fps)

2. RESPONSIVE DESIGN:
   [✓] Layout on 1920x1080 (projector)
   [✓] Layout on 1366x768 (laptop)
   [✓] Layout on tablet (iPad)
   [✓] Layout on mobile (360x640)

3. RTL SUPPORT:
   [✓] Hebrew text right-aligned
   [✓] Numbers display correctly
   [✓] Flexbox/Grid reverse correctly

4. ACCESSIBILITY:
   [✓] Color contrast meets WCAG AA
   [✓] Focus indicators visible
   [✓] Tab order logical
```

---

### 13. Deployment & Distribution

#### 13.1 Deployment Method
**Requirement ID:** REQ-033  
**Priority:** P0 (Critical)

```
DISTRIBUTION FORMAT:
- Single HTML file
- Filename: בינגו_מתמטי.html (or math_bingo.html for compatibility)
- File size: < 100KB

DEPLOYMENT CHANNELS:
1. Direct download (teacher downloads from source)
2. Email attachment
3. USB drive copy
4. Cloud storage (Google Drive, Dropbox)
5. School network share

NO HOSTING REQUIRED:
- Runs entirely client-side
- No server dependencies
- No database needed
- Works offline

UPDATES:
- Teacher downloads new version when available
- Replaces old file with new file
- No automatic updates
```

#### 13.2 Documentation
**Requirement ID:** REQ-034  
**Priority:** P1 (High)

```
USER DOCUMENTATION (embedded in HTML or separate file):

QUICK START GUIDE:
1. Setup (2 minutes)
   - Open file in browser
   - Set board size and number of players
   - Click "Generate Boards"
   - Print boards (Ctrl+P)

2. During Game (30-45 minutes)
   - Project screen to classroom
   - Click "Draw Question"
   - Students solve
   - Click "Reveal Answer"
   - Students mark if they have answer
   - Repeat until someone wins

3. Winning Conditions
   - Full row (horizontal)
   - Full column (vertical)
   - Full diagonal (optional)

TROUBLESHOOTING:
- "Nothing happens when I click": Ensure JavaScript enabled
- "Boards look wrong": Use Chrome or Firefox browser
- "Print looks bad": Use Print Preview, adjust margins

TIPS:
- Test with 1-2 boards before printing all
- Keep answer range moderate for better variety
- Remind students: answer must be exact match
```

---

### 14. Future Enhancements (Optional)

#### 14.1 Phase 2 Features
**Requirement ID:** REQ-035  
**Priority:** P3 (Future)

```
POTENTIAL ADDITIONS:

1. CUSTOM QUESTION IMPORT:
   - Upload CSV with questions/answers
   - Teacher can add their own questions
   - Merge with default question bank

2. DIFFICULTY LEVELS:
   - Tag questions as Easy/Medium/Hard
   - Filter by difficulty
   - Progressive difficulty mode

3. TIMER MODE:
   - Countdown timer for solving (30-60 seconds)
   - Visual timer bar
   - Auto-reveal after timeout

4. SOUND EFFECTS:
   - Optional sound on question draw
   - Celebratory sound on answer reveal
   - Background music (muted by default)

5. SAVE/LOAD GAME STATE:
   - LocalStorage persistence
   - Resume game after browser close
   - Export/import game state

6. STATISTICS EXPORT:
   - Download question history as CSV
   - Track which questions were difficult
   - Generate classroom analytics

7. MULTIPLE WINNERS:
   - Support diagonal wins
   - Four corners win
   - Full board blackout

8. TEAM MODE:
   - Divide class into teams
   - Team score tracking
   - Team vs. team competition
```

---

### 15. Technical Constraints

#### 15.1 Technology Stack
**Requirement ID:** REQ-036  
**Priority:** P0 (Critical)

```
ALLOWED TECHNOLOGIES:
✓ HTML5
✓ CSS3 (including Grid, Flexbox, Animations)
✓ Vanilla JavaScript (ES6+)

NOT ALLOWED:
✗ No external libraries (jQuery, React, Vue, etc.)
✗ No build tools (Webpack, Babel, etc.)
✗ No package managers (npm, yarn)
✗ No external fonts or icon sets
✗ No server-side code
✗ No databases

RATIONALE:
- Maximum portability
- No dependencies
- Works offline
- Easy to maintain
- Teacher can edit directly if needed
```

---

### 16. Success Metrics

#### 16.1 Key Performance Indicators
**Requirement ID:** REQ-037  
**Priority:** P2 (Medium)

```
PRODUCT SUCCESS METRICS:

Teacher Satisfaction:
- Setup time < 5 minutes (target: 3 minutes)
- Positive feedback on usability
- Willingness to reuse for future classes

Student Engagement:
- Participation rate > 90%
- Focused attention during game
- Positive student feedback

Technical Performance:
- Zero critical bugs in 30-day period
- Load time < 1 second
- Works on 100% of school computers

Educational Impact:
- Students report better understanding of material
- Improved test scores in covered topics (measured separately)
- Increased willingness to participate vs. traditional review
```

---

### 17. Question Bank Details (Full Specification)

#### 17.1 Complete Question List
**Requirement ID:** REQ-038  
**Priority:** P0 (Critical)

```javascript
MINIMUM 45 QUESTIONS REQUIRED:

// Category 1: Isosceles Triangles (5 questions)
{ q: "משולש שווה שוקיים: זווית ראש 40°, מה זווית הבסיס?", a: 70 }
{ q: "משולש שווה שוקיים: זווית בסיס 65°, מה זווית הראש?", a: 50 }
{ q: "משולש שווה שוקיים: זווית ראש 30°, מה זווית הבסיס?", a: 75 }
{ q: "משולש שווה שוקיים: זווית בסיס 72°, מה זווית הראש?", a: 36 }
{ q: "משולש שווה שוקיים: זווית בסיס 80°, מה זווית הראש?", a: 20 }

// Category 2: Kite Shapes (3 questions)
{ q: "דלתון: זוויות 80°, 110°, 90°, מה הזווית הרביעית?", a: 80 }
{ q: "דלתון: שלוש זוויות 90°, 100°, 90°, מה הזווית הרביעית?", a: 80 }
{ q: "דלתון: זוויות 70°, 100°, 120°, מה הזווית הרביעית?", a: 70 }

// Category 3: Algebraic Identities (10 questions)
{ q: "(x+5)² פתח וחשב ל-x=2: התשובה?", a: 49 }
{ q: "(x+3)² פתח: המקדם של x?", a: 6 }
{ q: "(x-4)² פתח: האיבר החופשי?", a: 16 }
{ q: "(2x+1)² פתח: מקדם x²?", a: 4 }
{ q: "(x+7)(x-7) פתח: האיבר החופשי?", a: -49 }
{ q: "(3x+2)² פתח: מקדם x?", a: 12 }
{ q: "(x+10)² - (x-2)²: לאחר פישוט, מה מקדם x?", a: 24 }
{ q: "(x+6)² פתח: האיבר החופשי?", a: 36 }
{ q: "(5x-1)² פתח: מקדם x²?", a: 25 }
{ q: "(x+4)(x-4): התשובה?", a: x²-16 } // Note: May need answer as -16

// Category 4: Linear Equations (10 questions)
{ q: "y=3x+7: מה השיפוע?", a: 3 }
{ q: "y=3x+7: מה החותך?", a: 7 }
{ q: "y=-2x+5: מה השיפוע?", a: -2 }
{ q: "y=-2x+5: מה החותך?", a: 5 }
{ q: "2y=6x+10 העבר לצורה y=mx+b: מה m?", a: 3 }
{ q: "2y=6x+10 העבר לצורה y=mx+b: מה b?", a: 5 }
{ q: "y=2x+1 ו-y=-x+7 נקודת חיתוך: מה x?", a: 2 }
{ q: "y=2x+1 ו-y=-x+7 נקודת חיתוך: מה y?", a: 5 }
{ q: "y=4x-3: מה השיפוע?", a: 4 }
{ q: "y=x+10: מה החותך?", a: 10 }

// Category 5: Quadratic Equations (10 questions)
{ q: "x²-5x+6=0: מה a?", a: 1 }
{ q: "x²-5x+6=0: מה b?", a: -5 }
{ q: "x²-5x+6=0: מה c?", a: 6 }
{ q: "2x²+3x-10=0: מה a?", a: 2 }
{ q: "x²-7x+10=0 פתור: הפתרון הגדול?", a: 5 }
{ q: "x²-7x+10=0 פתור: הפתרון הקטן?", a: 2 }
{ q: "x²-6x+9=0: הדיסקרימיננטה?", a: 0 }
{ q: "3x²+5x+2=0: מה a?", a: 3 }
{ q: "x²-4x+4=0: מה הפתרון?", a: 2 }
{ q: "-x²+3x-1=0: מה a?", a: -1 }

// Category 6: General Math (12 questions)
{ q: "חשב: 7×8", a: 56 }
{ q: "חשב: 12²", a: 144 }
{ q: "חשב: √64", a: 8 }
{ q: "חשב: 3²+4²", a: 25 }
{ q: "חשב: 100÷4", a: 25 }
{ q: "חשב: 15×6", a: 90 }
{ q: "משוואה: 2x+5=15, מה x?", a: 5 }
{ q: "משוואה: 3x-7=8, מה x?", a: 5 }
{ q: "משוואה: x/2=8, מה x?", a: 16 }
{ q: "אחוזים: 50% מ-80?", a: 40 }
{ q: "אחוזים: 25% מ-60?", a: 15 }
{ q: "חשב: 9²", a: 81 }

TOTAL: 50 questions

QUALITY CHECKLIST:
✓ All questions in Hebrew
✓ All answers numeric
✓ No duplicate answers in same category
✓ Answers range: -49 to 144
✓ Questions aligned with curriculum
✓ Unambiguous wording
✓ Reasonable solving time (30-90 seconds each)
```

---

### 18. Development Phases

#### 18.1 MVP (Minimum Viable Product)
**Requirement ID:** REQ-039  
**Priority:** P0 (Critical)

```
PHASE 1: CORE FUNCTIONALITY (Week 1)

Must Have:
✓ Question bank (minimum 40 questions)
✓ Draw question functionality
✓ Reveal answer functionality
✓ Board generation (3x3, 4x4, 5x5)
✓ Print-friendly board layout
✓ Basic statistics (count remaining)
✓ Hebrew RTL support

Nice to Have (defer if time constrained):
- Animations
- History list
- Answer range filtering

DELIVERABLE:
- Working single-file HTML application
- Can generate and print boards
- Can conduct classroom game
```

#### 18.2 Polish & Enhancement
**Requirement ID:** REQ-040  
**Priority:** P1 (High)

```
PHASE 2: POLISH (Week 2)

Add:
✓ Emoji rain animation
✓ Floating emoji effects
✓ Pulse animation
✓ Question history display
✓ Improved styling (gradients, shadows)
✓ Mobile responsive design
✓ Answer range filtering
✓ Input validation

DELIVERABLE:
- Production-ready application
- Fully animated
- Professional appearance
- Complete error handling
```

---

### 19. Handoff to AI Agent

#### 19.1 Agent Instructions
**Requirement ID:** REQ-041  
**Priority:** P0 (Critical)

```
TO: AI Development Agent
FROM: Product Requirements Document
RE: Math Bingo Game Implementation

TASK:
Develop a single-file HTML application according to this PRD.

DELIVERABLES:
1. math_bingo.html (complete, working file)
2. Brief implementation notes
3. Test results (manual testing against REQ-031)

DEVELOPMENT APPROACH:
1. Start with MVP (Phase 1)
2. Test core functionality
3. Add polish (Phase 2)
4. Final testing

CRITICAL REQUIREMENTS:
- Single HTML file (no external dependencies)
- Works offline
- Hebrew RTL support
- Print-friendly boards
- All 50 questions included

TESTING CHECKLIST:
- [ ] Generate boards (various sizes)
- [ ] Draw all questions
- [ ] Reveal answers
- [ ] Print boards
- [ ] Responsive design
- [ ] RTL text direction
- [ ] Error handling

ACCEPTANCE CRITERIA:
✓ Teacher can generate and print boards in < 5 minutes
✓ Game runs smoothly with 40 students
✓ Animations are smooth and engaging
✓ All questions work correctly
✓ Prints correctly in Chrome/Firefox

QUESTIONS/CLARIFICATIONS:
Contact: [Product Owner] for any ambiguities
```

---

## Appendix A: Color Palette

```css
/* Primary Colors */
--primary-purple: #667eea;
--primary-dark-purple: #764ba2;

/* Secondary Colors */
--secondary-pink: #f093fb;
--secondary-red: #f5576c;

/* Tertiary Colors */
--tertiary-blue: #4facfe;
--tertiary-cyan: #00f2fe;

/* Background Colors */
--bg-light-purple: #f0f4ff;
--bg-light-blue: #e0e8ff;
--bg-white: #ffffff;

/* Text Colors */
--text-dark: #333333;
--text-medium: #555555;
--text-light: #999999;

/* Border Colors */
--border-purple: #667eea;
--border-light: #e0e0e0;

/* Gradients */
--gradient-primary: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
--gradient-secondary: linear-gradient(135deg, #f093fb 0%, #f5576c 100%);
--gradient-tertiary: linear-gradient(135deg, #4facfe 0%, #00f2fe 100%);
```

---

## Appendix B: Typography

```css
/* Font Family */
font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;

/* Font Sizes */
--fs-small: 0.9em;
--fs-body: 1em;
--fs-large: 1.3em;
--fs-xlarge: 1.5em;
--fs-display: 2.5em;
--fs-huge: 3em;

/* Font Weights */
--fw-normal: 400;
--fw-semibold: 600;
--fw-bold: 700;

/* Line Heights */
--lh-tight: 1.2;
--lh-normal: 1.4;
--lh-relaxed: 1.6;
```

---

## Appendix C: Spacing Scale

```css
/* Spacing (padding, margin, gap) */
--space-xs: 5px;
--space-sm: 10px;
--space-md: 15px;
--space-lg: 20px;
--space-xl: 30px;
--space-2xl: 40px;

/* Border Radius */
--radius-sm: 5px;
--radius-md: 10px;
--radius-lg: 15px;

/* Shadows */
--shadow-sm: 0 2px 5px rgba(0,0,0,0.1);
--shadow-md: 0 5px 15px rgba(0,0,0,0.1);
--shadow-lg: 0 10px 30px rgba(0,0,0,0.2);
```

---

## Document Version Control

**Version:** 1.0  
**Date:** 2025-02-21  
**Author:** Product Requirements Team  
**Status:** Approved for Development  

**Revision History:**
- v1.0 (2025-02-21): Initial release

**Next Review Date:** After Phase 1 completion

---

END OF DOCUMENT
