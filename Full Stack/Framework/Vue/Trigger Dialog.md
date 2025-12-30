To trigger dialog 
```javascript
<template>
  <div>
    <v-btn color="primary" @click="dialog = true">
      Open Dialog (Script Setup)
    </v-btn>

    <v-dialog v-model="dialog" max-width="500">
      <v-card>
        <v-card-title class="headline">✨ Modern Dialog Title</v-card-title>
        <v-card-text>
          This dialog is controlled using Vue 3's simpler setup syntax.
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="red darken-1" text @click="dialog = false">
            Close
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </div>
</template>

<script setup>
import { ref } from 'vue'
// The `ref` function makes 'dialog' a reactive state variable.
// Its value starts at false (closed).
const dialog = ref(false)

// Note: In the template, you don't need to use 'dialog.value', 
// Vue automatically unwraps the ref for you!
</script>
```