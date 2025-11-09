# Implementation Plan

- [x] 1. Create base HTML structure with Tailwind CSS integration
  - Set up single HTML file with proper DOCTYPE and meta tags
  - Include Tailwind CSS via CDN
  - Add Google Fonts for spooky typography (Creepster/Nosifer for headers, Roboto for body)
  - Define custom CSS variables for color palette in style block
  - Create basic semantic HTML structure for chat container, theme selection, and party plan sections
  - _Requirements: 1.1, 6.1, 6.2, 6.3, 8.1, 8.2_

- [x] 2. Implement state management system
  - Create appState object with conversationHistory, currentPhase, selectedTheme, partyPlan, and isAIResponding properties
  - Implement addMessage() function to append messages to conversation history
  - Implement setPhase() function to update current interaction phase
  - Implement setTheme() function to store selected theme
  - Implement setPlan() function to store generated party plan
  - Implement getConversationContext() function to format history for LLM API calls
  - _Requirements: 1.3, 10.1, 10.3, 10.4_

- [x] 3. Build chat interface UI and message rendering
  - Create chat container with messages area and input area
  - Implement renderMessage() function to create and append message bubbles
  - Style user messages (right-aligned) and AI messages (left-aligned) with distinct visual treatment
  - Implement auto-scroll functionality to show latest messages
  - Add flickering candle animation effect to chat container
  - Apply responsive styling for mobile and desktop layouts
  - _Requirements: 1.1, 1.2, 1.4, 6.1, 6.2, 6.3, 6.4, 7.1, 8.1, 8.2, 8.3_

- [x] 4. Create chat input handling and user interaction
  - Implement event listener for send button click
  - Implement event listener for Enter key press in input field
  - Add input validation to disable send button when input is empty
  - Clear input field after message is sent
  - Call addMessage() to store user input in state
  - Trigger LLM API call with user message
  - _Requirements: 1.3, 1.4_

- [x] 5. Implement LLM API client and integration
  - Create buildSystemPrompt() function that returns phase-specific instructions
  - Create sendToLLM() function that constructs API request with conversation history
  - Include fetch API call to OpenAI-compatible endpoint
  - Handle API response and extract assistant message content
  - Implement error handling for network failures and invalid responses
  - Add API key configuration (prompt user or environment variable)
  - _Requirements: 9.1, 9.2, 9.3, 9.4, 9.5, 10.2_

- [x] 6. Build typing indicator animation
  - Create typing indicator HTML element with bubbling cauldron animation
  - Implement showTypingIndicator() function to display indicator
  - Implement hideTypingIndicator() function to remove indicator
  - Add CSS keyframe animation for bubbling effect
  - Call showTypingIndicator() when API request starts
  - Call hideTypingIndicator() when API response is received
  - _Requirements: 7.2, 7.4_

- [x] 7. Implement theme selection component
  - Create theme-options container with two theme-card elements
  - Implement showThemeOptions() function to populate theme cards with name and description
  - Add event listeners to "Choose This Theme" buttons
  - Implement theme selection by button click (store in state, hide options, show confirmation)
  - Implement theme selection by typed theme name (parse user input, match to theme, store in state)
  - Add animation for theme cards appearance
  - Style theme cards with spooky aesthetic and hover effects
  - _Requirements: 2.1, 2.2, 2.3, 2.4, 2.5, 7.3_

- [x] 8. Create theme refinement handling
  - Detect refinement requests in user input during appropriate phases
  - Pass refinement context to LLM via conversation history
  - Update UI to acknowledge refinement
  - Allow multiple refinement iterations before finalizing theme
  - _Requirements: 3.1, 3.2, 3.3, 3.4, 3.5_

- [x] 9. Build party plan display component
  - Create party-plan container with sections for decorations, food/drinks, music, and activities
  - Implement renderPartyPlan() function to parse LLM response into structured data
  - Populate each section with appropriate content (lists for decorations/food/activities, paragraph for music)
  - Show party plan component when plan is generated
  - Position party plan outside chat flow (separate section or fixed position)
  - Apply responsive styling to prevent horizontal scrolling on mobile and desktop
  - _Requirements: 4.1, 4.2, 4.3, 4.4, 4.5, 4.6, 5.1, 5.2, 5.3_

- [x] 10. Implement copy plan functionality
  - Add "Copy Plan" button to party plan component
  - Implement copyPlanToClipboard() function using Clipboard API
  - Format party plan as plain text for clipboard
  - Show visual feedback when copy is successful
  - Handle clipboard API errors gracefully
  - _Requirements: 5.4_

- [x] 11. Add button hover animations and interactive effects
  - Apply hover state animations to all interactive buttons
  - Use scale transform and glowing box-shadow on hover
  - Add smooth transitions for hover effects
  - Ensure animations don't interfere with usability
  - _Requirements: 7.3, 7.4_

- [x] 12. Implement conversation flow orchestration
  - Create initial welcome message on page load
  - Implement phase transitions: initial → theme-selection → refinement → plan-generated
  - Build phase-specific system prompts for LLM
  - Ensure LLM generates exactly 2 themes in theme-selection phase
  - Ensure LLM generates structured plan with correct item counts in plan-generated phase
  - Handle user confirmation to proceed from refinement to plan generation
  - _Requirements: 1.2, 2.1, 3.2, 3.3, 4.1, 4.2, 4.3, 4.4, 4.5, 4.6, 9.2, 9.3, 9.4_

- [x] 13. Add error handling and user feedback
  - Display user-friendly error messages for API failures ("The spirits are restless...")
  - Implement retry logic for failed API requests
  - Show rate limiting message when appropriate ("The cauldron is bubbling...")
  - Handle missing API key with configuration prompt
  - Provide fallback messages if LLM response is invalid
  - _Requirements: 1.1, 1.2_

- [x] 14. Optimize responsive layout and mobile experience
  - Test and adjust layout at 375px (mobile), 768px (tablet), 1440px (desktop)
  - Ensure chat window occupies majority of screen space on mobile
  - Verify all interactive elements are accessible on touch devices
  - Test party plan readability on all screen sizes
  - Adjust font sizes and spacing for mobile readability
  - _Requirements: 8.1, 8.2, 8.3, 8.4_

- [x] 15. Polish visual design and animations
  - Fine-tune color palette application across all components
  - Verify typography hierarchy and readability
  - Adjust animation timing and intensity for optimal user experience
  - Add subtle textures or background effects for Halloween atmosphere
  - Ensure consistent spacing and alignment throughout interface
  - Test reduced-motion preferences for accessibility
  - _Requirements: 6.1, 6.2, 6.3, 6.4, 7.1, 7.2, 7.3, 7.4_
