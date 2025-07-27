<script lang="ts">
import { onMount } from "svelte";
import { goto } from '@mateothegreat/svelte5-router';
import axios from "axios";
import { authToken } from "../stores/auth";
import { API_HOST } from "../constants";

let email = "";
let password = "";
let errorMessage = "";
$: formValid = email.length > 0 && password.length > 0;

onMount(() => {
    if ($authToken) {
        goto("/");
    }
});

async function login() {
  try {
    const response = await axios.post(`${API_HOST}/api/v1/auth/login/`, {
      email,
      password,
    });
    authToken.set(response.data?.token);
    goto("/");
  } catch (error) {
    errorMessage = "An unexpected error occurred"
  }
}
</script>

<div class="auth-container">
  <form on:submit|preventDefault={login} class="auth-form">
    <div class="form-header">
      <h2>ログイン</h2>
    </div>

    {#if errorMessage}
      <div class="error">{errorMessage}</div>
    {/if}

    <div class="input-group">
      <input type="email" placeholder="メールアドレス" bind:value={email} required />
    </div>
    <div class="input-group">
      <input
        type="password"
        placeholder="Password"
        bind:value={password}
        required
      />
    </div>

    <div class="action-group">
      <button type="submit" class="auth-btn" disabled={!formValid}>ログイン</button>
    </div>
    <div class="switch-auth">
      アカウントを持っていないですか？<a href="/register">登録はこちらから</a>。
    </div>
  </form>
</div>
