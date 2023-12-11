<template>
  <div class="hello">
    <button @click="login">Login with Google</button>
  </div>
</template>

<script setup>
import { defineProps } from 'vue';
import { createOidcAuth, SignInType } from 'vue-oidc-client/vue3';

defineProps({ msg: String });

// note the ending '/'
const appUrl = 'https://mydomain.com/myapp/';

const authCallbackPath = '/index.html?auth-callback=1';
const logoutCallbackPath = '/index.html?logout-callback=1';
const scopes = 'offline_access';
const backendUri = 'https://localhost:58374'
const redirectUri = `http://localhost:5500${authCallbackPath}`;
const logoutRedirectUri = `${backendUri}${logoutCallbackPath}`;
const clientId = 'ipos_webapp';


const clientSettings = {
  authority: backendUri,
  client_id: clientId,
  redirect_uri: redirectUri,
  post_logout_redirect_uri: logoutRedirectUri,
  response_type: 'code',
  filterProtocolClaims: true,
  loadUserInfo: false,
  scope: scopes,
  extraTokenParams: { scope: scopes },
};

// SignInType could be Window or Popup
var mainOidc = createOidcAuth('main', SignInType.Popup, appUrl, clientSettings);

const login = () => {

  mainOidc.signIn();

};

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}
</style>
