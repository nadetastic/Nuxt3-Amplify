<script setup lang="ts">
import { Authenticator } from "@aws-amplify/ui-vue";
import { withSSRContext } from "aws-amplify";
import "@aws-amplify/ui-vue/styles.css";

const { data } = await useAsyncData('authinfo', async () => {
  const { node: { req } } = useRequestEvent();
  const { Auth } = withSSRContext({ req });
  try {
    const data = await Auth.currentAuthenticatedUser();
    return data;
  } catch(e){
    // user not authenticated or other errors
    return e
  }
});


</script>
<template>
  <p>From SSR: {{ data?.attributes ? data.attributes : data }}</p>
  <hr />
  <Authenticator>
    <template v-slot="{ user, signOut }">
      <h1>Hello {{ user.username }}!</h1>
      <button @click="signOut">Sign Out</button>
    </template>
  </Authenticator>
  <div></div>
</template>
