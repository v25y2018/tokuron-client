<script>
import { Router } from '@mateothegreat/svelte5-router';
import Register from "./routes/Register.svelte";
import Login from './routes/Login.svelte';
import Chat from "./routes/Chat.svelte";

const routes = [
  {
    name: 'register',
    path: '/register',
    component: Register,
  },
  {
    name: 'login',
    path: '/login',
    component: Login,
  },
  {
    name: 'chat-detail',
    path: '/(?<chatId>[1-9][0-9]*)',
    component: Chat,
  },
  {
    name: 'chat',
    path: '/',
    component: Chat
  }
];
</script>

<Router {routes} />