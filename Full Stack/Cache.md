There are 3 type of cache
- **Session**
- **Local Storage**
- **Cookie Storage**
**To store local storage**
- localStorage.setItem('key', 'value')
- localStorage.getItem('key')
- localStorage.removeItem()
- localStorage.clear()
**To store Session storage**
- sessionStorage.setItem('key', 'value')
- sessionStorage.getItem('key')
- sessionStorage.removeItem()
- sessionStorage.clear()
**To store Cookie storage**
- document.cookie = 'name=kyle; expires=' + new Date(9999, 0, 1).toUTCString()


**When To Store**
**Local storage** :
- User preferences (theme, language, layout) • Feature flags 
- Onboarding state (has seen tutorial?) • Shopping cart (for guests) • 
- Recently viewed items • 
- Non-sensitive form drafts • 
- JWT token (only if using refresh tokens properly) • 
- Large client-side caches (e.g., images as base64, if necessary)
**Session storage**
- Form data during multi-step wizards (per tab) • 
- Temporary unsaved changes in a tab • 
- One-time flash messages • 
- CSRF tokens (sometimes, though cookies are more common) • 
- Tab-specific state (e.g., scroll position in a specific flow)
**Cookie Storage**
- Authentication session tokens (HttpOnly, Secure, SameSite) • 
- CSRF tokens (if double-submit pattern) • 
- Tracking IDs (analytics, ads) • 
- Essential preferences that server needs (e.g., ab-test group) • 
- Small user identifiers