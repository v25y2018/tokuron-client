<script lang="ts">
  import { onMount } from "svelte";
  import { goto } from '@mateothegreat/svelte5-router';
  import axios from "axios";
  import { authToken } from "../stores/auth";
  import {API_HOST} from "../constants";
  let name = "";
  let email = "";
  let password = "";
  let errorMessage = "";

  onMount(() => {
    if ($authToken) {
      goto("/");
    }
  });

  async function register() {
    try {
      await axios.post(`${API_HOST}/api/v1/auth/register/`, {
        name,
        email,
        password,
      });
      goto("/login");
    } catch (error) {
      errorMessage = "An unexpected error occurred"
    }
  }
  $: formValid = email.length > 0 && password.length > 0 && name.length > 0;
</script>

<div class="auth-container">
  <form on:submit|preventDefault={register} class="auth-form">
    <div class="form-header">
      <h2>アカウント登録</h2>
    </div>

    {#if errorMessage}
      <div class="error">{errorMessage}</div>
    {/if}

    <div class="input-group">
      <input type="text" placeholder="名前" bind:value={name} required />
    </div>
    <div class="input-group">
      <input type="email" placeholder="メールアドレス" bind:value={email} required />
    </div>
        <div class="input-group">
      <input
        type="password"
        placeholder="パスワード"
        bind:value={password}
        required
      />
    </div>

    <div class="action-group">
      <button type="submit" class="auth-btn" disabled={!formValid}>サインアップ</button>
    </div>
    <div class="switch-auth">
      既にアカウントを持っていますか？<a href="/login">ログインはこちらから</a>
    </div>
  </form>
</div>

