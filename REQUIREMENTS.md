# Screenplay Editor & Chatbot App: Requirements Canvas

## General Purpose
This document details the requirements for a screenplay app with AI integration. The basic purpose of this app is to have a classical screenplay editor (like Studiobinder) connected to AI.

## 1. Overall Layout

- Split-Screen Interface
  - Left Pane: AI Chat Interface
  - Right Pane: Screenplay Text Editor
  - Divider: Draggable line for dynamic resizing
  - Resizable Panes: Maintain min width for each pane; smooth real-time feedback
  - Use modular, easy to replace components (UI will be edited heavily)

## 2. Chat Pane (Left)

### 2.1 Model Selection

- Model Dropdown
  - Displays selected AI model with a menu for alternatives
  - Includes visual indicators of each model's capabilities
  - The default AI model is selected on default (To be decided which model is it)

### 2.2 Chat Modes

- General Chat Mode
  - Standard conversation with AI (no editing of screenplay text)

- Edit Mode (Version 2, not relevant for version 1)
  - AI can directly modify the screenplay document
  - Visible alert when in Edit Mode, plus confirmation prompts for suggested changes

### 2.3 Chat Organization

- Tabbed Interface
  - Each mode (General Chat vs. Edit Mode) in a separate tab
  - Persistent chat history in each mode
  - Easy switching between modes

### 2.4 Message Display

- User vs. AI Distinction
  - Clear styling differences
  - Optional timestamps
  - Message status indicators
  - Copy/Share message options

## 3. Screenplay Editor (Right)

### 3.1 Action Panel

- Location & Appearance
  - Fixed at the top of the editor pane
  - Always accessible buttons for each screenplay format action

- Actions
  - Scene Heading (Slugline)
  - Action (Description)
  - Character
  - Dialogue
  - Parenthetical
  - Transition
  - Shot
  - Additional standard elements as needed

### 3.2 Text Formatting Behaviors

#### 3.2.1 New Text Entry Mode

- Clicking an action button activates that format for future text
- If text is selected while clicking an action button, its current format will change based on the selected button
- The selected format remains active until changed again
- Pressing Enter to move to the next line will trigger automatic format transitions in specific cases:
  - Enter after "Character" changes the format to Dialogue
  - Enter after Slugline changes format to Action
  - Enter after Parenthetical changes format to Dialog

#### 3.2.2 Text Modification Mode

- Selecting text + clicking a format action = immediate reformat of highlighted text
- Does not change the active format for subsequent text

## 4. Specific Format Rules

### 4.1 Character Format

- Text: ALL CAPS
- Alignment: Centered
- Margins: Screenplay standard (typ. ~3" from left, ~2.5" from right)
- Features: Optional auto-suggest for frequently used character names

### 4.2 Dialogue Format

- Text: Normal case (not all caps)
- Alignment: Centered, narrower margins
- Placed: Directly under character name

### 4.3 Scene Heading (Slugline)

- Text: ALL CAPS, starts with INT./EXT.
- Alignment: Left-aligned

### 4.4 Action Format

- Text: Normal case
- Alignment: Left-aligned, full margins
- Spacing: Single.

### 4.5 Parenthetical Format

- Text: Normal case, wrapped in parentheses
- Margins: Slightly indented more than dialogue
- Width: Narrower than dialogue

## 5. Behavior States & Transitions

### 5.1 Active State Indicators

- Visual Feedback: Highlighted/"pressed" button for the current format

### 5.2 Format Transitions

- Maintain spacing, alignment, and partial custom styling
- Avoid abrupt shifts or losing content

### 5.3 Error Prevention

- To be decided later

### 5.4 Text Highlighting

- All the text, even of different formats can be highlighted (No isolation of text blocks)
- From the user side this is a single document (like Google Docs) just with "fancy" format options

## 6. Technical Implementation Notes

### 6.1 Format Storage

- Each text block stores:
  - Format type (e.g., Character, Dialogue, etc.)
  - Original text
  - Formatting metadata (margins, alignment)
  - Position in the document

### 6.2 Format Application

- Immediate Feedback: Changes apply instantly and are undoable/redoable
- Cursor Position: Preserve if possible when reformatting

### 6.3 Performance Requirements

- Speed: <100ms updates
- Large Document Handling: Smooth operation for lengthy screenplays
- Responsive Typing: No lag during typing or format switching

## 7. Implementation Recommendations

### 7.1 Screenplay Editor Implementation

- Leverage Existing Rich Text / Prose Libraries
  - ProseMirror (and its React-based wrapper Tiptap)
  - Slate.js (for React)
  - Draft.js (though less actively maintained nowadays)

### 7.2 Consider the "Fountain" Markup or Similar

- Screenwriters often use the Fountain format as a plain text markup
- Consider leveraging existing parsers like fountain-js

### 7.3 MVP First

- Implement a basic "Screenplay Editor" with essential formatting
- Implement a simple chat window with a single model
- Once stable, layer in advanced features

### 7.4 Use Modular Components

- Keep each piece decoupled: Chat Pane is a component, Editor is another, the Split Pane is a wrapper, etc.

### 7.5 Plan for Undo/Redo

- Both for text editing and for the AI's suggestions
