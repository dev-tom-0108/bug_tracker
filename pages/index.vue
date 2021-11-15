<script lang="ts" setup>
import { useNuxtApp } from "#app";
import Bug from "@/interfaces/Bug";

const title = "Bug Tracker";  

const { data, error } = await useFetch<string, Array<Bug>>(`https://00r1pdndpb.execute-api.eu-central-1.amazonaws.com/dev/reports`, { method: 'POST', body: { reporter: 'anonymous' } })

const bugs: Array<Bug> = data.value;

var index = 0;
bugs.forEach(function(e) {
  if(typeof e === "object") {
    index++;
    e["id"] = index;
  }
});

console.log(bugs)

if (error.value) {
  const { message: errorMsg } = error.value;
  if (errorMsg.includes("404")) {
    console.log("failed to fetch data");
  }
}
</script>

<template>
    <div>
      <h1 class="text-5xl font-bold mb-4">Bugs</h1>
      <p>fetching bugs from api</p>
      <p v-if="!bugs">...loading</p>
      <div v-for="bug of bugs" class="flex text-left">
        <nuxt-link :to="'/bugs/' + bug.id">
          <h3>Bug - {{ bug.id }}</h3>
        </nuxt-link>
      </div>
    </div>
</template>
