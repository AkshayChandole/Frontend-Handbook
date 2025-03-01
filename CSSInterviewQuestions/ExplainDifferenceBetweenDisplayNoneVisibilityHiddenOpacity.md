# [Explain difference between: display: none, visibility: hidden, opacity: 0 ?](#explain-difference-between-display-none-visibility-hidden-opacity)

### Question:
What is the difference between display: none, visibility: hidden, and opacity: 0 in CSS?

### Answer:  
Each of these CSS properties controls the visibility of an element, but they behave differently:  

1. **`display: none;`**  
   - The element is completely removed from the document flow.  
   - It does not take up space on the page.  
   - Other elements are positioned as if the element doesn’t exist.  
   - Example:  
     ```css
     .hidden {
       display: none;
     }
     ```

2. **`visibility: hidden;`**  
   - The element remains in the document flow but becomes invisible.  
   - It still occupies space, affecting the layout of other elements.  
   - Example:  
     ```css
     .invisible {
       visibility: hidden;
     }
     ```

3. **`opacity: 0;`**  
   - The element is fully transparent but still exists in the document flow.  
   - It occupies space and can still be interacted with (e.g., clicked on).  
   - Example:  
     ```css
     .transparent {
       opacity: 0;
     }
     ```

### Code Example:
![image](https://github.com/user-attachments/assets/fe766e6b-8301-437a-a2a9-da1e4186496f)


### Key Differences:  

| Property            | Visible? | Takes Space? | Affects Layout? | Interactable? |
|---------------------|----------|--------------|----------------|--------------|
| `display: none`    | ❌ No     | ❌ No         | ✅ No          | ❌ No       |
| `visibility: hidden` | ❌ No     | ✅ Yes        | ✅ Yes         | ❌ No       |
| `opacity: 0`       | ❌ No     | ✅ Yes        | ✅ Yes         | ✅ Yes      |

If you want to hide an element while keeping its space, use `visibility: hidden`.  
If you want to remove it completely, use `display: none`.  
If you want it hidden but still interactive, use `opacity: 0`.
