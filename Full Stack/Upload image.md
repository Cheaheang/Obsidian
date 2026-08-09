to **Peek** in log
```js
const formData = new FormData();
formData.append('name', 'Gaming Mouse');
formData.append('image', fileInput.files[0]);

// To actually SEE what's inside:
for (let [key, value] of formData.entries()) {
  console.log(`${key}:`, value);
}
```