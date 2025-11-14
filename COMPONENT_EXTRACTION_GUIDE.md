# Deep Time Timeline Explorer - Component Extraction Guide

This guide explains how to extract the Deep Time Timeline Explorer as a standalone React component.

## Structure

The code in `TImelineTest.html` is divided into two clear sections:

### 1. Core Timeline Component (Lines 1165-3689)
This is the reusable timeline component logic that can be extracted into a React component.

**Markers:**
- **Start:** `// DEEP TIME TIMELINE EXPLORER - CORE COMPONENT`
- **End:** `// END OF CORE COMPONENT`

### 2. Test Suite (Lines 3690-end)
This is the testing infrastructure that validates the component.

**Markers:**
- **Start:** `// COMPREHENSIVE TEST SUITE`
- Contains test helper functions and test definitions

## Extraction Steps

### Option 1: Extract as React Component

1. **Copy Core Component Code** (between the markers above)
2. **Convert to React Hooks:**
   - Replace `let` variables with `useState`
   - Replace event listeners with React event handlers
   - Replace `document.getElementById` with `useRef`
   - Convert initialization code to `useEffect`

3. **Example Structure:**

```jsx
import React, { useState, useEffect, useRef } from 'react';

export function DeepTimeTimelineExplorer({ 
  initialStartDate, 
  initialEndDate,
  onDateRangeChange 
}) {
  // State management
  const [topStartMinutes, setTopStartMinutes] = useState(0);
  const [topEndMinutes, setTopEndMinutes] = useState(0);
  // ... more state
  
  // Refs for DOM elements
  const topStartHandleRef = useRef(null);
  const topEndHandleRef = useRef(null);
  // ... more refs
  
  // Core logic functions
  function getCurrentDateMinutes() {
    // ... existing logic
  }
  
  function updateRangeSliders() {
    // ... existing logic
  }
  
  // ... all other core functions
  
  // Effects
  useEffect(() => {
    // Initialization logic
    initializeTimelines();
    updateGranularitySlider();
    // ...
  }, []);
  
  useEffect(() => {
    // Call parent callback when dates change
    if (onDateRangeChange) {
      onDateRangeChange({
        start: topStartMinutes,
        end: topEndMinutes
      });
    }
  }, [topStartMinutes, topEndMinutes, onDateRangeChange]);
  
  return (
    <div className="timeline-card">
      {/* Copy HTML structure from TImelineTest.html */}
      {/* BUT remove the test button and test panel */}
      <div className="range-display" ref={rangeDisplayRef}></div>
      {/* ... rest of component JSX */}
    </div>
  );
}
```

### Option 2: Use as Vanilla JS Component

The code can also be packaged as a vanilla JavaScript module:

```javascript
// timeline-explorer.js
export class DeepTimeTimelineExplorer {
  constructor(containerElement, options = {}) {
    this.container = containerElement;
    this.options = options;
    
    // Copy all core component variables and functions
    this.MINUTES_PER_YEAR = 525600;
    // ... etc
    
    this.init();
  }
  
  init() {
    // Copy initialization code from core component
    this.initializeTimelines();
    this.updateGranularitySlider();
    // ...
  }
  
  // Copy all core functions as methods
  getCurrentDateMinutes() { /* ... */ }
  updateRangeSliders() { /* ... */ }
  // ...
  
  // Public API
  getDateRange() {
    return {
      start: this.topStartMinutes,
      end: this.topEndMinutes
    };
  }
  
  setDateRange(start, end) {
    this.topStartMinutes = start;
    this.topEndMinutes = end;
    this.updateRangeSliders();
  }
}
```

## Test Suite

The test suite remains separate and can be used to validate the component:

### Automated Tests (5 tests)
- Past/Future color distinction
- Small date ranges (hours/days)
- Geological time (Mesozoic Era)
- Extreme past (Earth formation ~4.5Ga)
- Future navigation

### Manual Tests (13 tests)
- Initial state verification
- Timeline colors
- Fine slider interaction
- Coarse slider interaction
- Vertical date adjustment
- Speed control
- Granularity control
- Granularity impact
- Earth view toggle
- Geological tooltips
- Date input editing
- Handle separation constraints
- Slider synchronization

## Usage Example

```jsx
import { DeepTimeTimelineExplorer } from './DeepTimeTimelineExplorer';

function App() {
  const handleDateChange = (range) => {
    console.log('Date range changed:', range);
  };
  
  return (
    <div>
      <h1>My Application</h1>
      <DeepTimeTimelineExplorer 
        initialStartDate={new Date()}
        initialEndDate={new Date(Date.now() + 86400000)}
        onDateRangeChange={handleDateChange}
      />
    </div>
  );
}
```

## Dependencies

The component has no external dependencies - it uses only vanilla JavaScript and browser APIs.

## Styling

Copy all CSS from the `<style>` section in `TImelineTest.html`, excluding the test-related styles:
- `.test-button-container`
- `.test-button`
- `.test-panel`
- `.test-panel-*`

## Notes

- The component is fully self-contained
- No build tools required (though you may want to use them for optimization)
- Works with any framework or vanilla JS
- The test suite can be run separately to validate integration
