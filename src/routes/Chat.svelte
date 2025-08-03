<script lang="ts">
    import {onMount} from "svelte";
    import {goto} from '@mateothegreat/svelte5-router';
    import {authToken} from "../stores/auth";
    import ChatList from "../components/ChatList.svelte";
    import MessageList from "../components/MessageList.svelte";
    let { route } = $props();
    let chatId = route.result.path.params ? route.result.path.params.chatId : null;

    function logout() {
        authToken.remove();
        goto('/login');
    }

    onMount(()=> {
        if (!$authToken) {
            goto("/login");
        }
    });
</script>
<div>
    <div class="container">
        <div class="sidebar">
            <ChatList chatId={chatId} />
            <button class="logout-button" onclick={logout}>
                ログアウト
            </button>
        </div>
    </div>
</div>

<div class="main">
  {#if chatId}
    <MessageList chatId={chatId}/>
  {/if}
</div>


<style>
    .container {
        display: flex;
        height: 100vh;
    }

    .sidebar {
        height: 100vh;
        overflow-y: auto;
        padding: 0;
        width: 160px;
    }

    .main {
        height: 100vh;
        overflow-y: auto;
        padding: 0 10px;
        width: 540px;
    }


button {
  margin-top: 30px;
  width: 100%;
}

button:hover {
  background-color: #c84;
  border-color: #a40;
  color: white;
}

</style>
