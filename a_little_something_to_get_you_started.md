## A Little Something to Get You Started (Web CTF Write-up)

### Description

Challenge Name: Trivial - A Little Something to Get You Started  
Category: Web  
Difficulty: Easy  
Objective: Find the hidden flag by analyzing the page source.

### XSS Vulnerabilities Identified

None identified in this challenge.

### Analysis of the Issue

The challenge revolves around inspecting the page source for hidden clues. An unused image file reference (`background.png`) was found, which was not displayed on the page but present in the source code.

### Why Requests Were Blocked

No requests were blocked during this challenge.

### Solution

By manually accessing the hidden image file (`background.png`) via the URL, the flag was revealed inside the image.

### Key Steps

- Inspected the page source for unusual or unused references.
- Found a suspicious image file reference: `background.png`.
- Accessed the image directly by appending it to the site URL.
- Discovered the flag within the image file.

### Flag Extraction

The flag was extracted by visiting `https://example.com/background.png` and analyzing the image contents.

### Lessons Learned

- Always check for hidden media files in the page source.
- Manually test file URLs to uncover hidden content.
- Developer tools are invaluable (`Ctrl + Shift + I` or `F12`).

### Additional Debugging Tips

- Use browser developer tools to inspect all resources loaded by the page.
- Look for unused or commented-out code that may reference hidden files.
- Try accessing files directly if you find suspicious references.

### Conclusion

This challenge was a great introduction to web-based CTFs, emphasizing the importance of understanding web page structure and source code analysis. Developers may leave hints in the source that can lead to hidden content.

---

💀 🏴 HAPPY HACKING!

