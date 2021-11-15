<script lang="ts" setup>
import Bug from "@/interfaces/Bug";

const { params } = useRoute();
const router = useRouter();

// specify the return type as an array of bugs
const { data, error } = await useFetch<string, Array<Bug>>(
  `https://00r1pdndpb.execute-api.eu-central-1.amazonaws.com/dev/reports/${params.id}`, 
  { method: 'POST', body: { reporter: 'anonymous' } }
);

const bug: Array<Bug> = data.value; // reactive

var index = 0;
bugs.forEach(function(e) {
  if(typeof e === "object") {
    index++;
    e["id"] = index;
  }
});

// redirect to a custom 404.vue page
if (!bug) {
  router.push({
    path: "/404",
  });
}
</script>

<template>
  <div v-if="bug" class="todo">
    <h1 class="text-5xl font-bold mb-8">Bug #{{ $route.params.id }}</h1>
    <pre>{{ bug }}</pre>
    <pre>
    <ul>
      <li v-for="(value, key) in bug">{{ key }}: {{ value }}</li>
    </ul>
    </pre>
  </div>
</template>

<style>
pre {
  width: 100%;
  white-space: pre-wrap;
  word-wrap: break-word;
}
</style>
