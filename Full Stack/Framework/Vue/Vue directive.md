There are 2 vue directive:
- v-once: This element only rendering once, then all the child of that component will be static component 
**Fun fact:**
	- when using v-once, even you put v-if it did not check the condition
- v-memo: This element use to store previous renders to improve performance 
**Fun fact:**
	-v-memo don't work under for loop, but it work if you put it in the element of for loop