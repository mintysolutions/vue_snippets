# Vue Code Examples

## Vuetify Example

### Tabs

```vue
<template>
  <v-tabs v-model="activeTab" grow>
    <v-tab :value="1">Choose Slot</v-tab>
    <v-tab :value="2">Contacts</v-tab>
    <v-tab :value="3">Confirmation</v-tab>
  </v-tabs>
  <v-tabs-window v-model="activeTab">
    <v-tabs-window-item :value="1" :key="1">
      <!-- Content for slot 1 -->
    </v-tabs-window-item>
    <v-tabs-window-item :value="2" :key="2">
      <!-- Content for slot 2 -->
    </v-tabs-window-item>
    <v-tabs-window-item :value="3" :key="3">
      <!-- Content for slot 3 -->
    </v-tabs-window-item>
  </v-tabs-window>
</template>

<script>
export default {
  data() {
    return {
      activeTab: 1
    };
  }
};
</script>
```
