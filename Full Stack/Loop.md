create a form that user can input and optional add other row, when click button
```javascript
const items = ref([])
const name = ref('')
const description = ref('')
const price = ref(0)

const rowsTotal = 
items.value .filter(row => row.recurrent).reduce((sum, row) => sum+row.quantity * row.rate, 0)
  
 const subtotal = base + rowsTotal
 const tax = subtotal * vatRate.value
  
 return subtotal + tax + Number(formattedAmountWithTenPercentage.value || 0)
  
  })

const addRow = () => {
	items.value.push({
		name: '',
		description: '' 
	}) 
}

const removeRow = index => {
	items.value.splice(index, 1)
}
```
