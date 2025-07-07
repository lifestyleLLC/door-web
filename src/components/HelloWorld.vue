<script setup lang="ts">

defineProps<{ msg: string }>()

</script>

<template>
  <h1> {{ msg }} </h1>

  <button @click="open">开</button>

  <div v-if="response">
    <pre>{{ response }}</pre>
  </div>
  <div v-if="error">
    <p style="color: red;">{{ error }}</p>
  </div>
</template>


<script lang="ts">
export default {
  name: 'PostRequestForm',
  data() {
    return {
      formData: {
        password: '7a',
      },
      loading: false,
      response: null,
      error: "",
    };
  },
  methods: {
    async open() {
      this.loading = true;
      this.response = null;
      this.error = "";

      try {
        const res = await fetch('https://main.d32fsrln6io36p.amplifyapp.com/api/legacy', {
          method: 'POST',
          body: JSON.stringify(this.formData),
        });

        if (!res.ok) {
          throw new Error(`HTTP error! status: ${res.status}`);
        }

        const data = await res.json();
        this.response = data;
      } catch (err) {
        const errorObject = err as Error;
        this.error = errorObject.message;
      } finally {
        this.loading = false;
      }
    },
  },
};
</script>
