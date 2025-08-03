<script lang="ts">
import { onMount } from "svelte";
import axios from "axios";
import { API_HOST } from "../constants";
export let chatId: string;

let messages: { message: string, type: string, createdAt: string }[] = [];
let newMessage = "";
let errorMessage: string | null = null;
let isLoading = false;

onMount(async () => {
  await loadMessages();
});

async function loadMessages() {
  try {
    const response = await axios.get(
      `${API_HOST}/api/v1/chat/${chatId}/message/`
    );
    messages = response.data.data;
    messages = messages.map(
      m => {return {...m, createdAt: m.createdAt.replace(' ', 'T') + 'Z'}}
    );
  } catch (error) {
    errorMessage = "メッセージの取得に失敗しました";
  }
}

async function sendMessage() {
  isLoading = true;
  try {
    const response = await axios.post(
      `${API_HOST}/api/v1/chat/${chatId}/message/`,
      { message: newMessage }
    );
    let createdAtISO = response.data.data.createdAt.replace(' ', 'T') + 'Z';
    response.data.data.createdAt = createdAtISO;
    let sentDate = new Date(Date.now()).toISOString();
    let sentMessage = {message: newMessage, type: "user", createdAt: sentDate};
    messages = [...messages, sentMessage, response.data.data];
    newMessage = "";
  } catch (error) {
    errorMessage = "メッセージの送信に失敗しました";
  }
  isLoading = false;
}

$: {
  if (chatId) {
    loadMessages();
  }
}
</script>

<div class="message-list">
  {#if errorMessage}
    <div class="error">{errorMessage}</div>
  {/if}
  <ul>
    {#each messages as message}
      <li class:user={message.type === "user"}>
        {message.message}
        <span>{new Date(message.createdAt).toLocaleString("ja-JP")}</span>
      </li>
    {/each}
  </ul>
<textarea bind:value={newMessage} placeholder="メッセージ"></textarea>
<button onclick={sendMessage} disabled={isLoading}>
  {#if isLoading}
    送信中
  {:else}
    送信
  {/if}
</button>
</div>

<style>
.message-list {
  padding: 0;
}

ul {
  list-style: none;
  margin: 0;
  padding: 0;
}

li {
  background-color: #311;
  margin: 5px 0;
  padding: 10px;
}

li.user {
  background-color: #113;
}

textarea {
  border: 1px solid #ccc;
  box-sizing: border-box;
  margin-bottom: 10px;
  padding: 10px;
  width: 100%;
}

button {
  width: 100%;
}

button:hover {
  background-color: #48c;
  color: white;
}



</style>

